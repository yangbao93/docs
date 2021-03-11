在工作中遇到了一个问题，在对比两个JSON的时候，使用到了JsonNode。但是在内容输出的时候遇到了问题。



## 问题

原始的json举例如下：

```json
{
  "strKey":"value",
  "intKey":1
}
```

当我们使用JsonNode输出**strKey**的时候，结果竟然不是**"value"**而是**""value""**

```java
String jsonStr = "{\"strKey\":\"value\",\"intKey\":1}";
ObjectMapper mapper = new ObjectMapper();
JsonNode source = mapper.readTree(jsonStr);
source.get("strKey").toString();
```

## 原因

toString()方法的原因

```java
// JsonNode是一个抽象类
public abstract class JsonNode
    extends JsonSerializable.Base // i.e. implements JsonSerializable
    implements TreeNode, Iterable<JsonNode>
{
  ...

  @Override
  public abstract String toString();
}
```

如上代码所见，JsonNode是一个抽象类，其中的toString()方法 是交给他的子类去实现的。那么我们在进行toString的时候，去调用了什么子类去实现呢？ **去使用了JsonNode的TextNode子类，用了它的toString()方法**

我们查询源码会发现：

1. mapper.readTree() 的源码 主要如下

```java
public JsonNode readTree(String content)
    throws IOException, JsonProcessingException
{
    JsonNode n = (JsonNode) _readMapAndClose(_jsonFactory.createParser(content), JSON_NODE_TYPE);
    return (n == null) ? NullNode.instance : n;
}
```

其中关键是执行了_readMapAndClose方法，这个方法中关键源码如下：

```java
 protected Object _readMapAndClose(JsonParser p0, JavaType valueType)
        throws IOException
 {
   ...
 	 JsonDeserializer<Object> deser = _findRootDeserializer(ctxt, valueType);
   if (cfg.useRootWrapping()) {
        result = _unwrapAndDeserialize(p, ctxt, cfg, valueType, deser);
   } else {
       // 主要的步骤 
        result = deser.deserialize(p, ctxt);
   }
   ...
 }
```

找到JsonNodeDeserializer下的deserialize方法

```java
public JsonNode deserialize(JsonParser p, DeserializationContext ctxt) throws IOException
{
    switch (p.getCurrentTokenId()) {
    case JsonTokenId.ID_START_OBJECT: // 执行此步骤
        return deserializeObject(p, ctxt, ctxt.getNodeFactory());
    case JsonTokenId.ID_START_ARRAY:
        return deserializeArray(p, ctxt, ctxt.getNodeFactory());
    default:
        return deserializeAny(p, ctxt, ctxt.getNodeFactory());
    }
}
```

本文中的例子，会调用deserializeObject方法，这个方法中我们看到，

1. 首先会生成一个ObjectNode；
2. 根据JsonParser中的id信息，将对应的field信息生成出对应的具体JsonNode实现，如TextNode、IntNode等；
3. 将对应的node，按照fieldName，Node，放入在ObjectNode中的类成员变量_children中，protected final Map<String, JsonNode> _children;

```java
protected final ObjectNode deserializeObject(JsonParser p, DeserializationContext ctxt,
        final JsonNodeFactory nodeFactory) throws IOException
{
		// 生成一个ObjectNode
    ObjectNode node = nodeFactory.objectNode();
    String key;
    if (p.isExpectedStartObjectToken()) {
        key = p.nextFieldName();
    } else {
        JsonToken t = p.getCurrentToken();
        if (t == JsonToken.END_OBJECT) {
            return node;
        }
        if (t != JsonToken.FIELD_NAME) {
            return (ObjectNode) ctxt.handleUnexpectedToken(handledType(), p);
        }
        key = p.getCurrentName();
    }
    for (; key != null; key = p.nextFieldName()) {
        JsonNode value;
        JsonToken t = p.nextToken();
        if (t == null) {
            throw ctxt.mappingException("Unexpected end-of-input when binding data into ObjectNode");
        }
        switch (t.id()) {
        case JsonTokenId.ID_START_OBJECT:
            value = deserializeObject(p, ctxt, nodeFactory);
            break;
        case JsonTokenId.ID_START_ARRAY:
            value = deserializeArray(p, ctxt, nodeFactory);
            break;
        case JsonTokenId.ID_EMBEDDED_OBJECT:
            value = _fromEmbedded(p, ctxt, nodeFactory);
            break;
        case JsonTokenId.ID_STRING: // 根据JsonParser中的id信息，利用nodeFactory生成对应的node
            value = nodeFactory.textNode(p.getText());
            break;
        case JsonTokenId.ID_NUMBER_INT:
            value = _fromInt(p, ctxt, nodeFactory);
            break;
        case JsonTokenId.ID_TRUE:
            value = nodeFactory.booleanNode(true);
            break;
        case JsonTokenId.ID_FALSE:
            value = nodeFactory.booleanNode(false);
            break;
        case JsonTokenId.ID_NULL:
            value = nodeFactory.nullNode();
            break;
        default:
            value = deserializeAny(p, ctxt, nodeFactory);
        }
        JsonNode old = node.replace(key, value);
        if (old != null) {
            _handleDuplicateField(p, ctxt, nodeFactory,
                    key, node, old, value);
        }
    }
    return node;
}
```

### 结论

1. ObjectMapper 在readTree的时候，会将文本信息转化为一个JsonParser；
2. JsonParser中会将每个关键的field信息存放，如名称、内容、类别（开始结束符号？字符串？数字？对象）；
3. ObjectMapper._readMapAndClose方法，会生成一个ObjectNode，并根据JsonParser中的信息，将每个field按照类别生成出对应的Node实例，以Map<String, JsonNode>放在 _children中；
4. 我们在使用ObjectNode.get("strKey")方法时候，会取出对应的实例，然后执行其中的方法；这就造成了，我们执行get("strKey").toString()代码时候，执行了TextNode中的toString()方法；输出了额外的引号；

### toString()方法

然后 **TextNode**继承实现了ValueNode

```java
public class TextNode
    extends ValueNode
{
  ...
	/**
    * Different from other values, Strings need quoting
  */
   @Override
  public String toString()
    {
      int len = _value.length();
      len = len + 2 + (len >> 4);
      StringBuilder sb = new StringBuilder(len);
      appendQuoted(sb, _value);
      return sb.toString();
    }

  protected static void appendQuoted(StringBuilder sb, String content)
  {
    sb.append('"');
    CharTypes.appendQuoted(sb, content);
    sb.append('"');
  }
}
```

我们发现在**TextNode**中，实现toString()方法的时候，对值进行了"的包裹，所以导致了上述问题的产生；

**如何避免这个问题呢？**

### asText()方法

```java
@Override
public String asText() {
    return _value;
}
```

使用asText方法即可，这个方法会直接返回value的值，不会对其进行任何的包裹。
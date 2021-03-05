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

如上代码所见，JsonNode是一个抽象类，其中的toString()方法 是交给他的子类去实现的。那么我们在进行toString的时候，去调用了什么子类去实现呢？

首先是我们的**BaseJsonNode**继承并实现了JsonNode

```java
public abstract class BaseJsonNode
    extends JsonNode
    implements JsonSerializable
{...}
```

然后是**ValueNode**继承实现了BaseJsonNode

```java
public abstract class ValueNode
    extends BaseJsonNode
{...}
```

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

```java
@Override
public String asText() {
    return _value;
}
```

使用asText方法即可，这个方法会直接返回value的值，不会对其进行任何的包裹。
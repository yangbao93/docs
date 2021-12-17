# Mather类中的匹配方法Mather.find()与Mather.mathes()的区别

## 什么是Mather

mather的包来自java.util.regex.Matcher，我们常用于正则表达式去匹配文本
举例（正则匹配要求数字0-9至少一个）：

```java
    String reg = "[0-9]+";
    Pattern compile = Pattern.compile(reg);
    Matcher mt = compile.matcher("abc123");
```

针对上述的匹配规则find和mathes的结果不一样

### mt.find()

```java
    System.out.println(mt.find());
    //true
```

### mt.matches()

```java
    System.out.println(mt.matches()); 
    // false
```

## 差异的原因

matches是针对整个字符串做匹配，比如上述的正则表达式中，要求是数字一个或多个。但传入的是abc123，针对整个字符串来说，并不能满足配的规则，因为是含有字母的。当我们把匹配规则修改为：[a-z]+[0-9]+，这时候就matches就为true了

find方法是将整个字符串进行拆分，拆分成若干个子字符串，只要子字符串任一个匹配就返回true。比如传入了abc123，就会拆分成abc、123，此时123匹配上了，所以就返回了true

我们可以看一下find方法的源码：

```java
    public boolean find() {
        int nextSearchIndex = last;
        if (nextSearchIndex == first)
            nextSearchIndex++;

        // If next search starts before region, start it at region
        if (nextSearchIndex < from)
            nextSearchIndex = from;

        // If next search starts beyond region then it fails
        if (nextSearchIndex > to) {
            for (int i = 0; i < groups.length; i++)
                groups[i] = -1;
            return false;
        }
        return search(nextSearchIndex);
    }
```

此时我们修改一下方法来验证猜想

```java
    // 我们输出find方法匹配的字符串
    while (mt.find()) {
            String matchStr = mt.group();
            System.out.println(matchStr);
    }
    // 输出 123
```

## find方法可能会带来的问题？

### find超时问题

针对于字符串的匹配，我们想要提取有用的信息，matches会稍微不满足情况。我们需要使用find方法。在我工作中，因为需要拆分地址中的楼层信息，此时使用的一个正则表达式就出现了超时问题。

具体情况如：

```java
    // 正则表达式
    String reg = "((第{0,1}[0-9]+[、,]{0,1})+[层楼]$)";
    // 需要匹配的字符串
    String address = "超级无敌广场奥特曼展层2001,2002,2003,2004,2072,2073,2074,2075,1004商户";
    // 此时因为字符串比较长，导致运行超时情况 cost time 57s 
```

此时，为防止线上业务出现异常，所以需要对这种情况处理
处理方案：
    1. 根据正则的规则，对满足情况的数据才进行处理；（即 以层楼结尾的数据才进行处理）
    2. 对数据进行截取，只要字符在一定的数量内，不会出现超时；（隐患 可能因为截取导致原本可以匹配的数据无法匹配上）
    3. 对此方法进行异步处理，当处理时间超过的限度，则跳过；（隐患 线上的数据会因超长导致不会被更新）
    4. 离线任务方式，通过离线任务处理好，在主流程中只需要取匹配好的结果即可；（需要额外的保证）

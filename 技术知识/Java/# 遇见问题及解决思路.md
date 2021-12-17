# 遇见问题及解决思路

ClassNotFound问题 1. 查询NotFound文件是否存在。 2. 不存在--&gt;添加 3. 存在更新代码--&gt;更新maven--&gt;clean--&gt;publish--&gt;运行 Error configuring application listener of class _\*_ 1. 包问题 2. 配置问题 3. tomcat缓存问题，clean--&gt;运行 数据库查询不到数据问题，resultType，resultMap 1. 首先检查数据库字段和entity中字段是否匹配，如不匹配使用resultMap进行对应。 2. resultType和resultMap不可以在同一语句出现，但是可以在同一xml文件中存在。 resultMap在数据库字段与entity字段不一致时使用。 举例resultMap： id:提高性能；column:数据库中字段；property:entity中字段

读取properties文件数据的方法 1. Properties:

1. Properties prop = new Properties\(\);
2. FileInputStream in = new FileInputSteam\("路径"\);
3. prop.load\(in\);
4. in.close;
5. ResourceBundle:只能读取classpath路径下的文件
6. 7. ResourceBundle rb = ResourceBundle.getBundle\("emailsetting"\);
8. re.getString\(key\);
9. 关于applicationContext.xml文件的配置

@Autowired @Qualifier\(mailProperties\) private Properties prop;

prop.get\(key\);

关于intValue和parseInt的报错问题

String str1 = "123"; int int1 = Integer.ParseInt\(str1\); int int2 = Integer.valueOf\(str1\).intValue\(\);

上述代码不会报错，均可以转换成功；

Integer.valueOf\(rb.getString\(EMAIL\_PORT\).toString\(\)\).intValue\(\) Integer.ParseInt\(rb.getString\(EMAIL\_PORT\)\).toString\(\)

上述代码会在Integer.ParseInt 的时候报出错误！

valueOf是返回一个integer，可以使用对象的各种方法，如toString（）等。但是parseInt返回的是一个int类型，是一个基本类型，故会报错。

关于null，“”，new object（），三者的对比，isEmpty ，isBlanK

```text
String str1 = new  String();
String str2 = "";
String str3 = null
```

此段代码中，str1和str2都是赋予了内存空间，但是没有值。或者说值是“”； str3是没有赋予内存空间。 举例：杯子是否有水。str3 ==null是没有杯子，str2，与str1则是杯子中没有水。

isEmpty表示是否为空，String str1 = “ ”（empty为false，isBlank为true）

[http://www.cnblogs.com/xudong-bupt/p/3758136.html](http://www.cnblogs.com/xudong-bupt/p/3758136.html)


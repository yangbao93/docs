# JAVA 中 提取 JSON 字符串中的 KEY 和 VALUE 值，去除JSON中的VALUE值的 前后空格 - deng11342的专栏 - 博客频道 - CSDN.NET

## JAVA 中 提取 JSON 字符串中的 KEY 和 VALUE 值，去除JSON中的VALUE值的 前后空格 - deng11342的专栏 - 博客频道 - CSDN.NET

## [JAVA 中 提取 JSON 字符串中的 KEY 和 VALUE 值，去除JSON中的VALUE值的 前后空格](http://blog.csdn.net/deng11342/article/details/7385131)

. 标签： [json](http://www.csdn.net/tag/json) [java](http://www.csdn.net/tag/java) [iterator](http://www.csdn.net/tag/iterator) [string](http://www.csdn.net/tag/string) [null](http://www.csdn.net/tag/null)  
2012-03-22 22:02 32510人阅读 [评论](http://blog.csdn.net/deng11342/article/details/7385131#comments) \(2\) [收藏](java-zhong-ti-qu-json-zi-fu-chuan-zhong-de-key-he-value-zhi-qu-chu-json-zhong-de-value-zhi-de-qian-h.md) [举报](http://blog.csdn.net/deng11342/article/details/7385131#report) .

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/JAVA%20中%20提取%20JSON%20字符串中的%20KEY%20和%20VALUE%20值，去除JSON中的VALUE值的%20前后空格%20-%20deng11342的专栏%20-%20博客频道%20-%20CSDN.NET/category_icon.jpg) 分类： java _（19）_ ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/JAVA%20中%20提取%20JSON%20字符串中的%20KEY%20和%20VALUE%20值，去除JSON中的VALUE值的%20前后空格%20-%20deng11342的专栏%20-%20博客频道%20-%20CSDN.NET/arrow_triangle%20_down.jpg)

. 版权声明：本文为博主原创文章，未经博主允许不得转载。

**\[html\]** [view plain](http://blog.csdn.net/deng11342/article/details/7385131#) [copy](http://blog.csdn.net/deng11342/article/details/7385131#)

1. JSONObject jobj = JSONObject.fromObject\(conditions == null ? "{}"
2. : conditions\);
3. Iterator it = jobj.keys\(\);
4. String infotype = "FCCS";
5. while \(it.hasNext\(\)\) {
6. String key = it.next\(\).toString\(\);
7. // 将所有的空串去掉
8. if \(StringUtil.getString\(jobj.getString\(key\)\) == null\) {
9. continue;
10. }
11. if\("infoType".equals\(key\)\){ //试剂类型
12. if\("FCCS".equals\(jobj.getString\(key\)\)\){
13. infotype = "FCCS";
14. }else if\("HS".equals\(jobj.getString\(key\)\)\){
15. infotype = "HS";
16. }
17. }

**\[html\]** [view plain](http://blog.csdn.net/deng11342/article/details/7385131#) [copy](http://blog.csdn.net/deng11342/article/details/7385131#)

1. /\*\*
2. * @Title: JsonStrTrim
3. * @author : jsw
4. * @date : 2012-12-7
5. * @time : 上午09:19:18
6. * @Description: 传入string 类型的 json字符串 去处字符串中的属性值的空格
7. * @param jsonStr
8. * @return
9. * @exception:\(异常说明\)
10. \*/
11. public JSONObject JsonStrTrim\(String jsonStr\){
12. JSONObject reagobj = JSONObject.fromObject\(jsonStr\);
13. // 取出 jsonObject 中的字段的值的空格
14. Iterator itt = reagobj.keys\(\);
15. while \(itt.hasNext\(\)\) {
16. String key = itt.next\(\).toString\(\);
17. String value = reagobj.getString\(key\);
18. if\(value == null\){
19. continue ;
20. }else if\("".equals\(value.trim\(\)\)\){
21. continue ;
22. }else{
23. reagobj.put\(key, value.trim\(\)\);
24. }
25. }
26. return reagobj;
27. }
28. /\*\*
29. * @Title: JsonStrTrim
30. * @author : jsw
31. * @date : 2012-12-7
32. * @time : 上午09:21:48
33. * @Description: 传入jsonObject 去除当中的空格
34. * @param jsonStr
35. * @return
36. * @exception:\(异常说明\)
37. \*/
38. public JSONObject JsonStrTrim\(JSONObject jsonStr\){
39. JSONObject reagobj = jsonStr ;
40. // 取出 jsonObject 中的字段的值的空格
41. Iterator itt = reagobj.keys\(\);
42. while \(itt.hasNext\(\)\) {
43. String key = itt.next\(\).toString\(\);
44. String value = reagobj.getString\(key\);
45. if\(value == null\){
46. continue ;
47. }else if\("".equals\(value.trim\(\)\)\){
48. continue ;
49. }else{
50. reagobj.put\(key, value.trim\(\)\);
51. }
52. }
53. return reagobj;
54. }
55. /\*\*
56. * @Title: JsonStrTrim
57. * @author : jsw
58. * @date : 2012-12-7
59. * @time : 上午11:48:59
60. * @Description: 将 jsonarry 的jsonObject 中的value值去处前后空格
61. * @param arr
62. * @return
63. * @exception:\(异常说明\)
64. \*/
65. public JSONArray JsonStrTrim\(JSONArray arr\){
66. if\( arr != null && arr.size\(\) &gt; 0\){
67. for \(int i = 0; i &lt; arr.size\(\); i++\) {
68. JSONObject obj = \(JSONObject\) arr.get\(i\);
69. // 取出 jsonObject 中的字段的值的空格
70. Iterator itt = obj.keys\(\);
71. while \(itt.hasNext\(\)\) {
72. String key = itt.next\(\).toString\(\);
73. String value = obj.getString\(key\);
74. if\(value == null\){
75. continue ;
76. }else if\("".equals\(value.trim\(\)\)\){
77. continue ;
78. }else{
79. obj.put\(key, value.trim\(\)\);
80. }
81. }
82. arr.set\(i, obj \);
83. }
84. }
85. return arr;
86. }

[. 顶 0 踩 2 .](http://blog.csdn.net/deng11342/article/details/7385131#)

* 上一篇 [JAVA 读取 制定路径的 XML 文件 和 获取 服务器路径](http://blog.csdn.net/deng11342/article/details/7381073)
* 下一篇 [JAVA 后台取值 ISO-8859-1 的值](http://blog.csdn.net/deng11342/article/details/7412770)

### 我的同类文章

java _（19）_

* _•_ [java 实现PHP serialize\(\) unserialize接口](http://blog.csdn.net/deng11342/article/details/46840365) 2015-07-11 _阅读_ **1354**
* _•_ [基于 apache 的 commons-net-3.3.jar 的 ftp java代码上传下载文件](http://blog.csdn.net/deng11342/article/details/17009041) 2013-11-28 _阅读_ **6221**
* _•_ [JAVA 中 用quartz 完成定时任务的相关配置](http://blog.csdn.net/deng11342/article/details/8257122) 2012-12-04 _阅读_ **553**
* _•_ [Myeclipse 各种注释配置/时间格式的修改](http://blog.csdn.net/deng11342/article/details/8177437) 2012-11-13 _阅读_ **4889**
* _•_ [JAVA 从项目的 properties 文件中 提取 属性值](http://blog.csdn.net/deng11342/article/details/7543583) 2012-05-07 _阅读_ **793**
* _•_ [JSON 相关封装](http://blog.csdn.net/deng11342/article/details/7439688) 2012-04-09 _阅读_ **281**
* _•_ [身份证规则验证java代码方法 非正则匹配](http://blog.csdn.net/deng11342/article/details/46840339) 2015-07-11 _阅读_ **518**
* _•_ [文件的相当路径](http://blog.csdn.net/deng11342/article/details/9066193) 2013-06-09 _阅读_ **301**
* _•_ [tomcat 热部署问题](http://blog.csdn.net/deng11342/article/details/8249115) 2012-12-02 _阅读_ **2138**
* _•_ [java List 排序 Collections.sort\(\) 对 List 排序](http://blog.csdn.net/deng11342/article/details/7825973) 2012-08-03 _阅读_ **431**
* _•_ [使用JXL生成Excel时发生java.lang.ArrayIndexOutOfBoundsException错误](http://blog.csdn.net/deng11342/article/details/7479136) 2012-04-19 _阅读_ **6681**

  [更多文章](http://blog.csdn.net/deng11342/article/category/904119)

[http://static.blog.csdn.net/images/arrow\_triangle \_down.jpg](http://static.blog.csdn.net/images/arrow_triangle%20_down.jpg)


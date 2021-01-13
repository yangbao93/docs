# JAVA 中 提取 JSON 字符串中的 KEY 和 VALUE 值，去除JSON中的VALUE值的 前后空格 - deng11342的专栏 - 博客频道 - CSDN.NET
# [JAVA 中 提取 JSON 字符串中的 KEY 和 VALUE 值，去除JSON中的VALUE值的 前后空格](http://blog.csdn.net/deng11342/article/details/7385131)

.
标签： [json](http://www.csdn.net/tag/json) [java](http://www.csdn.net/tag/java) [iterator](http://www.csdn.net/tag/iterator) [string](http://www.csdn.net/tag/string) [null](http://www.csdn.net/tag/null)  
2012-03-22 22:02 32510人阅读  [评论](http://blog.csdn.net/deng11342/article/details/7385131#comments) (2)  [收藏](#)  [举报](http://blog.csdn.net/deng11342/article/details/7385131#report) 
.

![](JAVA%20%E4%B8%AD%20%E6%8F%90%E5%8F%96%20JSON%20%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%20KEY%20%E5%92%8C%20VALUE%20%E5%80%BC%EF%BC%8C%E5%8E%BB%E9%99%A4JSON%E4%B8%AD%E7%9A%84VALUE%E5%80%BC%E7%9A%84%20%E5%89%8D%E5%90%8E%E7%A9%BA%E6%A0%BC%20-%20deng11342%E7%9A%84%E4%B8%93%E6%A0%8F%20-%20%E5%8D%9A%E5%AE%A2%E9%A2%91%E9%81%93%20-%20CSDN.NET/category_icon.jpg)
分类：
java *（19）* 
![](JAVA%20%E4%B8%AD%20%E6%8F%90%E5%8F%96%20JSON%20%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%20KEY%20%E5%92%8C%20VALUE%20%E5%80%BC%EF%BC%8C%E5%8E%BB%E9%99%A4JSON%E4%B8%AD%E7%9A%84VALUE%E5%80%BC%E7%9A%84%20%E5%89%8D%E5%90%8E%E7%A9%BA%E6%A0%BC%20-%20deng11342%E7%9A%84%E4%B8%93%E6%A0%8F%20-%20%E5%8D%9A%E5%AE%A2%E9%A2%91%E9%81%93%20-%20CSDN.NET/arrow_triangle%20_down.jpg)

.
版权声明：本文为博主原创文章，未经博主允许不得转载。

**[html]** [view plain](http://blog.csdn.net/deng11342/article/details/7385131#) [copy](http://blog.csdn.net/deng11342/article/details/7385131#)

1. JSONObject jobj = JSONObject.fromObject(conditions == null ? "{}"
2. : conditions);

4. Iterator it = jobj.keys();

6. String infotype = "FCCS";

8. while (it.hasNext()) {

10. String key = it.next().toString();

12. // 将所有的空串去掉
13. if (StringUtil.getString(jobj.getString(key)) == null) {

15. continue;
16. }

18. if("infoType".equals(key)){ //试剂类型
19. if("FCCS".equals(jobj.getString(key))){
20. infotype = "FCCS";
21. }else if("HS".equals(jobj.getString(key))){
22. infotype = "HS";
23. }
24. }

**[html]** [view plain](http://blog.csdn.net/deng11342/article/details/7385131#) [copy](http://blog.csdn.net/deng11342/article/details/7385131#)

1. /**
2. * @Title: JsonStrTrim
3. * @author : jsw
4. * @date : 2012-12-7
5. * @time : 上午09:19:18
6. * @Description: 传入string 类型的 json字符串 去处字符串中的属性值的空格
7. * @param jsonStr
8. * @return
9. * @exception:(异常说明)
10. */
11. public JSONObject JsonStrTrim(String jsonStr){

13. JSONObject reagobj = JSONObject.fromObject(jsonStr);
14. // 取出 jsonObject 中的字段的值的空格
15. Iterator itt = reagobj.keys();

17. while (itt.hasNext()) {

19. String key = itt.next().toString();
20. String value = reagobj.getString(key);

22. if(value == null){
23. continue ;
24. }else if("".equals(value.trim())){
25. continue ;
26. }else{
27. reagobj.put(key, value.trim());
28. }
29. }
30. return reagobj;
31. }

33. /**
34. * @Title: JsonStrTrim
35. * @author : jsw
36. * @date : 2012-12-7
37. * @time : 上午09:21:48
38. * @Description: 传入jsonObject 去除当中的空格
39. * @param jsonStr
40. * @return
41. * @exception:(异常说明)
42. */
43. public JSONObject JsonStrTrim(JSONObject jsonStr){

45. JSONObject reagobj = jsonStr ;
46. // 取出 jsonObject 中的字段的值的空格
47. Iterator itt = reagobj.keys();

49. while (itt.hasNext()) {

51. String key = itt.next().toString();
52. String value = reagobj.getString(key);

54. if(value == null){
55. continue ;
56. }else if("".equals(value.trim())){
57. continue ;
58. }else{
59. reagobj.put(key, value.trim());
60. }
61. }
62. return reagobj;
63. }

66. /**
67. * @Title: JsonStrTrim
68. * @author : jsw
69. * @date : 2012-12-7
70. * @time : 上午11:48:59
71. * @Description: 将 jsonarry 的jsonObject 中的value值去处前后空格
72. * @param arr
73. * @return
74. * @exception:(异常说明)
75. */
76. public JSONArray JsonStrTrim(JSONArray arr){

78. if( arr != null && arr.size() > 0){
79. for (int i = 0; i < arr.size(); i++) {

81. JSONObject obj = (JSONObject) arr.get(i);
82. // 取出 jsonObject 中的字段的值的空格
83. Iterator itt = obj.keys();

85. while (itt.hasNext()) {

87. String key = itt.next().toString();
88. String value = obj.getString(key);

90. if(value == null){
91. continue ;
92. }else if("".equals(value.trim())){
93. continue ;
94. }else{
95. obj.put(key, value.trim());
96. }
97. }
98. arr.set(i, obj );
99. }
100. }
101. return arr;
102. }

[.   顶 0   踩 2  .](http://blog.csdn.net/deng11342/article/details/7385131#) 
 
* 上一篇 [JAVA 读取 制定路径的 XML 文件 和 获取 服务器路径](http://blog.csdn.net/deng11342/article/details/7381073)
* 下一篇 [JAVA 后台取值 ISO-8859-1 的值](http://blog.csdn.net/deng11342/article/details/7412770)

#### 我的同类文章

java *（19）*  
* *•* [java 实现PHP serialize() unserialize接口](http://blog.csdn.net/deng11342/article/details/46840365) 2015-07-11 *阅读* **1354**
* *•* [基于 apache 的 commons-net-3.3.jar 的 ftp java代码上传下载文件](http://blog.csdn.net/deng11342/article/details/17009041) 2013-11-28 *阅读* **6221**
* *•* [JAVA 中 用quartz 完成定时任务的相关配置](http://blog.csdn.net/deng11342/article/details/8257122) 2012-12-04 *阅读* **553**
* *•* [Myeclipse 各种注释配置/时间格式的修改](http://blog.csdn.net/deng11342/article/details/8177437) 2012-11-13 *阅读* **4889**
* *•* [JAVA 从项目的 properties 文件中 提取 属性值](http://blog.csdn.net/deng11342/article/details/7543583) 2012-05-07 *阅读* **793**
* *•* [JSON 相关封装](http://blog.csdn.net/deng11342/article/details/7439688) 2012-04-09 *阅读* **281**

* *•* [身份证规则验证java代码方法 非正则匹配](http://blog.csdn.net/deng11342/article/details/46840339) 2015-07-11 *阅读* **518**
* *•* [文件的相当路径](http://blog.csdn.net/deng11342/article/details/9066193) 2013-06-09 *阅读* **301**
* *•* [tomcat 热部署问题](http://blog.csdn.net/deng11342/article/details/8249115) 2012-12-02 *阅读* **2138**
* *•* [java List 排序 Collections.sort() 对 List 排序](http://blog.csdn.net/deng11342/article/details/7825973) 2012-08-03 *阅读* **431**
* *•* [使用JXL生成Excel时发生java.lang.ArrayIndexOutOfBoundsException错误](http://blog.csdn.net/deng11342/article/details/7479136) 2012-04-19 *阅读* **6681**
  [更多文章](http://blog.csdn.net/deng11342/article/category/904119)

http://static.blog.csdn.net/images/arrow_triangle%20_down.jpg
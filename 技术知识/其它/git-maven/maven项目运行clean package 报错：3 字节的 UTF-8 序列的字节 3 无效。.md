# maven项目运行clean package 报错：3 字节的 UTF-8 序列的字节 3 无效。

maven项目运行clean package命令，但是提示mybatis.xml文件报错：3 字节的 UTF-8 序列的字节 3 无效，自己查看了target文件夹下面的编译后的mybatis文件，里面有些中文都乱码了，所以应该是编码的问题导致的。 方法：以下三种方法中本人用的是第二种，毕竟还是要全局设置编码属性比较合适。 网上搜集的方法有： 方法1、将xml头文件改为GBK编码方式 &lt;?xml version="1.0" encoding="GBK"?&gt;

方法2、使用Maven修改默认格式 （本人使用此方法有效） 1

 2org.apache.maven.plugins 3maven-resources-plugin 4 5UTF-8 6 7

方法3、在pom.xml里加入 1

 2 3UTF-8 4

[https://blog.csdn.net/qq\_33802316/article/details/77934730](https://blog.csdn.net/qq_33802316/article/details/77934730)


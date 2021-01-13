# maven项目运行clean package 报错：3 字节的 UTF-8 序列的字节 3 无效。
maven项目运行clean package命令，但是提示mybatis.xml文件报错：3 字节的 UTF-8 序列的字节 3 无效，自己查看了target文件夹下面的编译后的mybatis文件，里面有些中文都乱码了，所以应该是编码的问题导致的。
方法：以下三种方法中本人用的是第二种，毕竟还是要全局设置编码属性比较合适。
网上搜集的方法有：
方法1、将xml头文件改为GBK编码方式
<?xml version="1.0" encoding="GBK"?>

方法2、使用Maven修改默认格式 （本人使用此方法有效）
1

<plugin>

2

<groupId>org.apache.maven.plugins</groupId>

3

<artifactId>maven-resources-plugin</artifactId>

4

<configuration>

5

<encoding>UTF-8</encoding>

6

</configuration>

7

</plugin>

方法3、在pom.xml里加入
1

<properties>

2

<!-- 设置默认编码 -->

3

<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

4

</properties>

https://blog.csdn.net/qq_33802316/article/details/77934730
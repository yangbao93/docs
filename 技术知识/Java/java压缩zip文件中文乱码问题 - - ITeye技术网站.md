# java压缩zip文件中文乱码问题 - - ITeye技术网站

## [java压缩zip文件中文乱码问题](http://riching.iteye.com/blog/579634)

[Java](http://www.iteye.com/blogs/tag/Java) [Apache](http://www.iteye.com/blogs/tag/Apache) [JDK](http://www.iteye.com/blogs/tag/JDK) [Ant](http://www.iteye.com/blogs/tag/Ant) [C](http://www.iteye.com/blogs/tag/C) .

用java来打包文件生成压缩文件，有两个地方会出现乱码 1、内容的中文乱码问题，这个问题网上很多人给出了解决方法，两种：修改sun的源码；使用开源的类库org.apache.tools.zip.ZipOutputStream和org.apache.tools.zip.ZipEntry，这两个类ant.jar中有，可以下载使用即可，毫无疑问，选择后者更方便 2、压缩文件注释的中文乱码问题：zos.setComment\("中文测试"\);这个问题在网上查了半天没看到有人解释，遂只能自己想办法解决。在自己机器上的工程创建的测试类，没有任何问题，但是在公司的项目中使用一直出现乱码，通过使用设置编码的方法（zos.setEncoding\("gbk"\);）终于发现了问题，测试项目的编码方式为gbk，而公司项目的默认编码是utf-8，所以测试项目没问题而公司的项目中出现了问题。

org.apache.tools.zip.ZipOutputStream默认使用项目的编码方式，理论上讲utf-8也是支持中文的，是在想不通为啥还是乱码，通过setEncoding方法改成gbk即可解决

附上一段压缩文件的代码 Java代码  
![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/java压缩zip文件中文乱码问题%20-%20-%20ITeye技术网站/image_7.png)

1. package com.compress;
2. import java.io.BufferedInputStream;
3. import java.io.BufferedOutputStream;
4. import java.io.DataInputStream;
5. import java.io.File;
6. import java.io.FileInputStream;
7. import java.io.FileOutputStream;
8. import org.apache.tools.zip.ZipEntry;
9. import org.apache.tools.zip.ZipOutputStream;
10. public class CompressEncodingTest {
11. /\*\*
12. * @param args
13. * @throws Exception
14. \*/
15. public static void main\(String\[\] args\) throws Exception {
16. File f = new File\("中文测试.txt"\);
17. ZipOutputStream zos = new ZipOutputStream\(new BufferedOutputStream\(
18. new FileOutputStream\("zipTest.zip"\), 1024\)\);
19. zos.putNextEntry\(new ZipEntry\("中国人.txt"\)\);
20. DataInputStream dis = new DataInputStream\(new BufferedInputStream\(
21. new FileInputStream\(f\)\)\);
22. zos.putNextEntry\(new ZipEntry\(f.getName\(\)\)\);
23. int c;
24. while \(\(c = dis.read\(\)\) != -1\) {
25. zos.write\(c\);
26. }
27. zos.setEncoding\("gbk"\);
28. zos.setComment\("中文测试"\);
29. zos.closeEntry\(\);
30. zos.close\(\);
31. }
32. }

分享到：  
![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/java压缩zip文件中文乱码问题%20-%20-%20ITeye技术网站/image_4.jpeg)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/java压缩zip文件中文乱码问题%20-%20-%20ITeye技术网站/image_3.jpeg)

. [window 修改ip脚本](http://riching.iteye.com/blog/753056) \| [转：java中四种操作xml方式的比较](http://riching.iteye.com/blog/407863)

* 2010-01-25 21:27
* 浏览 13133
* [评论\(6\)](http://riching.iteye.com/blog/579634#comments)
* 论坛回复 / [浏览](http://riching.iteye.com/topic/579634) \(2 / 8426\)
* 分类:[编程语言](http://www.iteye.com/blogs/category/language)
* [相关推荐](http://www.iteye.com/wiki/blog/579634)

### 参考知识库

* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/java压缩zip文件中文乱码问题%20-%20-%20ITeye技术网站/1453169124297_297.jpg)
* * [Java SE知识库](http://lib.csdn.net/base/javase) _12515_ 关注 _\|_ _450_ 收录
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/java压缩zip文件中文乱码问题%20-%20-%20ITeye技术网站/1453701371636_636.jpg)
* * [Java Web知识库](http://lib.csdn.net/base/javaweb) _13154_ 关注 _\|_ _1171_ 收录
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/java压缩zip文件中文乱码问题%20-%20-%20ITeye技术网站/1456818035722_722.jpg)
* * [Java EE知识库](http://lib.csdn.net/base/javaee) _4633_ 关注 _\|_ _622_ 收录
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/java压缩zip文件中文乱码问题%20-%20-%20ITeye技术网站/1458106113412_412.jpg)
* * [JavaScript知识库](http://lib.csdn.net/base/javascript)  _4952_ 关注 _\|_ _1283_ 收录

  .

### 评论

[6 楼 dabing69221](java-ya-suo-zip-wen-jian-zhong-wen-luan-ma-wen-ti-iteye-ji-shu-wang-zhan.md) 2013-12-23 riching 写道 大神你牛逼，受教了，我这种不求甚解的习惯要改，谢谢 dabing69221 写道 楼主说“测试项目的编码方式为gbk，而公司项目的默认编码是utf-8，所以测试项目没问题而公司的项目中出现了问题 ”楼主这么分析也对，但是不全面，下面我给楼主分析一下出错原因： 通过org.apache.tools.zip.ZipOutputStream类的源码，我们看到这么一个方法，没错下面这个方法就是幕后真凶：

protected byte\[\] getBytes\(String name\) throws ZipException { if\(encoding == null\) return name.getBytes\(\); return name.getBytes\(encoding\); UnsupportedEncodingException uee; uee; throw new ZipException\(uee.getMessage\(\)\); } 以上这句代码，就是将我们的文件名的字符变为字节，（这里变为字节的目的就是为了通过字节流把字节写出去） 这里判断了一下encoding是否为空，楼主如果不写encoding的话，就会执行name.getByte\(\)也就是使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。（这里使用平台的默认字符集就是楼主所在项目的编码），假如说这里项目所在编码为UTF-8，那么这里就会自动将文件名的字符转换为UTF-8的字节，然后写入到底层输出流。

当我们双击打开那个压缩的zip文件时，程序就会使用ANSI编码（通常指的是平台的默认编码，例如英文操作系统中是ISO-8859-1，中文系统是GBK）。别忘了，我们写文件的时候使用UTF-8的编码来写文件名的，这个时候-- 奇迹出现了“乱码”，为什么呢？ GBK每个汉字占2个字节，而UTF-8每个汉字占3个字节，它们所占字节数都不一样，乱码是必须的，这下楼主知道为什么你的程序老是乱码了吗？ 呵呵呵

呵呵

5 楼 [riching](http://riching.iteye.com/) 2013-12-02 大神你牛逼，受教了，我这种不求甚解的习惯要改，谢谢 dabing69221 写道 楼主说“测试项目的编码方式为gbk，而公司项目的默认编码是utf-8，所以测试项目没问题而公司的项目中出现了问题 ”楼主这么分析也对，但是不全面，下面我给楼主分析一下出错原因： 通过org.apache.tools.zip.ZipOutputStream类的源码，我们看到这么一个方法，没错下面这个方法就是幕后真凶：

protected byte\[\] getBytes\(String name\) throws ZipException { if\(encoding == null\) return name.getBytes\(\); return name.getBytes\(encoding\); UnsupportedEncodingException uee; uee; throw new ZipException\(uee.getMessage\(\)\); } 以上这句代码，就是将我们的文件名的字符变为字节，（这里变为字节的目的就是为了通过字节流把字节写出去） 这里判断了一下encoding是否为空，楼主如果不写encoding的话，就会执行name.getByte\(\)也就是使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。（这里使用平台的默认字符集就是楼主所在项目的编码），假如说这里项目所在编码为UTF-8，那么这里就会自动将文件名的字符转换为UTF-8的字节，然后写入到底层输出流。

当我们双击打开那个压缩的zip文件时，程序就会使用ANSI编码（通常指的是平台的默认编码，例如英文操作系统中是ISO-8859-1，中文系统是GBK）。别忘了，我们写文件的时候使用UTF-8的编码来写文件名的，这个时候-- 奇迹出现了“乱码”，为什么呢？ GBK每个汉字占2个字节，而UTF-8每个汉字占3个字节，它们所占字节数都不一样，乱码是必须的，这下楼主知道为什么你的程序老是乱码了吗？ 呵呵呵

4 楼 [dabing69221](http://dabing69221.iteye.com/) 2013-12-01 楼主说“测试项目的编码方式为gbk，而公司项目的默认编码是utf-8，所以测试项目没问题而公司的项目中出现了问题 ”楼主这么分析也对，但是不全面，下面我给楼主分析一下出错原因： 通过org.apache.tools.zip.ZipOutputStream类的源码，我们看到这么一个方法，没错下面这个方法就是幕后真凶：

protected byte\[\] getBytes\(String name\) throws ZipException { if\(encoding == null\) return name.getBytes\(\); return name.getBytes\(encoding\); UnsupportedEncodingException uee; uee; throw new ZipException\(uee.getMessage\(\)\); } 以上这句代码，就是将我们的文件名的字符变为字节，（这里变为字节的目的就是为了通过字节流把字节写出去） 这里判断了一下encoding是否为空，楼主如果不写encoding的话，就会执行name.getByte\(\)也就是使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。（这里使用平台的默认字符集就是楼主所在项目的编码），假如说这里项目所在编码为UTF-8，那么这里就会自动将文件名的字符转换为UTF-8的字节，然后写入到底层输出流。

当我们双击打开那个压缩的zip文件时，程序就会使用ANSI编码（通常指的是平台的默认编码，例如英文操作系统中是ISO-8859-1，中文系统是GBK）。别忘了，我们写文件的时候使用UTF-8的编码来写文件名的，这个时候-- 奇迹出现了“乱码”，为什么呢？ GBK每个汉字占2个字节，而UTF-8每个汉字占3个字节，它们所占字节数都不一样，乱码是必须的，这下楼主知道为什么你的程序老是乱码了吗？ 呵呵呵

3 楼 [dabing69221](http://dabing69221.iteye.com/) 2013-12-01 楼主，我给你分析一下，你的压缩zip包为什么会乱码？ 分析原因： 我们在使用JDK自带的ZipOutputStream压缩文件时，压缩文件没有采用英文命名时，没有问题。如果采用中文命令就会出现问题，为什么会这样呢？我们来看看ZipOutputStream源码，它到底是怎么把文件名转换为字节的？ private static byte\[\] getUTF8Bytes\(String s\) { char ac\[\] = s.toCharArray\(\); int i = ac.length; int j = 0; for\(int k = 0; k &lt; i; k++\) { char c = ac\[k\]; if\(c &lt;= '\177'\) { j++; continue; } if\(c &lt;= '\u07FF'\) j += 2; else j += 3; }

byte abyte0\[\] = new byte\[j\]; int l = 0; for\(int i1 = 0; i1 &lt; i; i1++\) { char c1 = ac\[i1\]; if\(c1 &lt;= '\177'\) { abyte0\[l++\] = \(byte\)c1; continue; } if\(c1 &lt;= '\u07FF'\) { abyte0\[l++\] = \(byte\)\(c1 &gt;&gt; 6 \| 192\); abyte0\[l++\] = \(byte\)\(c1 & 63 \| 128\); } else { abyte0\[l++\] = \(byte\)\(c1 &gt;&gt; 12 \| 224\); abyte0\[l++\] = \(byte\)\(c1 &gt;&gt; 6 & 63 \| 128\); abyte0\[l++\] = \(byte\)\(c1 & 63 \| 128\); } }

return abyte0; } 以上这个方法就是JDK自带的ZipOutputStream类中将文件名装换为字节的方法，通过源码我们不难看出，它是先将String变为char\[\]数组，然后通过遍历char\[\]数组，最后将char强转为byte,存放到byte数组中。 如果这样做的话，就暴露了一个问题，就是楼主所说的“中文乱码”问题，为什么呢？ 因为在Java中，一个char是2个字节\(byte\)，而一个中文汉字是一个字符，也就是2个字节；英文字母是一个字节，所以，一个英文字母可以保存到 一个字节\(byte\)中，而一个中文汉字不能（道理很简单啊，一个汉字是2个字节） --- 所以说这就是为什么保存英文的文件名是不会乱码，但是保存中文的文件名时会乱码的原因。

2 楼 [riching](http://riching.iteye.com/) 2010-06-06 xiaohahaa 写道 不使用外部包，只用jdk内部的类，能不能实现？

能实现，不过中文的文件名和中文的注释（comment）会乱码，内容没问题

1 楼 [xiaohahaa](http://xiaohahaa.iteye.com/) 2010-06-05 不使用外部包，只用jdk内部的类，能不能实现？

### 发表评论

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/java压缩zip文件中文乱码问题%20-%20-%20ITeye技术网站/image_8.png) [您还没有登录,请您登录后再发表评论](http://riching.iteye.com/login)

[http://img.knowledge.csdn.net/upload/base/1453169124297\_297.jpg](http://img.knowledge.csdn.net/upload/base/1453169124297_297.jpg)


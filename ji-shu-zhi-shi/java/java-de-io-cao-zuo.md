# Java的io操作

## Java的io操作

[Edit](http://maxiang.io/#/?provider=evernote&amp;guid=44df7323-5179-4e59-bb32-9232fa0222b9&amp;notebook=%E6%8A%80%E6%9C%AF%E7%9F%A5%E8%AF%86)

## Java的io操作

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Java的io操作/12-130Q122402I57.jpg)

### 将字符串写入文件

**1.使用PrintStream**

```text
File file = new File(filePath);  PrintStream ps = new PrintStream(new FileOutputStream(file));  ps.println(context);// 往文件里写入字符串  ps.append(context);// 在已有的基础上添加字符串
```

**2.使用BufferWriter**

```text
FileWriter fw = new FileWriter(filePath, true);   BufferedWriter bw = new BufferedWriter(fw);   bw.append("在已有的基础上添加字符串");   bw.write("abc\r\n ");// 往已有的文件上添加字符串     bw.close();   fw.close();
```

**3.使用PrintWriter**

```text
PrintWriter pw = new PrintWriter(new FileWriter(filePath));   pw.println("hef ");  pw.close();
```

**4.使用RandomAccessFile**

```text
RandomAccessFile rf = new RandomAccessFile(filePath, "rw");   rf.writeBytes("op\r\n");  rf.close();
```

**5.使用FileOutputStream**

```text
FileOutputStream fos = new FileOutputStream(filePath);  String s = "context";  fos.write(s.getBytes());  fos.close();
```

### Java输出内容到文件

```text
public class fileStreamTest2{public static void main(String[] args) throws IOException {File f = new File("a.txt");FileOutputStream fop = new FileOutputStream(f);// 构建FileOutputStream对象,文件不存在会自动新建OutputStreamWriter writer = new OutputStreamWriter(fop, "UTF-8");// 构建OutputStreamWriter对象,参数可以指定编码,默认为操作系统默认编码,windows上是gbkwriter.append("中文输入");// 写入到缓冲区writer.append("\r\n");//换行writer.append("English");// 刷新缓存冲,写入到文件,如果下面已经没有写入的内容了,直接close也会写入writer.close();//关闭写入流,同时会把缓冲区内容写入文件,所以上面的注释掉fop.close();// 关闭输出流,释放系统资源FileInputStream fip = new FileInputStream(f);// 构建FileInputStream对象InputStreamReader reader = new InputStreamReader(fip, "UTF-8");// 构建InputStreamReader对象,编码与写入相同StringBuffer sb = new StringBuffer();while (reader.ready()) {  sb.append((char) reader.read());  // 转成char加到StringBuffer对象中}System.out.println(sb.toString());reader.close();// 关闭读取流fip.close();// 关闭输入流,释放系统资源}}
```

#### Java创建文件/读取文件目录

java创建文件

```text
String dirname = "/tmp/user/java/bin";File d = new File(dirname);// 现在创建目录d.mkdirs();
```

java读取文件目录

```text
String dirname = "/tmp";File f1 = new File(dirname);if (f1.isDirectory()) {  System.out.println( "目录 " + dirname);  String s[] = f1.list();  for (int i=0; i < s.length; i++) {    File f = new File(dirname + "/" + s[i]);    if (f.isDirectory()) {      System.out.println(s[i] + " 是一个目录");    } else {      System.out.println(s[i] + " 是一个文件");    }  }} else {  System.out.println(dirname + " 不是一个目录");}
```

%23Java%u7684io%u64CD%u4F5C%0A%0A%21%5BJava%20IO%u7C7B%5D%28./12-130Q122402I57.jpg%29%0A%0A%0A%23%23%23%u5C06%u5B57%u7B26%u4E32%u5199%u5165%u6587%u4EF6%0A%0A**1.%u4F7F%u7528PrintStream**%0A%0A%20%20%20%20File%20file%20%3D%20new%20File%28filePath%29%3B%20%20%0A%20%20%20%20PrintStream%20ps%20%3D%20new%20PrintStream%28new%20FileOutputStream%28file%29%29%3B%20%20%0A%20%20%20%20ps.println%28context%29%3B//%20%u5F80%u6587%u4EF6%u91CC%u5199%u5165%u5B57%u7B26%u4E32%20%20%0A%20%20%20%20ps.append%28context%29%3B//%20%u5728%u5DF2%u6709%u7684%u57FA%u7840%u4E0A%u6DFB%u52A0%u5B57%u7B26%u4E32%20%0A%0A**2.%u4F7F%u7528BufferWriter**%0A%0A%20%20%20%20%20FileWriter%20fw%20%3D%20new%20FileWriter%28filePath%2C%20true%29%3B%20%20%0A%20%20%20%20%20BufferedWriter%20bw%20%3D%20new%20BufferedWriter%28fw%29%3B%20%20%0A%20%20%20%20%20bw.append%28%22%u5728%u5DF2%u6709%u7684%u57FA%u7840%u4E0A%u6DFB%u52A0%u5B57%u7B26%u4E32%22%29%3B%20%20%0A%20%20%20%20%20bw.write%28%22abc%5Cr%5Cn%20%22%29%3B//%20%u5F80%u5DF2%u6709%u7684%u6587%u4EF6%u4E0A%u6DFB%u52A0%u5B57%u7B26%u4E32%20%20%20%20%0A%20%20%20%20%20bw.close%28%29%3B%20%20%0A%20%20%20%20%20fw.close%28%29%3B%20%20%0A**3.%u4F7F%u7528PrintWriter**%0A%0A%20%20%20%20PrintWriter%20pw%20%3D%20new%20PrintWriter%28new%20FileWriter%28filePath%29%29%3B%20%20%20%0A%20%20%20%20pw.println%28%22hef%20%22%29%3B%20%20%0A%20%20%20%20pw.close%28%29%3B%20%20%0A**4.%u4F7F%u7528RandomAccessFile%20**%0A%0A%20%20%20%20%20RandomAccessFile%20rf%20%3D%20new%20RandomAccessFile%28filePath%2C%20%22rw%22%29%3B%20%20%0A%20%20%20%20%20rf.writeBytes%28%22op%5Cr%5Cn%22%29%3B%20%0A%20%20%20%20%20rf.close%28%29%3B%20%20%0A**5.%u4F7F%u7528FileOutputStream%20**%0A%0A%20%20%20%20FileOutputStream%20fos%20%3D%20new%20FileOutputStream%28filePath%29%3B%20%20%0A%20%20%20%20String%20s%20%3D%20%22context%22%3B%20%20%0A%20%20%20%20fos.write%28s.getBytes%28%29%29%3B%20%20%0A%20%20%20%20fos.close%28%29%3B%0A%23%23%23Java%u8F93%u51FA%u5185%u5BB9%u5230%u6587%u4EF6%0A%0A%20%20%20%20public%20class%20fileStreamTest2%7B%0A%09public%20static%20void%20main%28String%5B%5D%20args%29%20throws%20IOException%20%7B%0A%20%20%20%20%0A%20%20%20%20File%20f%20%3D%20new%20File%28%22a.txt%22%29%3B%0A%20%20%20%20FileOutputStream%20fop%20%3D%20new%20FileOutputStream%28f%29%3B%0A%20%20%20%20//%20%u6784%u5EFAFileOutputStream%u5BF9%u8C61%2C%u6587%u4EF6%u4E0D%u5B58%u5728%u4F1A%u81EA%u52A8%u65B0%u5EFA%0A%20%20%20%20%0A%20%20%20%20OutputStreamWriter%20writer%20%3D%20new%20OutputStreamWriter%28fop%2C%20%22UTF-8%22%29%3B%0A%20%20%20%20//%20%u6784%u5EFAOutputStreamWriter%u5BF9%u8C61%2C%u53C2%u6570%u53EF%u4EE5%u6307%u5B9A%u7F16%u7801%2C%u9ED8%u8BA4%u4E3A%u64CD%u4F5C%u7CFB%u7EDF%u9ED8%u8BA4%u7F16%u7801%2Cwindows%u4E0A%u662Fgbk%0A%20%20%20%20%0A%20%20%20%20writer.append%28%22%u4E2D%u6587%u8F93%u5165%22%29%3B%0A%20%20%20%20//%20%u5199%u5165%u5230%u7F13%u51B2%u533A%0A%20%20%20%20%0A%20%20%20%20writer.append%28%22%5Cr%5Cn%22%29%3B%0A%20%20%20%20//%u6362%u884C%0A%20%20%20%20%0A%20%20%20%20writer.append%28%22English%22%29%3B%0A%20%20%20%20//%20%u5237%u65B0%u7F13%u5B58%u51B2%2C%u5199%u5165%u5230%u6587%u4EF6%2C%u5982%u679C%u4E0B%u9762%u5DF2%u7ECF%u6CA1%u6709%u5199%u5165%u7684%u5185%u5BB9%u4E86%2C%u76F4%u63A5close%u4E5F%u4F1A%u5199%u5165%0A%20%20%20%20%0A%20%20%20%20writer.close%28%29%3B%0A%20%20%20%20//%u5173%u95ED%u5199%u5165%u6D41%2C%u540C%u65F6%u4F1A%u628A%u7F13%u51B2%u533A%u5185%u5BB9%u5199%u5165%u6587%u4EF6%2C%u6240%u4EE5%u4E0A%u9762%u7684%u6CE8%u91CA%u6389%0A%20%20%20%20%0A%20%20%20%20fop.close%28%29%3B%0A%20%20%20%20//%20%u5173%u95ED%u8F93%u51FA%u6D41%2C%u91CA%u653E%u7CFB%u7EDF%u8D44%u6E90%0A%20%0A%20%20%20%20FileInputStream%20fip%20%3D%20new%20FileInputStream%28f%29%3B%0A%20%20%20%20//%20%u6784%u5EFAFileInputStream%u5BF9%u8C61%0A%20%20%20%20%0A%20%20%20%20InputStreamReader%20reader%20%3D%20new%20InputStreamReader%28fip%2C%20%22UTF-8%22%29%3B%0A%20%20%20%20//%20%u6784%u5EFAInputStreamReader%u5BF9%u8C61%2C%u7F16%u7801%u4E0E%u5199%u5165%u76F8%u540C%0A%20%0A%20%20%20%20StringBuffer%20sb%20%3D%20new%20StringBuffer%28%29%3B%0A%20%20%20%20while%20%28reader.ready%28%29%29%20%7B%0A%20%20%20%20%20%20sb.append%28%28char%29%20reader.read%28%29%29%3B%0A%20%20%20%20%20%20//%20%u8F6C%u6210char%u52A0%u5230StringBuffer%u5BF9%u8C61%u4E2D%0A%20%20%20%20%7D%0A%20%20%20%20System.out.println%28sb.toString%28%29%29%3B%0A%20%20%20%20reader.close%28%29%3B%0A%20%20%20%20//%20%u5173%u95ED%u8BFB%u53D6%u6D41%0A%20%20%20%20%0A%20%20%20%20fip.close%28%29%3B%0A%20%20%20%20//%20%u5173%u95ED%u8F93%u5165%u6D41%2C%u91CA%u653E%u7CFB%u7EDF%u8D44%u6E90%0A%20%20%20%20%7D%0A%20%20%20%20%7D%0A%23%23%23%23Java%u521B%u5EFA%u6587%u4EF6/%u8BFB%u53D6%u6587%u4EF6%u76EE%u5F55%0Ajava%u521B%u5EFA%u6587%u4EF6%0A%0A%20%20%20%20String%20dirname%20%3D%20%22/tmp/user/java/bin%22%3B%0A%20%20%20%20File%20d%20%3D%20new%20File%28dirname%29%3B%0A%20%20%20%20//%20%u73B0%u5728%u521B%u5EFA%u76EE%u5F55%0A%20%20%20%20d.mkdirs%28%29%3B%0Ajava%u8BFB%u53D6%u6587%u4EF6%u76EE%u5F55%0A%0A%20%20%20%20String%20dirname%20%3D%20%22/tmp%22%3B%0A%20%20%20%20File%20f1%20%3D%20new%20File%28dirname%29%3B%0A%20%20%20%20if%20%28f1.isDirectory%28%29%29%20%7B%0A%20%20%20%20%20%20System.out.println%28%20%22%u76EE%u5F55%20%22%20+%20dirname%29%3B%0A%20%20%20%20%20%20String%20s%5B%5D%20%3D%20f1.list%28%29%3B%0A%20%20%20%20%20%20for%20%28int%20i%3D0%3B%20i%20%3C%20s.length%3B%20i++%29%20%7B%0A%20%20%20%20%20%20%20%20File%20f%20%3D%20new%20File%28dirname%20+%20%22/%22%20+%20s%5Bi%5D%29%3B%0A%20%20%20%20%20%20%20%20if%20%28f.isDirectory%28%29%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20System.out.println%28s%5Bi%5D%20+%20%22%20%u662F%u4E00%u4E2A%u76EE%u5F55%22%29%3B%0A%20%20%20%20%20%20%20%20%7D%20else%20%7B%0A%20%20%20%20%20%20%20%20%20%20System.out.println%28s%5Bi%5D%20+%20%22%20%u662F%u4E00%u4E2A%u6587%u4EF6%22%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%20else%20%7B%0A%20%20%20%20%20%20System.out.println%28dirname%20+%20%22%20%u4E0D%u662F%u4E00%u4E2A%u76EE%u5F55%22%29%3B%0A%20%20%20%20%7D


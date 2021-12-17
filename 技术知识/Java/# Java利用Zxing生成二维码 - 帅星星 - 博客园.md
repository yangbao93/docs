# Java利用Zxing生成二维码 - 帅星星 - 博客园

## Java利用Zxing生成二维码 - 帅星星 - 博客园

[帅星星](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)

* [博客园](http://www.cnblogs.com/)
* [首页](http://www.cnblogs.com/jtmjx/)
* [新随笔](https://i.cnblogs.com/EditPosts.aspx?opt=1)
* [联系](http://msg.cnblogs.com/send/%E5%B8%85%E6%98%9F%E6%98%9F)
* [订阅](http://www.cnblogs.com/jtmjx/rss)
* [管理](https://i.cnblogs.com/)

  随笔-11 文章-0 评论-8

## [Java利用Zxing生成二维码](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html)

Zxing是Google提供的关于条码（一维码、二维码）的解析工具，提供了二维码的生成与解析的方法，现在我简单介绍一下使用Java利用Zxing生成与解析二维码

1、二维码的生成

1.1 将Zxing-core.jar 包加入到classpath下。

1.2 二维码的生成需要借助MatrixToImageWriter类，该类是由Google提供的，可以将该类拷贝到源码中，这里我将该类的源码贴上，可以直接使用。

按 Ctrl+C 复制代码 按 Ctrl+C 复制代码

1.3 编写生成二维码的实现代码

按 Ctrl+C 复制代码 按 Ctrl+C 复制代码

现在运行后即可生成一张二维码图片，是不是很简单啊？ 接下来我们看看如何解析二维码

2、二维码的解析

2.1 将Zxing-core.jar 包加入到classpath下。

2.2 和生成一样，我们需要一个辅助类（ BufferedImageLuminanceSource），同样该类Google也提供了，这里我同样将该类的源码贴出来，可以直接拷贝使用个，省去查找的麻烦

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Java利用Zxing生成二维码%20-%20帅星星%20-%20博客园/copycode.gif)

```text
BufferedImageLuminanceSource 
 import com.google.zxing.LuminanceSource;

 import java.awt.Graphics2D;
 import java.awt.geom.AffineTransform;
 import java.awt.image.BufferedImage;

 public final class BufferedImageLuminanceSource extends LuminanceSource {

   private final BufferedImage image;
   private final int left;
   private final int top;

   public BufferedImageLuminanceSource(BufferedImage image) {
     this(image, 0, 0, image.getWidth(), image.getHeight());
   }

   public BufferedImageLuminanceSource(BufferedImage image, int left, int top, int width, int height) {
     super(width, height);

     int sourceWidth = image.getWidth();
     int sourceHeight = image.getHeight();
     if (left + width > sourceWidth || top + height > sourceHeight) {
       throw new IllegalArgumentException("Crop rectangle does not fit within image data.");
     }

     for (int y = top; y < top + height; y++) {
       for (int x = left; x < left + width; x++) {
         if ((image.getRGB(x, y) & 0xFF000000) == 0) {
           image.setRGB(x, y, 0xFFFFFFFF); // = white
         }
       }
     }

     this.image = new BufferedImage(sourceWidth, sourceHeight, BufferedImage.TYPE_BYTE_GRAY);
     this.image.getGraphics().drawImage(image, 0, 0, null);
     this.left = left;
     this.top = top;
   }

   @Override
   public byte[] getRow(int y, byte[] row) {
     if (y < 0 || y >= getHeight()) {
       throw new IllegalArgumentException("Requested row is outside the image: " + y);
     }
     int width = getWidth();
     if (row == null || row.length < width) {
       row = new byte[width];
     }
     image.getRaster().getDataElements(left, top + y, width, 1, row);
     return row;
   }

   @Override
   public byte[] getMatrix() {
     int width = getWidth();
     int height = getHeight();
     int area = width * height;
     byte[] matrix = new byte[area];
     image.getRaster().getDataElements(left, top, width, height, matrix);
     return matrix;
   }

   @Override
   public boolean isCropSupported() {
     return true;
   }

   @Override
   public LuminanceSource crop(int left, int top, int width, int height) {
     return new BufferedImageLuminanceSource(image, this.left + left, this.top + top, width, height);
   }

   @Override
   public boolean isRotateSupported() {
     return true;
   }

   @Override
   public LuminanceSource rotateCounterClockwise() {

       int sourceWidth = image.getWidth();
     int sourceHeight = image.getHeight();

     AffineTransform transform = new AffineTransform(0.0, -1.0, 1.0, 0.0, 0.0, sourceWidth);

     BufferedImage rotatedImage = new BufferedImage(sourceHeight, sourceWidth, BufferedImage.TYPE_BYTE_GRAY);

     Graphics2D g = rotatedImage.createGraphics();
     g.drawImage(image, transform, null);
     g.dispose();

     int width = getWidth();
     return new BufferedImageLuminanceSource(rotatedImage, top, sourceWidth - (left + width), getHeight(), width);
   }

 }
```

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Java利用Zxing生成二维码%20-%20帅星星%20-%20博客园/copycode.gif)

2.3 编写解析二维码的实现代码

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Java利用Zxing生成二维码%20-%20帅星星%20-%20博客园/copycode.gif)

```text
try {
                         MultiFormatReader formatReader = new MultiFormatReader();
             String filePath = "C:/Users/Administrator/Desktop/testImage/test.jpg";
             File file = new File(filePath);
             BufferedImage image = ImageIO.read(file);;
             LuminanceSource source = new BufferedImageLuminanceSource(image);
             Binarizer  binarizer = new HybridBinarizer(source);
             BinaryBitmap binaryBitmap = new BinaryBitmap(binarizer);
             Map hints = new HashMap();
             hints.put(EncodeHintType.CHARACTER_SET, "UTF-8");
             Result result = formatReader.decode(binaryBitmap,hints);

                         System.out.println("result = "+ result.toString());
             System.out.println("resultFormat = "+ result.getBarcodeFormat());
             System.out.println("resultText = "+ result.getText());

         } catch (Exception e) {
             e.printStackTrace();
         }
```

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Java利用Zxing生成二维码%20-%20帅星星%20-%20博客园/copycode.gif)

现在运行后可以看到控制台打印出了二维码的内容。

到此为止，利用Zxing生成和解析二维码就讲述演示完毕，主要为自己做备忘，同时方便有需要的人。呵呵！

分类: [Java](http://www.cnblogs.com/jtmjx/category/384929.html) 标签: [Java Zxing 二维码](http://www.cnblogs.com/jtmjx/tag/Java%20Zxing%20%E4%BA%8C%E7%BB%B4%E7%A0%81/) [好文要顶](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) [关注我](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) [收藏该文](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)  
![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Java利用Zxing生成二维码%20-%20帅星星%20-%20博客园/icon_weibo_24.png)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Java利用Zxing生成二维码%20-%20帅星星%20-%20博客园/wechat.png)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Java利用Zxing生成二维码%20-%20帅星星%20-%20博客园/sample_face.gif) [帅星星](http://home.cnblogs.com/u/jtmjx/) [关注 - 7](http://home.cnblogs.com/u/jtmjx/followees) [粉丝 - 15](http://home.cnblogs.com/u/jtmjx/followers)

[+加关注](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)

7 0

[»](http://www.cnblogs.com/jtmjx/archive/2012/06/29/2570130.html) 下一篇： [Oracle无法使用EM解决方案](http://www.cnblogs.com/jtmjx/archive/2012/06/29/2570130.html)

posted @ 2012-06-18 15:39 [帅星星](http://www.cnblogs.com/jtmjx/) 阅读\(48546\) 评论\(5\) [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=2545209) [收藏](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#)

[评论列表 ＃1楼](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) [2013-07-19 10:06 孤独青鸟](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)  
解析的时候报错，com.google.zxing.NotFoundException [支持\(1\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) [反对\(0\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)

[\#2楼](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#2834661) [2013-12-10 22:52 colabox](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)  
[@](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#2730567) 孤独青鸟 [引用](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#2730567) 解析的时候报错，com.google.zxing.NotFoundException

我也遇到同样的错误。 [支持\(0\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) [反对\(0\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)

[\#3楼](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#2935535) [2014-05-09 14:45 曾哥哥](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)  
[@](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#2834661) colabox 把容错等级设高点 H [支持\(0\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) [反对\(0\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)

[\#4楼](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#3049525) [2014-10-23 11:21 塞北夜莺](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)  
mark [支持\(0\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) [反对\(0\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)

[\#5楼](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#3111747) [2015-01-21 16:02 ZhLingF](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)  
mark 在研究 [支持\(0\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) [反对\(0\)](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)

[刷新评论](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) [刷新页面](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#) [返回顶部](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#top) 注册用户登录后才能发表评论，请 [登录](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) 或 [注册](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) ， [访问](http://www.cnblogs.com/) 网站首页。

**最新IT新闻**: · [Windows 10正式版年度累计更新14393.105推送！](http://news.cnblogs.com/n/552681/) · [《魔兽世界：军团再临》正式上线！110级来了](http://news.cnblogs.com/n/552680/) · [涂上保护层 西瓜变身弹力球摔不坏](http://news.cnblogs.com/n/552679/) · [Chrome 53首个稳定版本正式发布](http://news.cnblogs.com/n/552678/) · [第一部AI剪辑的预告片](http://news.cnblogs.com/n/552677/) » [更多新闻...](http://news.cnblogs.com/)

**最新知识库文章**: · [程序猿媳妇儿注意事项](http://kb.cnblogs.com/page/550625/) · [可是姑娘，你为什么要编程呢？](http://kb.cnblogs.com/page/540529/) · [知其所以然（以算法学习为例）](http://kb.cnblogs.com/page/549631/) · [如何给变量取个简短且无歧义的名字](http://kb.cnblogs.com/page/548394/) · [编程的智慧](http://kb.cnblogs.com/page/549080/)

» [更多知识库文章...](http://kb.cnblogs.com/)

### 公告

昵称： [帅星星](http://home.cnblogs.com/u/jtmjx/) 园龄： [4年7个月](http://home.cnblogs.com/u/jtmjx/) 粉丝： [15](http://home.cnblogs.com/u/jtmjx/followers/) 关注： [7](http://home.cnblogs.com/u/jtmjx/followees/) [+加关注](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md)

[&lt;](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) 2012年6月 [&gt;](java-li-yong-zxing-sheng-cheng-er-wei-ma-shuai-xing-xing-bo-ke-yuan.md) 日一二三四五六27282930311234567891011121314151617 [18](http://www.cnblogs.com/jtmjx/archive/2012/06/18.html) 19202122232425262728 [29](http://www.cnblogs.com/jtmjx/archive/2012/06/29.html) 301234567

### 搜索

### 常用链接

* [我的随笔](http://www.cnblogs.com/jtmjx/p/)
* [我的评论](http://www.cnblogs.com/jtmjx/MyComments.html)
* [我的参与](http://www.cnblogs.com/jtmjx/OtherPosts.html)
* [最新评论](http://www.cnblogs.com/jtmjx/RecentComments.html)
* [我的标签](http://www.cnblogs.com/jtmjx/tag/)

### 我的标签

* [JFreeChart](http://www.cnblogs.com/jtmjx/tag/JFreeChart/) \(7\)
* [Oracle](http://www.cnblogs.com/jtmjx/tag/Oracle/) \(2\)
* [Java Zxing 二维码](http://www.cnblogs.com/jtmjx/tag/Java%20Zxing%20%E4%BA%8C%E7%BB%B4%E7%A0%81/) \(1\)

### 随笔分类

* [Android](http://www.cnblogs.com/jtmjx/category/384930.html)
* [c\#](http://www.cnblogs.com/jtmjx/category/384931.html)
* [Java\(8\)](http://www.cnblogs.com/jtmjx/category/384929.html)
* [JFreeChart\(7\)](http://www.cnblogs.com/jtmjx/category/472984.html)
* [Oracle\(2\)](http://www.cnblogs.com/jtmjx/category/391881.html)

### 随笔档案

* [2013年4月 \(7\)](http://www.cnblogs.com/jtmjx/archive/2013/04.html)
* [2013年3月 \(1\)](http://www.cnblogs.com/jtmjx/archive/2013/03.html)
* [2012年7月 \(1\)](http://www.cnblogs.com/jtmjx/archive/2012/07.html)
* [2012年6月 \(2\)](http://www.cnblogs.com/jtmjx/archive/2012/06.html)

### 最新评论

* [1. Re:"ORA-00942: 表或视图不存在 "的原因和解决方法](http://www.cnblogs.com/jtmjx/archive/2012/07/03/2574766.html#3403518)
* ORA-00942: 表或视图不存在
* --Bruce\_beichende
* [2. Re:"ORA-00942: 表或视图不存在 "的原因和解决方法](http://www.cnblogs.com/jtmjx/archive/2012/07/03/2574766.html#3214985)
* 谢谢
* --银杏叶儿
* [3. Re:"ORA-00942: 表或视图不存在 "的原因和解决方法](http://www.cnblogs.com/jtmjx/archive/2012/07/03/2574766.html#3157964)
* 谢谢谢谢
* --孱弱的土鳖
* [4. Re:Java利用Zxing生成二维码](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#3111747)
* mark 在研究
* --\_叶落星辰
* [5. Re:Java利用Zxing生成二维码](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html#3049525)
* mark
* --塞北夜莺

### 阅读排行榜

* [1. "ORA-00942: 表或视图不存在 "的原因和解决方法\(70131\)](http://www.cnblogs.com/jtmjx/archive/2012/07/03/2574766.html)
* [2. Java利用Zxing生成二维码\(48545\)](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html)
* [3. Oracle无法使用EM解决方案\(3394\)](http://www.cnblogs.com/jtmjx/archive/2012/06/29/2570130.html)
* [4. JFreeChart 使用一 饼图之高级特性\(2465\)](http://www.cnblogs.com/jtmjx/archive/2013/04/23/jfreechart_advantage.html)
* [5. JFreeChart 使用 一 直方图之柱状图-高级特性\(1517\)](http://www.cnblogs.com/jtmjx/archive/2013/04/23/3039161.html)

### 评论排行榜

* [1. Java利用Zxing生成二维码\(5\)](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html)
* [2. "ORA-00942: 表或视图不存在 "的原因和解决方法\(3\)](http://www.cnblogs.com/jtmjx/archive/2012/07/03/2574766.html)

### 推荐排行榜

* [1. Java利用Zxing生成二维码\(7\)](http://www.cnblogs.com/jtmjx/archive/2012/06/18/2545209.html)
* [2. "ORA-00942: 表或视图不存在 "的原因和解决方法\(7\)](http://www.cnblogs.com/jtmjx/archive/2012/07/03/2574766.html)

[http://pic.cnblogs.com/face/sample\_face.gif](http://pic.cnblogs.com/face/sample_face.gif)


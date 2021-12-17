# spring各种邮件发送 - 路漫漫其修远兮,吾将上下而求索! - BlogJava

[spring各种邮件发送](http://www.blogjava.net/tangzurui/archive/2008/12/08/244953.html) Spring邮件抽象层的主要包为org.springframework.mail。它包括了发送电子邮件的主要接口MailSender，和 _值对象_ SimpleMailMessage，它封装了简单邮件的属性如 _from_, _to_,_cc_, _subject_,_text_ 。 包里还包含一棵以MailException为根的checked Exception继承树，它们提供了对底层邮件系统异常的高级别抽象。 要获得关于邮件异常层次的更丰富的信息，请参考Javadocs。

为了使用 _JavaMail_ 中的一些特色, 比如MIME类型的信件, Spring提供了MailSender的一个子接口, 即org.springframework.mail.javamail.JavaMailSender。Spring还提供了一个回调接口org.springframework.mail.javamail.MimeMessagePreparator, 用于准备JavaMail的MIME信件。

**1.发送简单的文本邮件**

package net.xftzr.mail; import java.util.Properties;

import org.springframework.mail.SimpleMailMessage; import org.springframework.mail.javamail.JavaMailSenderImpl;

/\*\*

* 本类测试简单邮件
* 直接用邮件发送
* @author  Administrator

  \*

  \*/

  public   class  SingleMailSend {

  public   static   void  main\(String args\[\]\){

  JavaMailSenderImpl senderImpl  =   new  JavaMailSenderImpl\(\);

  // 设定mail server

  senderImpl.setHost\( " smtp.163.com " \);

// 建立邮件消息 SimpleMailMessage mailMessage = new SimpleMailMessage\(\); // 设置收件人，寄件人 用数组发送多个邮件 // String\[\] array = new String\[\] {"sun111@163.com","sun222@sohu.com"}; // mailMessage.setTo\(array\); mailMessage.setTo\( " toEmail@sina.com " \); mailMessage.setFrom\( " userName@163.com " \); mailMessage.setSubject\( " 测试简单文本邮件发送！ " \); mailMessage.setText\( " 测试我的简单邮件发送机制！！ " \);

senderImpl.setUsername\( " userName " \) ; // 根据自己的情况,设置username senderImpl.setPassword\( " password " \) ; // 根据自己的情况, 设置password

Properties prop = new Properties\(\) ; prop.put\( " mail.smtp.auth " , " true " \) ; // 将这个参数设为true，让服务器进行认证,认证用户名和密码是否正确 prop.put\( " mail.smtp.timeout " , " 25000 " \) ; senderImpl.setJavaMailProperties\(prop\); // 发送邮件 senderImpl.send\(mailMessage\);

System.out.println\( " 邮件发送成功 ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/dot.gif) .. " \); } }

**2.发送简单的html邮件** org.springframework.mail.javamail.MimeMessageHelper是处理JavaMail邮件常用的顺手组件之一。它可以让你摆脱繁复的javax.mail.internetAPI类 package net.xftzr.mail;

import java.util.Properties;

import javax.mail.internet.MimeMessage; import org.springframework.mail.javamail.JavaMailSenderImpl; import org.springframework.mail.javamail.MimeMessageHelper; /\*\*

* 本类测试html邮件
* @author sunny

  \*

  \*/

  public class HTMLMailDemo {

  /\*\*

* @param args

  \*/

  public static void main\(String\[\] args\) throws Exception{

  JavaMailSenderImpl senderImpl = new JavaMailSenderImpl\(\);

//设定mail server senderImpl.setHost\("smtp.163.com"\);

//建立邮件消息,发送简单邮件和html邮件的区别 MimeMessage mailMessage = senderImpl.createMimeMessage\(\); MimeMessageHelper messageHelper = new MimeMessageHelper\(mailMessage\);

//设置收件人，寄件人 messageHelper.setTo\("Mailto@sina.com"\); messageHelper.setFrom\("username@163.com"\); messageHelper.setSubject\("测试HTML邮件！"\); //true 表示启动HTML格式的邮件 messageHelper.setText\("hello!!spring html Mail",true\);

senderImpl.setUsername\("username"\) ; // 根据自己的情况,设置username senderImpl.setPassword\("password"\) ; // 根据自己的情况, 设置password Properties prop = new Properties\(\) ; prop.put\("mail.smtp.auth", "true"\) ; // 将这个参数设为true，让服务器进行认证,认证用户名和密码是否正确 prop.put\("mail.smtp.timeout", "25000"\) ; senderImpl.setJavaMailProperties\(prop\); //发送邮件 senderImpl.send\(mailMessage\);

System.out.println\("邮件发送成功 ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/dot.gif) .."\); } }

**3.发送嵌套图片的邮件**

Email允许添加附件，也允许在multipart信件中内嵌资源。内嵌资源可能是你在信件中希望使用的图像，或者样式表，但是又不想把它们作为附件。 package net.xftzr.mail;

import java.io.File; import java.util.Properties;

import javax.mail.internet.MimeMessage; import org.springframework.core.io.FileSystemResource; import org.springframework.mail.javamail.JavaMailSenderImpl; import org.springframework.mail.javamail.MimeMessageHelper; /\*\*

* 本类测试邮件中嵌套图片
* @author sunny

  \*

  \*/

  public class AttachedImageMail {

  public static void main\(String\[\] args\) throws Exception{

  JavaMailSenderImpl senderImpl = new JavaMailSenderImpl\(\);

//设定mail server senderImpl.setHost\("smtp.163.com"\);

//建立邮件消息,发送简单邮件和html邮件的区别 MimeMessage mailMessage = senderImpl.createMimeMessage\(\); //注意这里的boolean,等于真的时候才能嵌套图片，在构建MimeMessageHelper时候，所给定的值是true表示启用， //multipart模式 MimeMessageHelper messageHelper = new MimeMessageHelper\(mailMessage,true\);

//设置收件人，寄件人 messageHelper.setTo\("toMail@sina.com"\); messageHelper.setFrom\("username@163.com"\); messageHelper.setSubject\("测试邮件中嵌套图片!！"\); //true 表示启动HTML格式的邮件 messageHelper.setText\("hello!!spring image html mail",true\);

FileSystemResource img = new FileSystemResource\(new File\("g:/123.jpg"\)\);

messageHelper.addInline\("aaa",img\);

senderImpl.setUsername\("username"\) ; // 根据自己的情况,设置username senderImpl.setPassword\("password"\) ; // 根据自己的情况, 设置password Properties prop = new Properties\(\) ; prop.put\("mail.smtp.auth", "true"\) ; // 将这个参数设为true，让服务器进行认证,认证用户名和密码是否正确 prop.put\("mail.smtp.timeout", "25000"\) ; senderImpl.setJavaMailProperties\(prop\);

//发送邮件 senderImpl.send\(mailMessage\);

System.out.println\("邮件发送成功 ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/dot.gif) .."\); } }

**4.发送包含附件的邮件**

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif) package net.xftzr.mail;

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif) import java.io.File;

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif) import java.util.Properties;

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif) import javax.mail.internet.MimeMessage;

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif) import org.springframework.core.io.FileSystemResource;

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif) import org.springframework.mail.javamail.JavaMailSenderImpl;

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/None.gif) import org.springframework.mail .javamail.MimeMessageHelper;

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/ExpandedBlockStart.gif) public class AttachedFileMail {

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/ExpandedSubBlockStart.gif) /\*\*

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif)

* 本类测试的是关于邮件中带有附件的例子

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif)

* @param args

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/ExpandedSubBlockEnd.gif) \*/

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/ExpandedSubBlockStart.gif) public static void main\(String\[\] args\) throws Exception{

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) JavaMailSenderImpl senderImpl = new JavaMailSenderImpl\(\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) //设定mail server

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) senderImpl.setHost\("smtp.163.com"\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) //建立邮件消息,发送简单邮件和html邮件的区别

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) MimeMessage mailMessage = senderImpl.createMimeMessage\(\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) //注意这里的boolean,等于真的时候才能嵌套图片，在构建MimeMessageHelper时候，所给定的值是true表示启用，

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) //multipart模式 为true时发送附件 可以设置html格式

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) MimeMessageHelper messageHelper = new MimeMessageHelper\(mailMessage,true,"utf-8"\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) //设置收件人，寄件人

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) messageHelper.setTo\("toMail@sina.com"\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) messageHelper.setFrom\("username@163.com"\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) messageHelper.setSubject\("测试邮件中上传附件!！"\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) //true 表示启动HTML格式的邮件

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) messageHelper.setText\("你好：附件中有学习资料！",true\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) FileSystemResource file = new FileSystemResource\(new File\("g:/test.rar"\)\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) //这里的方法调用和插入图片是不同的。

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) messageHelper.addAttachment\("test.rar",file\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) senderImpl.setUsername\("username"\) ; // 根据自己的情况,设置username

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) senderImpl.setPassword\("password"\) ; // 根据自己的情况, 设置password

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) Properties prop = new Properties\(\) ;

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) prop.put\("mail.smtp.auth", "true"\) ; // 将这个参数设为true，让服务器进行认证,认证用户名和密码是否正确

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) prop.put\("mail.smtp.timeout", "25000"\) ;

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) senderImpl.setJavaMailProperties\(prop\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) //发送邮件

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) senderImpl.send\(mailMessage\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/InBlock.gif) System.out.println\("邮件发送成功 ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/dot.gif) .."\);

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/ExpandedSubBlockEnd.gif) }

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring各种邮件发送%20-%20路漫漫其修远兮,吾将上下而求索!%20-%20BlogJava/ExpandedBlockEnd.gif) }

posted on 2008-12-08 10:32 [梓枫](http://www.blogjava.net/tangzurui/) 阅读\(4266\) [评论\(3\)](http://www.blogjava.net/tangzurui/archive/2008/12/08/244953.html#Post) [编辑](http://www.blogjava.net/tangzurui/admin/EditPosts.aspx?postid=244953) [收藏](http://www.blogjava.net/tangzurui/AddToFavorite.aspx?id=244953) 所属分类: [spring发送邮件](http://www.blogjava.net/tangzurui/category/36525.html)

[http://www.blogjava.net/Images/OutliningIndicators/ExpandedSubBlockStart.gif](http://www.blogjava.net/Images/OutliningIndicators/ExpandedSubBlockStart.gif)


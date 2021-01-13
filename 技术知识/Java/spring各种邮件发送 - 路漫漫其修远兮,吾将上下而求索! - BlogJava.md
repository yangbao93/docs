# spring各种邮件发送 - 路漫漫其修远兮,吾将上下而求索! - BlogJava
[spring各种邮件发送](http://www.blogjava.net/tangzurui/archive/2008/12/08/244953.html) 
Spring邮件抽象层的主要包为org.springframework.mail。它包括了发送电子邮件的主要接口MailSender，和 *值对象* SimpleMailMessage，它封装了简单邮件的属性如 *from*, *to*,*cc*, *subject*,*text* 。 包里还包含一棵以MailException为根的checked Exception继承树，它们提供了对底层邮件系统异常的高级别抽象。 要获得关于邮件异常层次的更丰富的信息，请参考Javadocs。

为了使用 *JavaMail* 中的一些特色, 比如MIME类型的信件, Spring提供了MailSender的一个子接口, 即org.springframework.mail.javamail.JavaMailSender。Spring还提供了一个回调接口org.springframework.mail.javamail.MimeMessagePreparator, 用于准备JavaMail的MIME信件。

**1.发送简单的文本邮件**

package  net.xftzr.mail;
import  java.util.Properties;

import  org.springframework.mail.SimpleMailMessage;
import  org.springframework.mail.javamail.JavaMailSenderImpl;

/**
* 本类测试简单邮件
* 直接用邮件发送
*  @author  Administrator
*
*/
public   class  SingleMailSend {
public   static   void  main(String args[]){
JavaMailSenderImpl senderImpl  =   new  JavaMailSenderImpl();
// 设定mail server
senderImpl.setHost( " smtp.163.com " );

// 建立邮件消息
SimpleMailMessage mailMessage  =   new  SimpleMailMessage();
// 设置收件人，寄件人 用数组发送多个邮件
// String[] array = new String[] {"sun111@163.com","sun222@sohu.com"};
// mailMessage.setTo(array);
mailMessage.setTo( " toEmail@sina.com " );
mailMessage.setFrom( " userName@163.com " );
mailMessage.setSubject( " 测试简单文本邮件发送！ " );
mailMessage.setText( " 测试我的简单邮件发送机制！！ " );

senderImpl.setUsername( " userName " ) ;  //  根据自己的情况,设置username
senderImpl.setPassword( " password " ) ;  //  根据自己的情况, 设置password

Properties prop  =   new  Properties() ;
prop.put( " mail.smtp.auth " ,  " true " ) ;  //  将这个参数设为true，让服务器进行认证,认证用户名和密码是否正确
prop.put( " mail.smtp.timeout " ,  " 25000 " ) ;
senderImpl.setJavaMailProperties(prop);
// 发送邮件
senderImpl.send(mailMessage);

System.out.println( " 邮件发送成功
![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/dot.gif)
.. " );
}
}

 
**2.发送简单的html邮件** 
org.springframework.mail.javamail.MimeMessageHelper是处理JavaMail邮件常用的顺手组件之一。它可以让你摆脱繁复的javax.mail.internetAPI类
package net.xftzr.mail;

import java.util.Properties;

import javax.mail.internet.MimeMessage;
import org.springframework.mail.javamail.JavaMailSenderImpl;
import org.springframework.mail.javamail.MimeMessageHelper;
/**
* 本类测试html邮件
* @author sunny
*
*/
public class HTMLMailDemo {
/**
* @param args
*/
public static void main(String[] args) throws Exception{
JavaMailSenderImpl senderImpl = new JavaMailSenderImpl();

//设定mail server
senderImpl.setHost("smtp.163.com");

//建立邮件消息,发送简单邮件和html邮件的区别
MimeMessage mailMessage = senderImpl.createMimeMessage();
MimeMessageHelper messageHelper = new MimeMessageHelper(mailMessage);

//设置收件人，寄件人
messageHelper.setTo("Mailto@sina.com");
messageHelper.setFrom("username@163.com");
messageHelper.setSubject("测试HTML邮件！");
//true 表示启动HTML格式的邮件
messageHelper.setText("<html><head></head><body><h1>hello!!spring html Mail</h1></body></html>",true);

senderImpl.setUsername("username") ; // 根据自己的情况,设置username
senderImpl.setPassword("password") ; // 根据自己的情况, 设置password
Properties prop = new Properties() ;
prop.put("mail.smtp.auth", "true") ; // 将这个参数设为true，让服务器进行认证,认证用户名和密码是否正确
prop.put("mail.smtp.timeout", "25000") ;
senderImpl.setJavaMailProperties(prop);
//发送邮件
senderImpl.send(mailMessage);

System.out.println("邮件发送成功
![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/dot.gif)
..");
}
}
 
**3.发送嵌套图片的邮件**

Email允许添加附件，也允许在multipart信件中内嵌资源。内嵌资源可能是你在信件中希望使用的图像，或者样式表，但是又不想把它们作为附件。
package net.xftzr.mail;

import java.io.File;
import java.util.Properties;

import javax.mail.internet.MimeMessage;
import org.springframework.core.io.FileSystemResource;
import org.springframework.mail.javamail.JavaMailSenderImpl;
import org.springframework.mail.javamail.MimeMessageHelper;
/**
* 本类测试邮件中嵌套图片
* @author sunny
*
*/
public class AttachedImageMail {
public static void main(String[] args) throws Exception{
JavaMailSenderImpl senderImpl = new JavaMailSenderImpl();

//设定mail server
senderImpl.setHost("smtp.163.com");

//建立邮件消息,发送简单邮件和html邮件的区别
MimeMessage mailMessage = senderImpl.createMimeMessage();
//注意这里的boolean,等于真的时候才能嵌套图片，在构建MimeMessageHelper时候，所给定的值是true表示启用，
//multipart模式
MimeMessageHelper messageHelper = new MimeMessageHelper(mailMessage,true);

//设置收件人，寄件人
messageHelper.setTo("toMail@sina.com");
messageHelper.setFrom("username@163.com");
messageHelper.setSubject("测试邮件中嵌套图片!！");
//true 表示启动HTML格式的邮件
messageHelper.setText("<html><head></head><body><h1>hello!!spring image html mail</h1>" +
"<img src=\"cid:aaa\"/></body></html>",true);

FileSystemResource img = new FileSystemResource(new File("g:/123.jpg"));

messageHelper.addInline("aaa",img);

senderImpl.setUsername("username") ; // 根据自己的情况,设置username
senderImpl.setPassword("password") ; // 根据自己的情况, 设置password
Properties prop = new Properties() ;
prop.put("mail.smtp.auth", "true") ; // 将这个参数设为true，让服务器进行认证,认证用户名和密码是否正确
prop.put("mail.smtp.timeout", "25000") ;
senderImpl.setJavaMailProperties(prop);

//发送邮件
senderImpl.send(mailMessage);

System.out.println("邮件发送成功
![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/dot.gif)
..");
}
}

**4.发送包含附件的邮件**

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)
package net.xftzr.mail;

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)
import java.io.File;

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)
import java.util.Properties;

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)
import javax.mail.internet.MimeMessage;

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)
import org.springframework.core.io.FileSystemResource;

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)
import org.springframework.mail.javamail.JavaMailSenderImpl;

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/None.gif)
import org.springframework.mail .javamail.MimeMessageHelper;

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/ExpandedBlockStart.gif)
public class AttachedFileMail {

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/ExpandedSubBlockStart.gif)
/**

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
* 本类测试的是关于邮件中带有附件的例子

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
* @param args

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/ExpandedSubBlockEnd.gif)
*/

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/ExpandedSubBlockStart.gif)
public static void main(String[] args) throws Exception{

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
JavaMailSenderImpl senderImpl = new JavaMailSenderImpl();

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
//设定mail server

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
senderImpl.setHost("smtp.163.com");

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
//建立邮件消息,发送简单邮件和html邮件的区别

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
MimeMessage mailMessage = senderImpl.createMimeMessage();

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
//注意这里的boolean,等于真的时候才能嵌套图片，在构建MimeMessageHelper时候，所给定的值是true表示启用，

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
//multipart模式 为true时发送附件 可以设置html格式

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
MimeMessageHelper messageHelper = new MimeMessageHelper(mailMessage,true,"utf-8");

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
//设置收件人，寄件人

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
messageHelper.setTo("toMail@sina.com");

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
messageHelper.setFrom("username@163.com");

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
messageHelper.setSubject("测试邮件中上传附件!！");

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
//true 表示启动HTML格式的邮件

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
messageHelper.setText("<html><head></head><body><h1>你好：附件中有学习资料！</h1></body></html>",true);

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
FileSystemResource file = new FileSystemResource(new File("g:/test.rar"));

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
//这里的方法调用和插入图片是不同的。

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
messageHelper.addAttachment("test.rar",file);

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
senderImpl.setUsername("username") ; // 根据自己的情况,设置username

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
senderImpl.setPassword("password") ; // 根据自己的情况, 设置password

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
Properties prop = new Properties() ;

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
prop.put("mail.smtp.auth", "true") ; // 将这个参数设为true，让服务器进行认证,认证用户名和密码是否正确

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
prop.put("mail.smtp.timeout", "25000") ;

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
senderImpl.setJavaMailProperties(prop);

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
//发送邮件

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
senderImpl.send(mailMessage);

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/InBlock.gif)
System.out.println("邮件发送成功
![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/dot.gif)
..");

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/ExpandedSubBlockEnd.gif)
}

![](spring%E5%90%84%E7%A7%8D%E9%82%AE%E4%BB%B6%E5%8F%91%E9%80%81%20-%20%E8%B7%AF%E6%BC%AB%E6%BC%AB%E5%85%B6%E4%BF%AE%E8%BF%9C%E5%85%AE,%E5%90%BE%E5%B0%86%E4%B8%8A%E4%B8%8B%E8%80%8C%E6%B1%82%E7%B4%A2!%20-%20BlogJava/ExpandedBlockEnd.gif)
}

posted on 2008-12-08 10:32 [梓枫](http://www.blogjava.net/tangzurui/) 阅读(4266) [评论(3)](http://www.blogjava.net/tangzurui/archive/2008/12/08/244953.html#Post) [编辑](http://www.blogjava.net/tangzurui/admin/EditPosts.aspx?postid=244953) [收藏](http://www.blogjava.net/tangzurui/AddToFavorite.aspx?id=244953) 所属分类: [spring发送邮件](http://www.blogjava.net/tangzurui/category/36525.html) 

http://www.blogjava.net/Images/OutliningIndicators/ExpandedSubBlockStart.gif
# Java实现邮件的发送
maven 添加依赖 （==引入jar包）

* pom.xml中添加：

mail相关：

```
<dependency>
        <groupId>javax.mail</groupId>
        <artifactId>mail</artifactId>
        <version>1.4.7</version>
     </dependency>
```

spring相关：

```
<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>3.2.6.RELEASE</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-orm</artifactId>
        <version>3.2.6.RELEASE</version>
      </dependency>
     <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-support</artifactId>
        <version>3.2.6.RELEASE</version>
     </dependency>
```

* 发送设置（简单的email发送）

JavaMailSenderImpl send = new JavaMailSenderImpl();

//发送前的设置
sendMail.setHost();//设置host，如SMTP.yonyou.com
sendMail.setUsername();//设置发送邮箱的用户名
sendMail.setPassword();//设置发送邮箱的密码
sendMail.setPort();//设置端口号

//发送的邮件设置
SimpleMailMessage message = new SimpleMailMessage();
message.setFrom();//发件人信息
message.setSubject();//邮件主题
message.setText();//邮件内容
message.setTo();//邮件接收人

//发送邮件
sendMail.send(message);

参考：
A simple text email

[http://commons.apache.org/proper/commons-email/userguide.html](http://commons.apache.org/proper/commons-email/userguide.html)

[spring各种邮件发送](http://www.blogjava.net/tangzurui/archive/2008/12/08/244953.html)
[http://www.blogjava.net/tangzurui/archive/2008/12/08/244953.html](http://www.blogjava.net/tangzurui/archive/2008/12/08/244953.html)

[使用Spring的JAVA Mail支持简化邮件发送](http://blog.csdn.net/zheng0518/article/details/50179213)
[http://blog.csdn.net/zheng0518/article/details/50179213](http://blog.csdn.net/zheng0518/article/details/50179213)

[通过Spring Mail Api发送邮件](http://blog.csdn.net/is_zhoufeng/article/details/12004073)
[http://blog.csdn.net/is_zhoufeng/article/details/12004073](http://blog.csdn.net/is_zhoufeng/article/details/12004073)

  **[javamail技术smtp发送邮件](http://blog.csdn.net/centre10/article/details/5928302)**
[http://blog.csdn.net/centre10/article/details/5928302](http://blog.csdn.net/centre10/article/details/5928302)

http://www.blogjava.net/tangzurui/archive/2008/12/08/244953.html
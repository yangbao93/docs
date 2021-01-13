# 了解基于Token的身份验证的来龙去脉 - SoHi - 博客园
[SoHi]()

* [CnBlogs](http://www.cnblogs.com/)
* [Home](http://www.cnblogs.com/sumahe/)
* [New Post](http://i.cnblogs.com/EditPosts.aspx?opt=1)
* [Contact](http://msg.cnblogs.com/send/SoHi)
* [Admin](http://i.cnblogs.com/)
* [Rss](http://www.cnblogs.com/sumahe/rss) [Posts - 0 Articles - 0 Comments - 0           了解基于Token的身份验证的来龙去脉](http://www.cnblogs.com/sumahe/rss)

**简介**

在Web领域基于Token的身份验证随处可见。在大多数使用Web API的互联网公司中，tokens 是多用户下处理认证的最佳方式。

以下几点特性会让你在程序中使用基于Token的身份验证

1.无状态、可扩展

2.支持移动设备

3.跨程序调用

4.安全

**那些使用基于Token的身份验证的大佬们**

大部分你见到过的API和Web应用都使用tokens。例如Facebook, Twitter, Google+, GitHub等。

**Token的起源**

在介绍基于Token的身份验证的原理与优势之前，不妨先看看之前的认证都是怎么做的。

　　 **基于服务器的验证**

我们都是知道HTTP协议是无状态的，这种无状态意味着程序需要验证每一次请求，从而辨别客户端的身份。
在这之前，程序都是通过在服务端存储的登录信息来辨别请求的。这种方式一般都是通过存储Session来完成。

下图展示了基于服务器验证的原理

![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/image_2.png)
随着Web，应用程序，已经移动端的兴起，这种验证的方式逐渐暴露出了问题。尤其是在可扩展性方面。

**基于服务器验证方式暴露的一些问题**

1.Seesion：每次认证用户发起请求时，服务器需要去创建一个记录来存储信息。当越来越多的用户发请求时，内存的开销也会不断增加。 
 

2.可扩展性：在服务端的内存中使用Seesion存储登录信息，伴随而来的是可扩展性问题。

3.CORS(跨域资源共享)：当我们需要让数据跨多台移动设备上使用时，跨域资源的共享会是一个让人头疼的问题。在使用Ajax抓取另一个域的资源，就可以会出现禁止请求的情况。

4.CSRF(跨站请求伪造)：用户在访问银行网站时，他们很容易受到跨站请求伪造的攻击，并且能够被利用其访问其他的网站。

在这些问题中，可扩展行是最突出的。因此我们有必要去寻求一种更有行之有效的方法。

**基于Token的验证原理**

基于Token的身份验证是无状态的，我们不将用户信息存在服务器或Session中。

这种概念解决了在服务端存储信息时的许多问题

NoSession意味着你的程序可以根据需要去增减机器，而不用去担心用户是否登录。

基于Token的身份验证的过程如下:

1.用户通过用户名和密码发送请求。

2.程序验证。

3.程序返回一个签名的token 给客户端。

4.客户端储存token,并且每次用于每次发送请求。

5.服务端验证token并返回数据。

**每一次请求都需要token。** token应该在HTTP的头部发送从而保证了Http请求无状态。我们同样通过设置服务器属性Access-Control-Allow-Origin:* ，让服务器能接受到来自所有域的请求。需要主要的是，在ACAO头部标明(designating)*时，不得带有像HTTP认证，客户端SSL证书和cookies的证书。

下面的图片解释了过程:

![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/image_8.png)
当我们在程序中认证了信息并取得token之后，我们便能通过这个Token做许多的事情。

我们甚至能基于创建一个基于权限的token传给第三方应用程序，这些第三方程序能够获取到我们的数据（当然只有在我们允许的特定的token）

**Tokens的优势**

**无状态、可扩展**

在客户端存储的Tokens是无状态的，并且能够被扩展。基于这种无状态和不存储Session信息，负载负载均衡器能够将用户信息从一个服务传到其他服务器上。

如果我们将已验证的用户的信息保存在Session中，则每次请求都需要用户向已验证的服务器发送验证信息(称为Session亲和性)。用户量大时，可能会造成

一些拥堵。

但是不要着急。使用tokens之后这些问题都迎刃而解，因为tokens自己hold住了用户的验证信息。

**安全性**

请求中发送token而不再是发送cookie能够防止CSRF(跨站请求伪造)。即使在客户端使用cookie存储token，cookie也仅仅是一个存储机制而不是用于认证。不将信息存储在Session中，让我们少了对session操作。

token是有时效的，一段时间之后用户需要重新验证。我们也不一定需要等到token自动失效，token有撤回的操作，通过token revocataion可以使一个特定的token或是一组有相同认证的token无效。

**可扩展性（）**

Tokens能够创建与其它程序共享权限的程序。例如，能将一个随便的社交帐号和自己的大号(Fackbook或是Twitter)联系起来。当通过服务登录Twitter(我们将这个过程Buffer)时，我们可以将这些Buffer附到Twitter的数据流上(we are allowing Buffer to post to our Twitter stream)。

使用tokens时，可以提供可选的权限给第三方应用程序。当用户想让另一个应用程序访问它们的数据，我们可以通过建立自己的API，得出特殊权限的tokens。

**多平台跨域**

我们提前先来谈论一下CORS(跨域资源共享)，对应用程序和服务进行扩展的时候，需要介入各种各种的设备和应用程序。

Having our API just serve data, we can also make the design choice to serve assets from a CDN. This eliminates the issues that CORS brings up after we set a quick header configuration for our application.

只要用户有一个通过了验证的token，数据和资源就能够在任何域上被请求到。

```
Access-Control-Allow-Origin: *
```

**基于标准**

创建token的时候，你可以设定一些选项。我们在后续的文章中会进行更加详尽的描述，但是标准的用法会在JSON Web Tokens体现。

最近的程序和文档是供给JSON Web Tokens的。它支持众多的语言。这意味在未来的使用中你可以真正的转换你的认证机制。

**总结**

这篇文章仅仅是介绍了为什么选择基于Token的身份验证，并且怎样使用它。

下篇文章《剖析JSON Web Tokens》，我们将会用详细代码的例子来描述如何使用JSON Web Tokens认证Node API

[原文链接](https://scotch.io/tutorials/the-ins-and-outs-of-token-based-authentication#the-problems-with-server-based-authentication)

标签: [tokens authenticatio](http://www.cnblogs.com/sumahe/tag/tokens%20authenticatio/)
绿色通道： [好文要顶](#) [关注我](#) [收藏该文](#) [与我联系](http://msg.cnblogs.com/send/SoHi)  
![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/image_7.png)
  
 
![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/image_9.png)
   [SoHi](http://home.cnblogs.com/u/sumahe/)
[关注 - 13](http://home.cnblogs.com/u/sumahe/followees)
[粉丝 - 3](http://home.cnblogs.com/u/sumahe/followers) 

0
0
(请您对文章做出评价)

posted @ 2015-08-13 14:53 [SoHi](http://www.cnblogs.com/sumahe/) Views(2) Comments(0) [Edit](http://i.cnblogs.com/EditPosts.aspx?postid=4727308) [收藏](http://www.cnblogs.com/sumahe/p/4727308.html#)

[抱歉！发生了错误！麻烦反馈至contact@cnblogs.com    刷新评论]() [刷新页面](http://www.cnblogs.com/sumahe/p/4727308.html#) [返回顶部](http://www.cnblogs.com/sumahe/p/4727308.html#top)
发表评论
昵称：

评论内容：

![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/quote.gif)

![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/image_1.png)

![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/image_10.png)

![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/image_3.png)

![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/InsertCode.gif)

![](%E4%BA%86%E8%A7%A3%E5%9F%BA%E4%BA%8EToken%E7%9A%84%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89%20-%20SoHi%20-%20%E5%8D%9A%E5%AE%A2%E5%9B%AD/img.gif)

[注销](#) 

[Ctrl+Enter快捷键提交]

[【推荐】50万行VC++源码: 大型组态工控、电力仿真CAD与GIS源码库](http://www.ucancode.com/index.htm)
[【推荐】融云即时通讯云－专注为 App 开发者提供IM云服务](http://www.rongcloud.cn/)
[免费集成JPush SDK，让APP快速实现推送功能](https://www.jpush.cn/)

<body style='background:transparent'></body></html>"" style="border: 0px; vertical-align: bottom; visibility: hidden; display: none;">

**最新IT新闻**:
· [一款订餐App的重生记](http://news.cnblogs.com/n/527055/)
· [简化IT程序员工作生活的4个窍门](http://news.cnblogs.com/n/527029/)
· [拉里·佩奇拯救我的20句话，相信也会警醒你](http://news.cnblogs.com/n/527054/)
· [柳传志侄女柳甄“首秀”：Uber中国到底怎么玩](http://news.cnblogs.com/n/527053/)
· [黎瑞刚：烧钱给内容，免费给用户，这样太辛苦](http://news.cnblogs.com/n/527052/)
» [更多新闻...](http://news.cnblogs.com/)

<body style='background:transparent'></body></html>"" style="border: 0px; vertical-align: bottom;">

**最新知识库文章**:
· [关于烂代码的那些事（中）](http://kb.cnblogs.com/page/526769/)
· [关于烂代码的那些事（上）](http://kb.cnblogs.com/page/526768/)
· [作为码农，我们为什么要写作](http://kb.cnblogs.com/page/526625/)
· [今天你写了自动化测试吗](http://kb.cnblogs.com/page/526474/)
· [数学和编程](http://kb.cnblogs.com/page/524548/)

» [更多知识库文章...](http://kb.cnblogs.com/)

### Search

### My Tags

	* [tokens authenticatio](http://www.cnblogs.com/sumahe/tag/tokens%20authenticatio/) (1)

Copyright ©2015 SoHi

http://www.sumahe.cn/HtmlPage_files/img.gif
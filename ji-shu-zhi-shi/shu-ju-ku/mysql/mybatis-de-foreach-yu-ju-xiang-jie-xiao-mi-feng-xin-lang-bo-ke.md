# MyBatis的foreach语句详解\_小蜜蜂\_新浪博客

## MyBatis的foreach语句详解_小蜜蜂_新浪博客

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/topbar_logo.gif) [新浪首页](http://www.sina.com.cn/) [登录](http://blog.sina.com.cn/s/blog_6a0cd5e501011snl.html###) [注册](http://login.sina.com.cn/signup/signupmail.php?entry=blog&amp;srcuid=1779226085&amp;src=blogicp)

## [小蜜蜂的博客](http://blog.sina.com.cn/javaelite)

[http://blog.sina.com.cn/javaelite](http://blog.sina.com.cn/javaelite) [❲订阅❳](http://blog.sina.com.cn/s/blog_6a0cd5e501011snl.html#) [❲手机订阅❳](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md)

[首页](http://blog.sina.com.cn/javaelite) [博文目录](http://blog.sina.com.cn/s/articlelist_1779226085_0_1.html) [图片](http://photo.blog.sina.com.cn/javaelite) [关于我](http://blog.sina.com.cn/s/profile_1779226085.html)

个人资料

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/44591352703973.jpg)

**小蜜蜂**

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif) [微博](http://weibo.com/u/1779226085?source=blog) [加好友](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md) [发纸条](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md)

[写留言](http://blog.sina.com.cn/s/profile_1779226085.html#write) [加关注](http://blog.sina.com.cn/s/blog_6a0cd5e501011snl.html#)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/ten_map.png)

* 博客等级：

  ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/1.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/9.gif)

* 博客积分： [354](http://control.blog.sina.com.cn/blog_rebuild/profile/controllers/points_action.php)

  ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif)

* * 博客访问： **560,054** 关注人气： **38** 获赠金笔： **26** 赠出金笔： **0** 荣誉徽章： ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif)
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif)
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif)
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif)
* 
相关博文

* [伊布进化传说之西甲弱水三千唯有离去](http://blog.sina.com.cn/s/blog_1547de0b90102wq35.html#cre=blogpagepc&amp;mod=f&amp;loc=1&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [用户357031435](http://blog.sina.com.cn/u/357031435)
* [李大霄：大盘会交满意答卷，操作这样的股票是首先！](http://blog.sina.com.cn/s/blog_766be2290102woyo.html#cre=blogpagepc&amp;mod=f&amp;loc=2&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [顾航风雨](http://blog.sina.com.cn/u/1986781737)
* [金牛个股解析（喜欢的朋友加微信cjx014或QQ1073719796私聊）](http://blog.sina.com.cn/s/blog_e09de99d0102wj3l.html#cre=blogpagepc&amp;mod=f&amp;loc=3&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [浩宇论市](http://blog.sina.com.cn/u/3768445341)
* [南海最新局势令美崩溃：中国收获巨好消息](http://blog.sina.com.cn/s/blog_c3d8bb1d0102wt46.html#cre=blogpagepc&amp;mod=f&amp;loc=4&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [好润来1988](http://blog.sina.com.cn/u/3285760797)
* [朴槿惠自食恶果：北京下一决定韩如临大敌](http://blog.sina.com.cn/s/blog_631625de0102wipv.html#cre=blogpagepc&amp;mod=f&amp;loc=5&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [风的渡口1986](http://blog.sina.com.cn/u/1662395870)
* [金牛个股解析{喜欢的朋友加微信cjx014或QQ1073719796}](http://blog.sina.com.cn/s/blog_e09de99d0102wj3b.html#cre=blogpagepc&amp;mod=f&amp;loc=6&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [浩宇论市](http://blog.sina.com.cn/u/3768445341)
* [失去陆客蔡英文转身向日本求助：结果被狠打脸](http://blog.sina.com.cn/s/blog_631625de0102wipl.html#cre=blogpagepc&amp;mod=f&amp;loc=7&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [风的渡口1986](http://blog.sina.com.cn/u/1662395870)
* [如果你想炒股致富：看懂此文终生受益](http://blog.sina.com.cn/s/blog_e2ea51f50102wbtt.html#cre=blogpagepc&amp;mod=f&amp;loc=8&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [杨辉分析](http://blog.sina.com.cn/u/3807007221)
* [个股解析（喜欢的朋友可以添加微信：thk536或者qq:905662750私聊）](http://blog.sina.com.cn/s/blog_b7955c1c0102xjcs.html#cre=blogpagepc&amp;mod=f&amp;loc=9&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [墨羽看盘](http://blog.sina.com.cn/u/3080018972)
* [少妇口述：老公出差后，那晚小叔子上了我的床……](http://blog.sina.com.cn/s/blog_6bd361d90102wlkf.html#cre=blogpagepc&amp;mod=f&amp;loc=10&amp;r=1&amp;doct=0&amp;rfunc=100&amp;tj=none)
* [老羊的往事](http://blog.sina.com.cn/u/1809015257)

  [更多&gt;&gt;](http://blog.sina.com.cn/)

推荐博文

* [“老年嫖娼”的迷思（发布33）](http://blog.sina.com.cn/s/blog_4dd47e5a0102wjhn.html?tj=2?tj=2)
* [白银变态杀人案的启示：连环杀手](http://blog.sina.com.cn/u/1305771610)
* [苏州是上海的一部分](http://blog.sina.com.cn/u/1450802503)
* [连环杀手是怎样炼成的？](http://blog.sina.com.cn/u/1233227211)
* [白银变态系列杀人案远未一锤定音](http://blog.sina.com.cn/u/1417037145)
* [王健林苟且 王思聪诗](http://blog.sina.com.cn/u/1225096303)
* [G20杭州峰会，中国为什么只邀](http://blog.sina.com.cn/u/1653689003)
* [还有多少徐玉玉在哭泣？](http://blog.sina.com.cn/u/1277071452)
* [城管执法如何迈向良法善治](http://blog.sina.com.cn/u/1420398274)
* [患癌教师死成新闻，才能讨回公道](http://blog.sina.com.cn/u/2008260605)
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/3pGV-fxvqctw2634099.jpg)
* * [美如仙境的云海](http://blog.sina.com.cn/s/blog_62da4fd20102ws38.html?tj=1)
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/cn5x-fxvixet4167792.jpg)
* * [到奈良访唐看木寻鹿](http://blog.sina.com.cn/s/blog_4db6a4df0102wwy0.html?tj=1)
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/W0s4-fxvixet4162282.jpg)
* * [乌克兰最美大学](http://blog.sina.com.cn/s/blog_59d8df1a0102wj76.html?tj=1)
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/I9dc-fxvixet4161173.jpg)
* * [钢管舞走进皇家园林](http://blog.sina.com.cn/s/blog_3fabe3290102wocc.html?tj=1)
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/reme-fxvkkcf6369688.jpg)
* * [G20灯光秀美爆了](http://blog.sina.com.cn/s/blog_7533c52c0102wu98.html?tj=1)
* ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/gN28-fxvkkcf6369508.jpg)
* * [阿富汗的豪华大餐啥样](http://blog.sina.com.cn/s/blog_543ad8870102wp4h.html?tj=1)

  .

[查看更多](http://blog.sina.com.cn/) &gt;&gt;

正文 字体大小： [大](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md) **中** [小](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md)

### MyBatis的foreach语句详解

\(2012-04-14 16:39:04\) ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif) [转载▼](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md)

标签：

#### [杂谈](http://search.sina.com.cn/?c=blog&amp;q=%D4%D3%CC%B8&amp;by=tag)

分类： [mybaits](http://blog.sina.com.cn/s/articlelist_1779226085_7_1.html) **MyBatis** **的** **foreach** **语句详解**

**1** 人收藏此文章, [我要收藏](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md) 发表于3个月前 , 已有 **113** 次阅读 共 [**0**](http://my.oschina.net/linuxred/blog/38802#comments#comments) 个评论

**foreach** 的主要用在构建in条件中，它可以在SQL语句中进行迭代一个集合。foreach元素的属性主要有 item，index，collection，open，separator，close。item表示集合中每一个元素进行迭代时的别名，index指 定一个名字，用于表示在迭代过程中，每次迭代到的位置，open表示该语句以什么开始，separator表示在每次进行迭代之间以什么符号作为分隔 符，close表示以什么结束，在使用foreach的时候最关键的也是最容易出错的就是collection属性，该属性是必须指定的，但是在不同情况 下，该属性的值是不一样的，主要有一下3种情况：

1. 如果传入的是单参数且参数类型是一个List的时候，collection属性值为list
2. 如果传入的是单参数且参数类型是一个array数组的时候，collection的属性值为array
3. 如果传入的参数是多个的时候，我们就需要把它们封装成一个Map了， **当然单参数也可以封装成** **map** ，实际上如果你在传入参数的时候，在 [breast](http://www.breast-chest.com/) 里面也是会把它封装成一个Map的，map的key就是参数名，所以这个时候collection属性值就是传入的List或array对象在自己封装的map里面的key

下面分别来看看上述三种情况的示例代码：

1.单参数List的类型：

 select \* from t\_blog where id in

 ＃{item} &lt;/select&gt; 上述collection的值为list，对应的Mapper是这样的 public List dynamicForeachTest\(List ids\); 测试代码： @Test public void dynamicForeachTest\(\) { SqlSession session = Util.getSqlSessionFactory\(\).openSession\(\); BlogMapper blogMapper = session.getMapper\(BlogMapper.class\); List ids = new ArrayList\(\); ids.add\(1\); ids.add\(3\); ids.add\(6\); List blogs = blogMapper.dynamicForeachTest\(ids\); for \(Blog blog : blogs\) System.out.println\(blog\); session.close\(\); } 2.单参数array数组的类型：

 select \* from t\_blog where id in

 ＃{item} &lt;/select&gt; 上述collection为array，对应的Mapper代码： public List dynamicForeach2Test\(int\[\] ids\); 对应的测试代码： @Test public void dynamicForeach2Test\(\) { SqlSession session = Util.getSqlSessionFactory\(\).openSession\(\); BlogMapper blogMapper = session.getMapper\(BlogMapper.class\); int\[\] ids = new int\[\] {1,3,6,9}; List blogs = blogMapper.dynamicForeach2Test\(ids\); for \(Blog blog : blogs\) System.out.println\(blog\); session.close\(\); } 3.自己把参数封装成Map的类型

 select \* from t\_blog where title like "%"＃{title}"%" and id in

 ＃{item} &lt;/select&gt; 上述collection的值为ids，是传入的参数Map的key，对应的Mapper代码： public List dynamicForeach3Test\(Map params\); 对应测试代码： @Test public void dynamicForeach3Test\(\) { SqlSession session = Util.getSqlSessionFactory\(\).openSession\(\); BlogMapper blogMapper = session.getMapper\(BlogMapper.class\); final List ids = new ArrayList\(\); ids.add\(1\); ids.add\(2\); ids.add\(3\); ids.add\(6\); ids.add\(7\); ids.add\(9\); Map params = new HashMap\(\); params.put\("ids", ids\); params.put\("title", "中国"\); List blogs = blogMapper.dynamicForeach3Test\(params\); for \(Blog blog : blogs\) System.out.println\(blog\); session.close\(\); }

分享： [133](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md) ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif) [喜欢8](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md) ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif) [赠金笔阅读\(59221\)┊ 评论](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md) \(10\) _┊_ [收藏](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md) \(4\) _┊_ [转载](http://blog.sina.com.cn/s/blog_6a0cd5e501011snl.html#) [\(22\)](http://blog.sina.com.cn/s/blog_6a0cd5e501011snl.html#) _┊_ [喜欢](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md) [▼](mybatis-de-foreach-yu-ju-xiang-jie-xiao-mi-feng-xin-lang-bo-ke.md) _┊_ [打印](http://blog.sina.com.cn/main_v5/ria/print.html?blog_id=blog_6a0cd5e501011snl) _┊_ [举报](http://blog.sina.com.cn/s/blog_6a0cd5e501011snl.html#) 已投稿到： ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MyBatis的foreach语句详解_小蜜蜂_新浪博客/sg_trans.gif) [排行榜](http://blog.sina.com.cn/lm/114/117/day.html)

前一篇： [MyBatis详解 与配置MyBatis+Spring+MySql](http://blog.sina.com.cn/s/blog_6a0cd5e501011snk.html) 后一篇： [❲转载❳js日期类型date构造函数使用方法详解](http://blog.sina.com.cn/s/blog_6a0cd5e501011so7.html) .

[幻灯播放](http://www.bj.cyberpolice.cn/index.jsp)

[http://simg.sinajs.cn/blog7style/images/common/number/9.gif](http://simg.sinajs.cn/blog7style/images/common/number/9.gif)


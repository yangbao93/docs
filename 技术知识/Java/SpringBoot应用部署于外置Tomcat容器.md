# SpringBoot 应用部署于外置 Tomcat 容器

## SpringBoot 应用部署于外置 Tomcat 容器

### SpringBoot 应用部署于外置 Tomcat 容器

[Java编程精选](springboot-ying-yong-bu-shu-yu-wai-zhi-tomcat-rong-qi.md)  
点击上方“ **Java编程精选** ”，选择“置顶公众号”

关键时刻，第一时间送达！

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20应用部署于外置%20Tomcat%20容器/640.jpg)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20应用部署于外置%20Tomcat%20容器/640.gif)

## 先不说楚枫的这般年纪，能够踏入元武一重说明了什么，最主要的是，楚枫在刚刚踏入核心地带时，明明只是灵武七重，而在这两个月不到的时间，连跳两重修为，又跳过一个大境界，踏入了元武一重，这般进步速度，简直堪称变态啊。

## “这楚枫不简单，原来是一位天才，若是让他继续成长下去，绝对能成为一号人物，不过可惜，他太狂妄了，竟与龚师兄定下生死约战，一年时间，他再厉害也无法战胜龚师兄。”有人认识到楚枫的潜力后，为楚枫感到惋惜。

## “哼，何须一年，此子今日就必败，巫九与龚师兄关系甚好，早就看他不顺眼了，如今他竟敢登上生死台挑战巫九，巫九岂会放过他？”但也有人认为，楚枫今日就已是在劫难逃。

## “何人挑战老子？”就在这时，又是一声爆喝响起，而后一道身影自人群之中掠出，最后稳稳的落在了比斗台上。

## 这位身材瘦弱，身高平平，长得那叫一个猥琐，金钩鼻子蛤蟆眼，嘴巴一张牙带色儿，说话臭气能传三十米，他若是当面对谁哈口气，都能让那人跪在地上狂呕不止。

## 不过别看这位长得不咋地，他在核心地带可是鼎鼎有名，剑道盟创建者，青龙榜第九名，正是巫九是也。

## “你就是巫九？”楚枫眼前一亮，第一次发现，世间还有长得如此奇葩的人。

## 巫九鼻孔一张，大嘴一咧，拍着那干瘪的肚子，得意洋洋的道：“老子就是巫九，你挑战老子？”

## “不是挑战你，是要宰了你。”楚枫冷声笑道。

## “好，老子满足你这个心愿，长老，拿张生死状来，老子今日在这里了解了这小子。”巫九扯开嗓子，对着下方吼了一声。

## 如果他对内门长老这么说话，也就算了，但是敢这么跟核心长老说话的，他可真是算作胆肥的，就连许多核心弟子，都是倒吸了一口凉气，心想这楚枫够狂，想不到这巫九更狂。

## 不过最让人无言的就是，巫九话音落下不久，真有一位核心长老自人群走出，缓缓得来到了比斗台上，左手端着笔墨，右手拿着生死状，来到了巫九的身前。

## “我去，这巫九什么身份，竟能这般使唤核心长老？”有人吃惊不已，那长老修为不低，乃是元武七重，比巫九还要高两个层次，但却这般听巫九的话，着实让人吃惊不已。

## “这你就不知道了吧，巫九在前些时日，拜了钟离长老为师尊，已正式得到钟离长老的亲传。”有人解释道。

## “钟离长老？可是那位性情古怪的钟离一护？”

## “没错，就是他。”

## “天哪，巫九竟然拜入了他的门下？”

## 人们再次大吃一惊，那钟离一护在青龙宗可是赫赫有名，若要是论其个人实力，在青龙宗内绝对能够排入前三，连护宗六老单打独斗都不会是他的对手。

## 只不过那钟离一护，如同诸葛青云一样，也是一位客卿长老，所以在青龙宗内只是挂个头衔，什么事都不管，更别说传授宗内弟子技艺了，如今巫九竟然能拜入他老人家门下，着实让人羡慕不已。

## “恩怨生死台，的确可以决斗生死，但必须要有所恩怨，你们两个人，可有恩怨？”那位长老开口询问道。

> 作者：CodeSheep
>
> [https://my.oschina.net/hansonwang99/blog/1824245](https://my.oschina.net/hansonwang99/blog/1824245)
>
> Java编程精选整理发布，转载请联系作者获得授权

1. 概述

   SpringBoot平时我们用的爽歪歪，爽到它自己连Tomcat都自集成了，我们可以直接编写SBT启动类，然后一键开启内置的Tomcat容器服务，确实是很好上手。但考虑到实际的情形中，我们的Tomcat服务器一般是另外部署好了的，有专门的维护方式。此时我们需要剥离掉SBT应用内置的Tomcat服务器，进而将应用发布并部署到外置的Tomcat容器之中，本文就实践一下这个。

### 2. 修改打包方式

修改项目的pom.xml配置，我们修改其打包方式为war方式，如：

```text
<groupId>com.example</groupId> <artifactId>demo</artifactId> <version>0.0.1-SNAPSHOT</version> <packaging>war</packaging>
```

### 3. 移除SBT自带的嵌入式Tomcat

修改pom.xml，从maven的pom中移除springboot自带的的嵌入式tomcat插件

```text
<dependency>    <groupId>org.springframework.boot</groupId>    <artifactId>spring-boot-starter-web</artifactId>    <!-- 移除嵌入式tomcat插件 -->    <exclusions>        <exclusion>            <groupId>org.springframework.boot</groupId>            <artifactId>spring-boot-starter-tomcat</artifactId>        </exclusion>    </exclusions> </dependency>
```

### 4. 添加servlet-api依赖

修改pom.xml，在maven的pom中添加servlet-api的依赖

```text
<dependency>    <groupId>javax.servlet</groupId>    <artifactId>javax.servlet-api</artifactId>    <version>3.1.0</version>    <scope>provided</scope> </dependency>
```

### 5. 修改启动类，并重写初始化方法

在SpringBoot中我们平常用main方法启动的方式，都有一个SpringBootApplication的启动类，类似代码如下：

```text
@SpringBootApplication public class Application {    public static void main(String[] args) {        SpringApplication.run(Application.class, args);    } }
```

而我们现在需要类似于web.xml的配置方式来启动spring应用，为此，我们在Application类的同级添加一个SpringBootStartApplication类，其代码如下:

```text
// 修改启动类，继承 SpringBootServletInitializer 并重写 configure 方法 public class SpringBootStartApplication extends SpringBootServletInitializer {    @Override    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {        // 注意这里一定要指向原先用main方法执行的Application启动类        return builder.sources(Application.class);    } }
```

### 6. 部署到外部的Tomcat容器并验证

* 在项目根目录下（即包含 `pom.xml` 的目录）记性maven打包操作：

```text
mvn clean package
```

等待打包完成，出现 `[INFO] BUILD SUCCESS` 即为打包成功

* 然后我们把 `target` 目录下生成的 `war` 包放到tomcat的 `webapps` 目录下，启动tomcat，即可自动解压部署。

最后在浏览器中验证:

```text
http://YOUR_IP:[端口号]/[打包项目名]
```

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20应用部署于外置%20Tomcat%20容器/640.jpg)

* 也可以直接将项目命名为ROOT，这样访问根目录即可访问tomcat中的SpringBoot应用

```text
http://YOUR_IP:[端口号]
```

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20应用部署于外置%20Tomcat%20容器/640.png)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20应用部署于外置%20Tomcat%20容器/640.jpg)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20应用部署于外置%20Tomcat%20容器/640.gif)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20应用部署于外置%20Tomcat%20容器/640.gif) [【点击成为源码大神】](https://mp.weixin.qq.com/s?__biz=MzI0NjYxMDQ4OQ==&amp;mid=2247484655&amp;idx=4&amp;sn=b63f0a1e32df2fed7ba7e0f0e838e3f9&amp;scene=21#wechat_redirect)

[http://mp.weixin.qq.com/s?\_\_biz=MzI4MDYwMDc3MQ==&mid=2247485116&idx=1&sn=9c4f7f649e17f2211f130b73492012ec&chksm=ebb74f10dcc0c6064516fbff37bb013f2c8115e13b027f829024ce849100f2a8735fb399b7db&mpshare=1&scene=1&srcid=08055NVVnaWPMWLsdLjRbdQt\#rd](http://mp.weixin.qq.com/s?__biz=MzI4MDYwMDc3MQ==&mid=2247485116&idx=1&sn=9c4f7f649e17f2211f130b73492012ec&chksm=ebb74f10dcc0c6064516fbff37bb013f2c8115e13b027f829024ce849100f2a8735fb399b7db&mpshare=1&scene=1&srcid=08055NVVnaWPMWLsdLjRbdQt#rd)


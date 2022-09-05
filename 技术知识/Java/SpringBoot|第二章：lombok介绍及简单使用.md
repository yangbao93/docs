# SpringBoot | 第二章：lombok 介绍及简单使用

## SpringBoot | 第二章：lombok 介绍及简单使用

oKong [ImportNew](springboot-di-er-zhang-lombok-jie-shao-ji-jian-dan-shi-yong.md)  
（点击上方公众号，可快速关注）

> 来源：oKong ，
>
> blog.lqdev.cn/2018/07/12/springboot/chapter-two/

在去北京培训的时候，讲师说到了lombok这个第三方插件包，使用了之后发现，确实是个神奇，避免了编写很多臃肿的且定式的代码，虽然现代的IDE都能通过快捷键或者右键的方式，使用Generate Getters and Setters快速生成setters/getters，但当某一个字段修改或者添加字段时，又需要重复的操作一遍，但使用了lombok之后。一切都是自动的，除了最常用的生成setters/getters，还有诸如：自动生成toString方法、equals、·haashcode·等，还能快速生成Builder模式的javabean类，实在是方便。程序猿是很懒的，一切重复的工作都想通过脚本或者自动化工具来完成，所以，使用lombok吧。

**为何要使用Lombok**

我们在开发过程中，通常都会定义大量的JavaBean，然后通过IDE去生成其属性的构造器、getter、setter、equals、hashcode、toString方法，当要增加属性或者对某个属性进行改变时，比如命名、类型等，都需要重新去生成上面提到的这些方法。这样重复的劳动没有任何意义，Lombok里面的注解可以轻松解决这些问题。

* 简化冗余的JavaBean代码，使得实体文件很简洁。
* 大大提高JavaBean中方法的执行效率，省去重复的步骤

**Lombok简介**

Lombok是一个可以通过简单的注解形式来帮助我们简化消除一些必须有但显得很臃肿的Java代码的工具，通过使用对应的注解，可以在编译源码的时候生成对应的方法。

官方地址：[https://projectlombok.org/](https://projectlombok.org/) github地址：[https://github.com/rzwitserloot/lombok](https://github.com/rzwitserloot/lombok)

官网对其解释为：

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第二章：lombok%20介绍及简单使用/640.png)

这里简单说下lombok实现的原理：主要是通过抽象语法树\(AST\)，在编译处理后，匹配到有其注解的类，那么注解编译器就会自动去匹配项目中的注解对应到在lombok语法树中的注解文件，并经过自动编译匹配来生成对应类中的getter或者setter方法，达到简化代码的目的。

利用此原理，也可自行编写一些工作中一些经常使用到的，比如实体类转Map对象，map对象转实体类，原本使用Beanutils或者cglib的BeanCopier实现转换，前者使用的是反射的机制，所以性能相对较差，后者是使用修改字节码技术，性能在未使用Converter时基本等同于set和get方法。但说白了还是麻烦，毕竟还需要缓存对象等做到复用等。而使用lombok的形式的话，一切都是自动的，性能基本是没有损失的，由于对AST不熟悉，之后有时间了可以进行插件编写下（去官网提过这个问题，官方回复说，不太符合lombok的使用场景，⊙﹏⊙‖∣，还是自己动手，风衣足食吧~）

**eclipse 安装**

1. 下载 lombok.jar 包
2. 运行lombok.jar包，会自动扫描系统的ide安装情况\(或者手动指定目录\)，点击Install/Update,即可。

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第二章：lombok%20介绍及简单使用/640.jpg)

1. 不运行jar包情况下，可直接指定eclipse.ini文件，设置javaagent属性即可\(第二种方法最后的效果也是这样的。\)：

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第二章：lombok%20介绍及简单使用/640.png)

**Lombok使用**

**添加maven依赖**

> org.projectlomboklombok1.16.20

**常用注解介绍**

1. @Getter / @Setter：可以作用在类上和属性上，放在类上，会对所有的非静态\(non-static\)属性生成Getter/Setter方法，放在属性上，会对该属性生成Getter/Setter方法。并可以指定Getter/Setter方法的访问级别。
2. @EqualsAndHashCode ：默认情况下，会使用所有非瞬态\(non-transient\)和非静态\(non-static\)字段来生成equals和hascode方法，也可以指定具体使用哪些属性。 @ToString 生成toString方法，默认情况下，会输出类名、所有属性，属性会按照顺序输出，以逗号分割。
3. @NoArgsConstructor, @RequiredArgsConstructor and @AllArgsConstructor：无参构造器、部分参数构造器、全参构造器
4.  **@Data：包含@ToString, @EqualsAndHashCode, 所有属性的@Getter, 所有non-final属性的@Setter和@RequiredArgsConstructor的组合，通常情况下，基本上使用这个注解就足够了。**
5. @Budilder：可以进行Builder方式初始化。
6. @Slf4j：等同于：private final Logger logger = LoggerFactory.getLogger\(XXX.class\);简直不能更爽了！一般上用在其他java类上

更多注解说明，可查看：[https://projectlombok.org/features/index.html](https://projectlombok.org/features/index.html)

**简单使用示例**

使用lombok

> @Data
>
> @Builder
>
> @NoArgsConstructor
>
> @AllArgsConstructor
>
> public class Demo {
>
> String code;
>
> String name;
>
> }

等同于

> public class Demo {
>
> String code;
>
> String name;
>
> public static DemoBuilder builder\(\) {
>
> return new DemoBuilder\(\);
>
> }
>
> public String getCode\(\) {
>
> return this.code;
>
> }
>
> public String getName\(\) {
>
> return this.name;
>
> }
>
> public void setCode\(String code\) {
>
> this.code = code;
>
> }
>
> public void setName\(String name\) {
>
> this.name = name;
>
> }
>
> public boolean equals\(Object o\) {
>
> if \(o == this\)
>
> return true;
>
> if \(!\(o instanceof Demo\)\)
>
> return false;
>
> Demo other = \(Demo\) o;
>
> if \(!other.canEqual\(this\)\)
>
> return false;
>
> Object this$code = getCode\(\);
>
> Object other$code = other.getCode\(\);
>
> if \(this$code == null ? other$code != null : !this$code.equals\(other$code\)\)
>
> return false;
>
> Object this$name = getName\(\);
>
> Object other$name = other.getName\(\);
>
> return this$name == null ? other$name == null : this$name.equals\(other$name\);
>
> }
>
> protected boolean canEqual\(Object other\) {
>
> return other instanceof Demo;
>
> }
>
> public int hashCode\(\) {
>
> int PRIME = 59;
>
> int result = 1;
>
> Object $code = getCode\(\);
>
> result = result \* 59 + \($code == null ? 43 : $code.hashCode\(\)\);
>
> Object $name = getName\(\);
>
> return result \* 59 + \($name == null ? 43 : $name.hashCode\(\)\);
>
> }
>
> public String toString\(\) {
>
> return "Demo\(code=" + getCode\(\) + ", name=" + getName\(\) + "\)";
>
> }
>
> public Demo\(\) {
>
> }
>
> public Demo\(String code, String name\) {
>
> this.code = code;
>
> this.name = name;
>
> }
>
> public static class DemoBuilder {
>
> private String code;
>
> private String name;
>
> public DemoBuilder code\(String code\) {
>
> this.code = code;
>
> return this;
>
> }
>
> public DemoBuilder name\(String name\) {
>
> this.name = name;
>
> return this;
>
> }
>
> public Demo build\(\) {
>
> return new Demo\(this.code, this.name\);
>
> }
>
> public String toString\(\) {
>
> return "Demo.DemoBuilder\(code=" + this.code + ", name=" + this.name + "\)";
>
> }
>
> }
>
> }

使用@Slf4j\(摘抄至官网\)

> @Slf4j
>
> public class LogExampleOther {
>
> public static void main\(String... args\) {
>
> log.error\("Something else is wrong here"\);
>
> }
>
> }

常规的

> public class LogExampleOther {
>
> private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger\(LogExampleOther.class\);
>
> public static void main\(String... args\) {
>
> log.error\("Something else is wrong here"\);
>
> }
>
> }

省了多少事！！！少年快使用吧！

**系列**

* [SpringBoot \| 第一章：第一个 SpringBoot 应用](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481504&amp;idx=2&amp;sn=e0cf1bf8b7400a19236a1d0ac5eda2b2&amp;chksm=bd2509df8a5280c98d09b564aa19d0f75014837d842a5d13386389ee514903a4e46cc7104628&amp;scene=21#wechat_redirect)

【关于投稿】

如果大家有原创好文投稿，请直接给公号发送留言。

① 留言格式： 【投稿】+《 文章标题》+ 文章链接

② 示例： 【投稿】《不要自称是程序员，我十多年的 IT 职场总结》：[http://blog.jobbole.com/94148/](http://blog.jobbole.com/94148/)

③ 最后请附上您的个人简介哈~

看完本文有收获？请转发分享给更多人

**关注「ImportNew」，提升Java技能**

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第二章：lombok%20介绍及简单使用/640.jpg)

[文章转载自公众号](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481512&amp;idx=3&amp;sn=8012746719f148d89a57a281ec7e5dda&amp;chksm=bd2509d78a5280c1111bc16859324143324e7932a6609fbff2a5e8604eb284ba0556360a69d0&amp;mpshare=1&amp;scene=1&amp;srcid=0802OtdIjk3HqqEzaUVHjf0F##) ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第二章：lombok%20介绍及简单使用/0.jpg) [一枚趔趄的猿](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481512&amp;idx=3&amp;sn=8012746719f148d89a57a281ec7e5dda&amp;chksm=bd2509d78a5280c1111bc16859324143324e7932a6609fbff2a5e8604eb284ba0556360a69d0&amp;mpshare=1&amp;scene=1&amp;srcid=0802OtdIjk3HqqEzaUVHjf0F##) [阅读原文](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481512&amp;idx=3&amp;sn=8012746719f148d89a57a281ec7e5dda&amp;chksm=bd2509d78a5280c1111bc16859324143324e7932a6609fbff2a5e8604eb284ba0556360a69d0&amp;mpshare=1&amp;scene=1&amp;srcid=0802OtdIjk3HqqEzaUVHjf0F##)

[http://mp.weixin.qq.com/s?\_\_biz=MjM5NzMyMjAwMA==&mid=2651481512&idx=3&sn=8012746719f148d89a57a281ec7e5dda&chksm=bd2509d78a5280c1111bc16859324143324e7932a6609fbff2a5e8604eb284ba0556360a69d0&mpshare=1&scene=1&srcid=0802OtdIjk3HqqEzaUVHjf0F\#rd](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&mid=2651481512&idx=3&sn=8012746719f148d89a57a281ec7e5dda&chksm=bd2509d78a5280c1111bc16859324143324e7932a6609fbff2a5e8604eb284ba0556360a69d0&mpshare=1&scene=1&srcid=0802OtdIjk3HqqEzaUVHjf0F#rd)


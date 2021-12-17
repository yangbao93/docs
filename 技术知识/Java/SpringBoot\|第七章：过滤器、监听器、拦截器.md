# SpringBoot \| 第七章：过滤器、监听器、拦截器

## SpringBoot \| 第七章：过滤器、监听器、拦截器

oKong [ImportNew](springboot-di-qi-zhang-guo-lv-qi-jian-ting-qi-lan-jie-qi.md)  
（点击上方公众号，可快速关注）

> 来源：oKong ，
>
> blog.lqdev.cn/2018/07/19/springboot/chapter-seven/

**前言**

在实际开发过程中，经常会碰见一些比如系统启动初始化信息、统计在线人数、在线用户数、过滤敏高词汇、访问权限控制\(URL级别\)等业务需求。这些对于业务来说一般上是无关的，业务方是无需关系的，业务只需要关系自己内部业务的事情。所以一般上实现以上的功能，都会或多或少的用到今天准备讲解的过滤器、监听器、拦截器来实现以上功能。

**过滤器**

过滤器Filter，是Servlet的的一个实用技术了。可通过过滤器，对请求进行拦截，比如读取session判断用户是否登录、判断访问的请求URL是否有访问权限\(黑白名单\)等。主要还是可对请求进行预处理。接下来介绍下，在springboot如何实现过滤器功能。

**利用WebFilter注解配置**

@WebFilter时Servlet3.0新增的注解，原先实现过滤器，需要在web.xml中进行配置，而现在通过此注解，启动启动时会自动扫描自动注册。

编写Filter类：

> //注册器名称为customFilter,拦截的url为所有
>
> @WebFilter\(filterName="customFilter",urlPatterns={"/\*"}\)
>
> @Slf4j
>
> public class CustomFilter implements Filter{
>
> @Override
>
> public void init\(FilterConfig filterConfig\) throws ServletException {
>
> log.info\("filter 初始化"\);
>
> }
>
> @Override
>
> public void doFilter\(ServletRequest request, ServletResponse response, FilterChain chain\)
>
> throws IOException, ServletException {
>
> // TODO Auto-generated method stub
>
> log.info\("doFilter 请求处理"\);
>
> //对request、response进行一些预处理
>
> // 比如设置请求编码
>
> // request.setCharacterEncoding\("UTF-8"\);
>
> // response.setCharacterEncoding\("UTF-8"\);
>
> //TODO 进行业务逻辑
>
> //链路 直接传给下一个过滤器
>
> chain.doFilter\(request, response\);
>
> }
>
> @Override
>
> public void destroy\(\) {
>
> log.info\("filter 销毁"\);
>
> }
>
> }

然后在启动类加入@ServletComponentScan注解即可。

> @SpringBootApplication
>
> @ServletComponentScan
>
> @Slf4j
>
> public class Chapter7Application {
>
> public static void main\(String\[\] args\) {
>
> SpringApplication.run\(Chapter7Application.class, args\);
>
> log.info\("chapter7 服务启动"\);
>
> }
>
> }

启动后，控制台输出：

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第七章：过滤器、监听器、拦截器/640.jpg)

过滤器已经生效了。但当注册多个过滤器时，无法指定执行顺序的，原本使用web。xml配置过滤器时，是可指定执行顺序的，但使用@WebFilter时，没有这个配置属性的\(需要配合@Order进行\)，所以接下来介绍下通过FilterRegistrationBean进行过滤器的注册。

**–小技巧–**

1. 通过过滤器的名字，进行顺序的约定，比如LogFilter和AuthFilter，此时AuthFilter就会比LogFilter先执行，因为首字母A比L前面。
2. 通过@Order指定执行顺序，值越小，越先执行

**FilterRegistrationBean方式**

FilterRegistrationBean是springboot提供的，此类提供setOrder方法，可以为filter设置排序值，让spring在注册web filter之前排序后再依次注册。

**改写filter**

其实就输出了@webFilter注解即可。其他的都没有变化。

**启动类中利用@bean注册FilterRegistrationBean**

> @Bean
>
> public FilterRegistrationBean filterRegistrationBean\(\) {
>
> FilterRegistrationBean registration = new FilterRegistrationBean\(\);
>
> //当过滤器有注入其他bean类时，可直接通过@bean的方式进行实体类过滤器，这样不可自动注入过滤器使用的其他bean类。
>
> //当然，若无其他bean需要获取时，可直接new CustomFilter\(\)，也可使用getBean的方式。
>
> registration.setFilter\(customFilter\(\)\);
>
> //过滤器名称
>
> registration.setName\("customFilter"\);
>
> //拦截路径
>
> registration.addUrlPatterns\("/\*"\);
>
> //设置顺序
>
> registration.setOrder\(10\);
>
> return registration;
>
> }
>
> @Bean
>
> public Filter customFilter\(\) {
>
> return new CustomFilter\(\);
>
> }

**注册多个时，就注册多个FilterRegistrationBean即可**

启动后，效果和第一种是一样的。

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第七章：过滤器、监听器、拦截器/640.jpg)

**监听器**

Listeeshi是servlet规范中定义的一种特殊类。用于监听servletContext、HttpSession和servletRequest等域对象的创建和销毁事件。监听域对象的属性发生修改的事件。用于在事件发生前、发生后做一些必要的处理。一般是获取在线人数等业务需求。

**创建一个ServletRequest监听器\(其他监听器类似创建\)**

> @WebListener
>
> @Slf4j
>
> public class Customlister implements ServletRequestListener{
>
> @Override
>
> public void requestDestroyed\(ServletRequestEvent sre\) {
>
> log.info\("监听器：销毁"\);
>
> }
>
> @Override
>
> public void requestInitialized\(ServletRequestEvent sre\) {
>
> log.info\("监听器：初始化"\);
>
> }
>
> }

和创建过滤器一样，在启动类中加入@ServletComponentScan进行自动注册即可。

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第七章：过滤器、监听器、拦截器/640.jpg)

**拦截器**

以上的过滤器、监听器都属于Servlet的api，我们在开发中处理利用以上的进行过滤web请求时，还可以使用Spring提供的拦截器\(HandlerInterceptor\)进行更加精细的控制。

编写自定义拦截器类

> @Slf4j
>
> public class CustomHandlerInterceptor implements HandlerInterceptor{
>
> @Override
>
> public boolean preHandle\(HttpServletRequest request, HttpServletResponse response, Object handler\)
>
> throws Exception {
>
> log.info\("preHandle:请求前调用"\);
>
> //返回 false 则请求中断
>
> return true;
>
> }
>
> @Override
>
> public void postHandle\(HttpServletRequest request, HttpServletResponse response, Object handler,
>
> ModelAndView modelAndView\) throws Exception {
>
> log.info\("postHandle:请求后调用"\);
>
> }
>
> @Override
>
> public void afterCompletion\(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex\)
>
> throws Exception {
>
> log.info\("afterCompletion:请求调用完成后回调方法，即在视图渲染完成后回调"\);
>
> }
>
> }

通过继承WebMvcConfigurerAdapter注册拦截器

> @Configuration
>
> public class WebMvcConfigurer extends WebMvcConfigurerAdapter{
>
> @Override
>
> public void addInterceptors\(InterceptorRegistry registry\) {
>
> //注册拦截器 拦截规则
>
> //多个拦截器时 以此添加 执行顺序按添加顺序
>
> registry.addInterceptor\(getHandlerInterceptor\(\)\).addPathPatterns\("/\*"\);
>
> }
>
> @Bean
>
> public static HandlerInterceptor getHandlerInterceptor\(\) {
>
> return new CustomHandlerInterceptor\(\);
>
> }
>
> }

启动后，访问某个url，控制台输出

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第七章：过滤器、监听器、拦截器/640.png)

**请求链路说明**

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第七章：过滤器、监听器、拦截器/640.jpg)

在整个请求的过程，此一图胜千言，希望对此有个深刻的了解，通过不同组合实现不同的业务功能。

**总结**

本章节主要介绍了常用web开发时，会用到的一些常用类，本章节对servlet未进行介绍，平时用的比较少，用法和配置其实和拦截器、监听器是类似的，再次就不阐述了。

**最后**

目前互联网上很多大佬都有SpringBoot系列教程，如有雷同，请多多包涵了。本文是作者在电脑前一字一句敲的，每一步都是实践的。若文中有所错误之处，还望提出，谢谢。

**系列**

* [SpringBoot \| 第一章：第一个 SpringBoot 应用](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481504&amp;idx=2&amp;sn=e0cf1bf8b7400a19236a1d0ac5eda2b2&amp;chksm=bd2509df8a5280c98d09b564aa19d0f75014837d842a5d13386389ee514903a4e46cc7104628&amp;scene=21#wechat_redirect)
* [SpringBoot \| 第二章：lombok 介绍及简单使用](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481512&amp;idx=3&amp;sn=8012746719f148d89a57a281ec7e5dda&amp;chksm=bd2509d78a5280c1111bc16859324143324e7932a6609fbff2a5e8604eb284ba0556360a69d0&amp;scene=21#wechat_redirect)
* [SpringBoot \| 第三章：springboot 配置详解](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481517&amp;idx=2&amp;sn=e545ff871587a33c13c8d5cbe1276e18&amp;chksm=bd2509d28a5280c428462a3c91b56189ecfc503ec10cb1bcd96bd0f3047eebbf5695aea71e1c&amp;scene=21#wechat_redirect)
* [SpringBoot \| 第四章：日志管理](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481528&amp;idx=3&amp;sn=7673a32c148b6f06dc1205e1b83280f9&amp;chksm=bd2509c78a5280d1eb847ef70ced5fabc8cff71b4691164be05407a3101de05598c3c6cfbb64&amp;scene=21#wechat_redirect)
* [SpringBoot \| 第五章：多环境配置](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481532&amp;idx=2&amp;sn=2b03db4e828d5220702771c028048c4f&amp;chksm=bd2509c38a5280d5478d153766bb762b16c5552ca1dd047d62783259a2cac95d54d8b25fb7d6&amp;scene=21#wechat_redirect)
* [SpringBoot \| 第六章：常用注解介绍及简单使用](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481539&amp;idx=3&amp;sn=efac2766e0252006a2ea9cdd97cf3b97&amp;chksm=bd2509bc8a5280aa3d6206cef52fd9487b8d914f453185985571251ec8973ba1fbe4fd2a91dd&amp;scene=21#wechat_redirect)

【关于投稿】

如果大家有原创好文投稿，请直接给公号发送留言。

① 留言格式： 【投稿】+《 文章标题》+ 文章链接

② 示例： 【投稿】《不要自称是程序员，我十多年的 IT 职场总结》：[http://blog.jobbole.com/94148/](http://blog.jobbole.com/94148/)

③ 最后请附上您的个人简介哈~

看完本文有收获？请转发分享给更多人

**关注「ImportNew」，提升Java技能**

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第七章：过滤器、监听器、拦截器/640.jpg)

[文章转载自公众号](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481544&amp;idx=3&amp;sn=44046a4afbe1056799a312228d570aaf&amp;chksm=bd2509b78a5280a10eb6f54e0861ae1c5becf3b5ab8044020690082985787ff387b7cfebe738&amp;mpshare=1&amp;scene=1&amp;srcid=0810WuNh6lvOnnQtrdMYGDwl##) ![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/SpringBoot%20%7C%20第七章：过滤器、监听器、拦截器/0.jpg) [一枚趔趄的猿](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481544&amp;idx=3&amp;sn=44046a4afbe1056799a312228d570aaf&amp;chksm=bd2509b78a5280a10eb6f54e0861ae1c5becf3b5ab8044020690082985787ff387b7cfebe738&amp;mpshare=1&amp;scene=1&amp;srcid=0810WuNh6lvOnnQtrdMYGDwl##) [阅读原文](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481544&amp;idx=3&amp;sn=44046a4afbe1056799a312228d570aaf&amp;chksm=bd2509b78a5280a10eb6f54e0861ae1c5becf3b5ab8044020690082985787ff387b7cfebe738&amp;mpshare=1&amp;scene=1&amp;srcid=0810WuNh6lvOnnQtrdMYGDwl##)

[http://mp.weixin.qq.com/s?\_\_biz=MjM5NzMyMjAwMA==&mid=2651481544&idx=3&sn=44046a4afbe1056799a312228d570aaf&chksm=bd2509b78a5280a10eb6f54e0861ae1c5becf3b5ab8044020690082985787ff387b7cfebe738&mpshare=1&scene=1&srcid=0810WuNh6lvOnnQtrdMYGDwl\#rd](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&mid=2651481544&idx=3&sn=44046a4afbe1056799a312228d570aaf&chksm=bd2509b78a5280a10eb6f54e0861ae1c5becf3b5ab8044020690082985787ff387b7cfebe738&mpshare=1&scene=1&srcid=0810WuNh6lvOnnQtrdMYGDwl#rd)


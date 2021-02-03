# Spring MVC的学习

## Spring MVC的学习

## Spring MVC的学习

spring MVC框架图

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Spring%20MVC的学习/微信截图_20161214132714.png)

spring MVC核心组件图

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Spring%20MVC的学习/微信截图_20161214132726.png)

### spring MVC中主要的注解

1. @controller：负责注册bean到spring上下文
   * @RequestMapping 用于定义访问的url ，method可以定义用以请求的方式
   * @PathVariable 用于定义方法中的参数
   * @ModelAttribute 应用于方法中的参数，参数可以直接在页面中获取
2. @ResponseBody 此注解可以用于方法上，表示返回的类型将直接作为HTTP响应字节流输出
   * @RequestParam 将和url中的参数id进行绑定
   * @SessionAttributes session的管理
   * @RequestBody 指方法参数应该被绑定到HTTP请求的Body上

### spring MVC配置

**指定spring MVC的入口程序（web.xml）**

```text
<servlet>    <servlet-name>springMvc</servlet-name>    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>    <load-on-startup>1</load-on-startup></servlet><servlet-mapping>    <servlet-name>springMvc</servlet-name>    <url-pattern>/</url-pattern>  </servlet-mapping>
```

**springmvc的核心配置文件（在\[servlet-name\]-servlet.xml中）**

```text
<!--mvc注解驱动--><mvc:annotation-driven/><!--使用annotation 自动注册bean, 并保证@Required、@Autowired的属性被注入--><context:component-scan base-package="com.imfbp.loan" /><!--定义JSP文件的位置--> <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"><property name="prefix" value="/WEB-INF/jsp/"></property><!--视图解析器的前缀--><property name="suffix" value=".jsp"></property><!--后缀--></bean>
```

**文件上传**

1. 引入apache-commons-filleupload.jar和apache-commons-io.jar两包
2. 在\[servlet-name\]-servlet.xml中添加下列代码

```text
<!--上传文件配置id起这个名字,文件上传最大值，字节为单位-->  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
```

**ajax请求返回json**

1. 引入jackson-core-asl.jar和jackson-mapper-asl.jar两包
2. 在\[servlet-name\]-servlet.xml中添加下列代码

```text
<!--注解适配器--><bean id="jsonView" class="org.springframework.web.servlet.view.json.MappingJackson2JsonView">         <property name="contentType" value="application/json;charset=UTF-8"/>    <property name="disableCaching" value="false"/></bean>
```

### spring MVC案例

#### 拦截器案例

**1. 拦截器注解**

```text
在[servlet-name]-servlet.xml中添加下列代码<!--配置拦截器--><mvc:interceptors><mvc:interceptor><!--某一模块的拦截：mypath/** --><mvc:mapping path="/**"/>    <bean class="com.yonyou.springmvc.intercepor.MyIntercepor"></bean></mvc:interceptor></mvc:interceptors>
```

**2. MyIntercepor实现HandlerInterceptor 接口，在MyIntercepor类中添加下列方法**

```text
//preHandle方法public boolean preHandle(HttpServletRequest request,  HttpServletResponse response,         Object  handler) throws Exception {  // TODO Auto-generated method stub   return false;   }//postHandle方法public void postHandle(HttpServletRequest request,  HttpServletResponse response,     Object handler,  ModelAndView modelAndView) throws Exception {  // TODO Auto-generated method stub    }  //afterCompletion方法  public void afterCompletion(HttpServletRequest request,  HttpServletResponse response,   Object handler, Exception ex)  throws Exception {     // TODO Auto-generated method stub     }
```

#### spring MVC的跳转

```text
在方法return时直接返回字符串或者把字符串放入ModelAndView里面返回。
```

#### spring MVC的重定向

`在返回的字符前面添加redirect：，如return "redirect:/namespace";`

file://C:\Users\Administrator\Desktop\微信截图\_20161214132714.png


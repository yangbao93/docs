# SSM框架搭建

SSM框架：spring+springMVC+mybatis 组成。 搭建工具选择为idea，jdk版本为1.7

1. pom文件的添加
   1. spring核心包
   2. mybatis包
   3. log4j包等

4.0.0com.muyitest.webmuyitwar1.0-SNAPSHOTmuyit Maven Webapp \[http://maven.apache.org\]\(http://maven.apache.org/\)UTF-8UTF-84.2.5.RELEASE3.2.85.1.291.7.181.2.17junitjunit3.8.1testjstljstl1.2javaxjavaee-api7.0junitjunit4.11testorg.springframeworkspring-core${spring.version}org.springframeworkspring-web${spring.version}org.springframeworkspring-oxm${spring.version}org.springframeworkspring-tx${spring.version}org.springframeworkspring-jdbc${spring.version}org.springframeworkspring-webmvc${spring.version}org.springframeworkspring-context${spring.version}org.springframeworkspring-context-support${spring.version}org.springframeworkspring-aop${spring.version}org.springframeworkspring-test${spring.version}org.mybatismybatis${mybatis.version}org.mybatismybatis-spring1.2.2mysqlmysql-connector-java${mysql-driver.version}commons-dbcpcommons-dbcp1.2.2com.alibabafastjson1.1.41log4jlog4j${log4j.version}org.slf4jslf4j-api${slf4j.version}org.slf4jslf4j-log4j12${slf4j.version}org.codehaus.jacksonjackson-mapper-asl1.9.13com.fasterxml.jackson.corejackson-core2.8.0com.fasterxml.jackson.corejackson-databind2.8.0commons-fileuploadcommons-fileupload1.3.1commons-iocommons-io2.4commons-codeccommons-codec1.9muyit

1. spring-mybatis.xml的配置
2. 配置扫描

> * 引入配置文件

* 创建datasource的bean

>

* 创建sqlSessionFactory，扫描mapper.xml文件

>

* 扫描dao下的文件

>

* 事务管理

>

* xml文件

  &lt;?xml version="1.0" encoding="utf-8" ?&gt;

  &lt;beans xmlns="[http://www.springframework.org/schema/beans"](http://www.springframework.org/schema/beans%22);

  xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance"](http://www.w3.org/2001/XMLSchema-instance%22); xmlns:p="[http://www.springframework.org/schema/p"](http://www.springframework.org/schema/p%22);

  xmlns:context="[http://www.springframework.org/schema/context"](http://www.springframework.org/schema/context%22);

  xmlns:aop="[http://www.springframework.org/schema/aop"](http://www.springframework.org/schema/aop%22); xmlns:tx="[http://www.springframework.org/schema/tx"](http://www.springframework.org/schema/tx%22);

  xmlns:util="[http://www.springframework.org/schema/util"](http://www.springframework.org/schema/util%22);

  xsi:schemaLocation="[http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)

  [http://www.springframework.org/schema/beans/spring-beans-3.2.xsd](http://www.springframework.org/schema/beans/spring-beans-3.2.xsd)

  [http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)

  [http://www.springframework.org/schema/context/spring-context-3.2.xsd](http://www.springframework.org/schema/context/spring-context-3.2.xsd)

  [http://www.springframework.org/schema/tx](http://www.springframework.org/schema/tx)

  [http://www.springframework.org/schema/tx/spring-tx-3.2.xsd](http://www.springframework.org/schema/tx/spring-tx-3.2.xsd)

  [http://www.springframework.org/schema/aop](http://www.springframework.org/schema/aop)

  [http://www.springframework.org/schema/aop/spring-aop-3.2.xsd](http://www.springframework.org/schema/aop/spring-aop-3.2.xsd)

  [http://www.springframework.org/schema/util](http://www.springframework.org/schema/util)

  [http://www.springframework.org/schema/util/spring-util-3.2.xsd"](http://www.springframework.org/schema/util/spring-util-3.2.xsd%22);&gt;

&lt;/beans&gt;

1. web.xml的配置
2. 配置所有xml配置文件

> contextConfigLocation&lt;/param-name&gt;
>
>   
>   
> classpath_:spring-mvc.xml,  
> classpath_:spring-mybatis.xml  
> &lt;/param-value&gt;  
> &lt;/context-param&gt;
>
> * 配置编码过滤器
>
> encodingFilter&lt;/filter-name&gt;
>
> org.springframework.web.filter.CharacterEncodingFilter&lt;/filter-class&gt;
>
> true&lt;/async-supported&gt;
>
> encodingFilter&lt;/param-name&gt;
>
> UTF-8&lt;/param-value&gt;  
> &lt;/init-param&gt;  
> &lt;/filter&gt;
>
> encodingFilter&lt;/filter-name&gt;
>
> /\*&lt;/url-pattern&gt;  
> &lt;/filter-mapping&gt;
>
> * spring监听
>
> org.springframework.web.context.ContextLoaderListener&lt;/listener-class&gt;  
>
>
> * 内存溢出监听
>
> org.springframework.web.util.IntrospectorCleanupListener&lt;/listener-class&gt;  
>
>
> * spring mvc servlet的配置
>
> SpringMVC&lt;/servlet-name&gt;
>
> org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;
>
> contextConfigLocation&lt;/param-name&gt;
>
> classpath:spring-mvc.xml&lt;/param-value&gt;  
> &lt;/init-param&gt;
>
> 1&lt;/load-on-startup&gt;
>
> true&lt;/async-supported&gt;  
> &lt;/servlet&gt;
>
> SpringMVC&lt;/servlet-name&gt;  
>
>
> /&lt;/url-pattern&gt;  
> &lt;/servlet-mapping&gt;
>
> /index.jsp&lt;/welcome-file&gt;  
> &lt;/welcome-file-list&gt;

* web.xml文件

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;web-app xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance"](http://www.w3.org/2001/XMLSchema-instance%22); xmlns="[http://java.sun.com/xml/ns/javaee"](http://java.sun.com/xml/ns/javaee%22); xsi:schemaLocation="[http://java.sun.com/xml/ns/javaee](http://java.sun.com/xml/ns/javaee) [http://java.sun.com/xml/ns/javaee/web-app\_3\_0.xsd"](http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd%22); version="3.0"&gt;

Archetype Created Web Application&lt;/display-name&gt; 

contextConfigLocation&lt;/param-name&gt;

  classpath_:spring-mvc.xml, classpath_:spring-mybatis.xml &lt;/param-value&gt; &lt;/context-param&gt;

encodingFilter&lt;/filter-name&gt;

org.springframework.web.filter.CharacterEncodingFilter&lt;/filter-class&gt;

true&lt;/async-supported&gt;

encodingFilter&lt;/param-name&gt;

UTF-8&lt;/param-value&gt; &lt;/init-param&gt; &lt;/filter&gt;

encodingFilter&lt;/filter-name&gt;

/\*&lt;/url-pattern&gt; &lt;/filter-mapping&gt; 

org.springframework.web.context.ContextLoaderListener&lt;/listener-class&gt; 

org.springframework.web.util.IntrospectorCleanupListener&lt;/listener-class&gt; 

SpringMVC&lt;/servlet-name&gt;

org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;

contextConfigLocation&lt;/param-name&gt;

classpath:spring-mvc.xml&lt;/param-value&gt; &lt;/init-param&gt;

1&lt;/load-on-startup&gt;

true&lt;/async-supported&gt; &lt;/servlet&gt;

SpringMVC&lt;/servlet-name&gt; 

/&lt;/url-pattern&gt; &lt;/servlet-mapping&gt;

/index.jsp&lt;/welcome-file&gt; &lt;/welcome-file-list&gt;

&lt;/web-app&gt;

1. spring-mvc.xml配置

```text
<?xml version="1.0" encoding="UTF-8"?>
```

text/html;charset=UTF-8

about:blank


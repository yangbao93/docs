# SSM框架搭建
SSM框架：spring+springMVC+mybatis 组成。
搭建工具选择为idea，jdk版本为1.7

1. pom文件的添加
	1. spring核心包
	2. mybatis包
	3. log4j包等

<project xmlns="[http://maven.apache.org/POM/4.0.0"](http://maven.apache.org/POM/4.0.0%22); xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance"](http://www.w3.org/2001/XMLSchema-instance%22);
xsi:schemaLocation="[http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0) [http://maven.apache.org/maven-v4_0_0.xsd"](http://maven.apache.org/maven-v4_0_0.xsd%22);>
<modelVersion>4.0.0</modelVersion>
<groupId>com.muyitest.web</groupId>
<artifactId>muyit</artifactId>
<packaging>war</packaging>
<version>1.0-SNAPSHOT</version>
<name>muyit Maven Webapp</name>
<url> [http://maven.apache.org](http://maven.apache.org/) </url>
<properties>
<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

<!-- spring版本号 -->
<spring.version>4.2.5.RELEASE</spring.version>

<!-- mybatis版本号 -->
<mybatis.version>3.2.8</mybatis.version>

<!-- mysql驱动版本号 -->
<mysql-driver.version>5.1.29</mysql-driver.version>

<!-- log4j日志包版本号 -->
<slf4j.version>1.7.18</slf4j.version>
<log4j.version>1.2.17</log4j.version>

</properties>
<dependencies>
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>3.8.1</version>
<scope>test</scope>
</dependency>
<!-- 添加jstl依赖 -->
<dependency>
<groupId>jstl</groupId>
<artifactId>jstl</artifactId>
<version>1.2</version>
</dependency>

<dependency>
<groupId>javax</groupId>
<artifactId>javaee-api</artifactId>
<version>7.0</version>
</dependency>

<!-- 添加junit4依赖 -->
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.11</version>
<!-- 指定范围，在测试时才会加载 -->
<scope>test</scope>
</dependency>

<!-- 添加spring核心依赖 -->
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-core</artifactId>
<version>${spring.version}</version>
</dependency>
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-web</artifactId>
<version>${spring.version}</version>
</dependency>
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-oxm</artifactId>
<version>${spring.version}</version>
</dependency>
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-tx</artifactId>
<version>${spring.version}</version>
</dependency>
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-jdbc</artifactId>
<version>${spring.version}</version>
</dependency>
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-webmvc</artifactId>
<version>${spring.version}</version>
</dependency>
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-context</artifactId>
<version>${spring.version}</version>
</dependency>
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-context-support</artifactId>
<version>${spring.version}</version>
</dependency>
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-aop</artifactId>
<version>${spring.version}</version>
</dependency>

<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-test</artifactId>
<version>${spring.version}</version>
</dependency>

<!-- 添加mybatis依赖 -->
<dependency>
<groupId>org.mybatis</groupId>
<artifactId>mybatis</artifactId>
<version>${mybatis.version}</version>
</dependency>

<!-- 添加mybatis/spring整合包依赖 -->
<dependency>
<groupId>org.mybatis</groupId>
<artifactId>mybatis-spring</artifactId>
<version>1.2.2</version>
</dependency>

<!-- 添加mysql驱动依赖 -->
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>${mysql-driver.version}</version>
</dependency>
<!-- 添加数据库连接池依赖 -->
<dependency>
<groupId>commons-dbcp</groupId>
<artifactId>commons-dbcp</artifactId>
<version>1.2.2</version>
</dependency>

<!-- 添加fastjson -->
<dependency>
<groupId>com.alibaba</groupId>
<artifactId>fastjson</artifactId>
<version>1.1.41</version>
</dependency>

<!-- 添加日志相关jar包 -->
<dependency>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
<version>${log4j.version}</version>
</dependency>
<dependency>
<groupId>org.slf4j</groupId>
<artifactId>slf4j-api</artifactId>
<version>${slf4j.version}</version>
</dependency>
<dependency>
<groupId>org.slf4j</groupId>
<artifactId>slf4j-log4j12</artifactId>
<version>${slf4j.version}</version>
</dependency>

<!-- log end -->
<!-- 映入JSON -->
<dependency>
<groupId>org.codehaus.jackson</groupId>
<artifactId>jackson-mapper-asl</artifactId>
<version>1.9.13</version>
</dependency>
<!-- [https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core) -->
<dependency>
<groupId>com.fasterxml.jackson.core</groupId>
<artifactId>jackson-core</artifactId>
<version>2.8.0</version>
</dependency>
<!-- [https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind) -->
<dependency>
<groupId>com.fasterxml.jackson.core</groupId>
<artifactId>jackson-databind</artifactId>
<version>2.8.0</version>
</dependency>

<dependency>
<groupId>commons-fileupload</groupId>
<artifactId>commons-fileupload</artifactId>
<version>1.3.1</version>
</dependency>

<dependency>
<groupId>commons-io</groupId>
<artifactId>commons-io</artifactId>
<version>2.4</version>
</dependency>

<dependency>
<groupId>commons-codec</groupId>
<artifactId>commons-codec</artifactId>
<version>1.9</version>
</dependency>
</dependencies>
<build>
<finalName>muyit</finalName>
</build>
</project>

1. spring-mybatis.xml的配置

* 配置扫描

> <!--自动扫描-->  
> <context:component-scan base-package="com.muyit.*"/>  
* 引入配置文件

> <bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">  

> <property name="location" value="classpath:/prop/jdbc.properties"/>  

> </bean>  

* 创建datasource的bean

> <bean id="datasource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">  

> <property name="driverClassName" value="${driverClasss}" />  

> <property name="url" value="${jdbcUrl}" />  

> <property name="username" value="${username}" />  

> <property name="password" value="${password}" />  

> <property name="initialSize" value="${initialSize}" />  

> <property name="maxActive" value="${maxActive}" />  

> <property name="maxWait" value="${maxWait}" />  

> <property name="maxIdle" value="${maxIdle}" />  

> <property name="minIdle" value="${minIdle}" />  

> </bean>  

* 创建sqlSessionFactory，扫描mapper.xml文件

> <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean" >  

> <property name="dataSource" ref="datasource" />  

> <!--自动扫描mapper文件-->  

> <property name="mapperLocations" value="classpath:/mapper/*.xml" />  

> </bean>  

* 扫描dao下的文件

> <!--DAO接口所在包名，Spring会自动查找其下的类-->  

> <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">  

> <property name="basePackage" value="com.muyit.dao" />  

> <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />  

> </bean>  

* 事务管理

> <!--事务管理-->  

> <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">  

> <property name="dataSource" ref="datasource" />  

> </bean>  

* xml文件
<?xml version="1.0" encoding="utf-8" ?>
<beans xmlns="[http://www.springframework.org/schema/beans"](http://www.springframework.org/schema/beans%22);
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
[http://www.springframework.org/schema/util/spring-util-3.2.xsd"](http://www.springframework.org/schema/util/spring-util-3.2.xsd%22);>

<!--自动扫描-->
<context:component-scan base-package="com.muyit.*"/>

<!--引入配置文件-->
<bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
<property name="location" value="classpath:/prop/jdbc.properties"/>
</bean>

<bean id="datasource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
<property name="driverClassName" value="${driverClasss}" />
<property name="url" value="${jdbcUrl}" />
<property name="username" value="${username}" />
<property name="password" value="${password}" />
<property name="initialSize" value="${initialSize}" />
<property name="maxActive" value="${maxActive}" />
<property name="maxWait" value="${maxWait}" />
<property name="maxIdle" value="${maxIdle}" />
<property name="minIdle" value="${minIdle}" />
</bean>

<!--spring和MyBatis完美整合，不需要mybatis的配置映射文件-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean" >
<property name="dataSource" ref="datasource" />
<!--自动扫描mapper文件-->
<property name="mapperLocations" value="classpath:/mapper/*.xml" />
</bean>

<!--DAO接口所在包名，Spring会自动查找其下的类-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<property name="basePackage" value="com.muyit.dao" />
<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
</bean>

<!--事务管理-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<property name="dataSource" ref="datasource" />
</bean>

</beans>

1. web.xml的配置

* 配置所有xml配置文件

> <!--添加配置文件的读取-->  
> <context-param>  
> <param-name>contextConfigLocation</param-name>  
> <param-value>  
> <!--classpath*:application.xml,-->  
> classpath*:spring-mvc.xml,  
> classpath*:spring-mybatis.xml  
> </param-value>  
> </context-param>  
* 配置编码过滤器

> <!--编码过滤器-->  
> <filter>  
> <filter-name>encodingFilter</filter-name>  
> <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>  
> <async-supported>true</async-supported>  
> <init-param>  
> <param-name>encodingFilter</param-name>  
> <param-value>UTF-8</param-value>  
> </init-param>  
> </filter>  
> <filter-mapping>  
> <filter-name>encodingFilter</filter-name>  
> <url-pattern>/*</url-pattern>  
> </filter-mapping>  
* spring监听

> <!--spring的监听-->  
> <listener>  
> <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>  
> </listener>  
* 内存溢出监听

> <!--spring防止内存溢出监听器-->  
> <listener>  
> <listener-class>org.springframework.web.util.IntrospectorCleanupListener</listener-class>  
> </listener>  
* spring mvc servlet的配置

> <servlet>  
> <servlet-name>SpringMVC</servlet-name>  
> <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
> <init-param>  
> <param-name>contextConfigLocation</param-name>  
> <param-value>classpath:spring-mvc.xml</param-value>  
> </init-param>  
> <load-on-startup>1</load-on-startup>  
> <async-supported>true</async-supported>  
> </servlet>  
> <servlet-mapping>  
> <servlet-name>SpringMVC</servlet-name>  
> <!--此处可以可以配置成*.do，对应struts的后缀习惯-->  
> <url-pattern>/</url-pattern>  
> </servlet-mapping>  
> <welcome-file-list>  
> <welcome-file>/index.jsp</welcome-file>  
> </welcome-file-list>  

* web.xml文件

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance"](http://www.w3.org/2001/XMLSchema-instance%22);
xmlns="[http://java.sun.com/xml/ns/javaee"](http://java.sun.com/xml/ns/javaee%22);
xsi:schemaLocation="[http://java.sun.com/xml/ns/javaee](http://java.sun.com/xml/ns/javaee) [http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"](http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd%22);
version="3.0">

<display-name>Archetype Created Web Application</display-name>
<!--添加配置文件的读取-->
<context-param>
<param-name>contextConfigLocation</param-name>
<param-value>
<!--classpath*:application.xml,-->
classpath*:spring-mvc.xml,
classpath*:spring-mybatis.xml
</param-value>
</context-param>

<!--编码过滤器-->
<filter>
<filter-name>encodingFilter</filter-name>
<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<async-supported>true</async-supported>
<init-param>
<param-name>encodingFilter</param-name>
<param-value>UTF-8</param-value>
</init-param>
</filter>
<filter-mapping>
<filter-name>encodingFilter</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
<!--spring的监听-->
<listener>
<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<!--spring防止内存溢出监听器-->
<listener>
<listener-class>org.springframework.web.util.IntrospectorCleanupListener</listener-class>
</listener>
<!--spring mvc servlet-->
<servlet>
<servlet-name>SpringMVC</servlet-name>
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<init-param>
<param-name>contextConfigLocation</param-name>
<param-value>classpath:spring-mvc.xml</param-value>
</init-param>
<load-on-startup>1</load-on-startup>
<async-supported>true</async-supported>
</servlet>
<servlet-mapping>
<servlet-name>SpringMVC</servlet-name>
<!--此处可以可以配置成*.do，对应struts的后缀习惯-->
<url-pattern>/</url-pattern>
</servlet-mapping>
<welcome-file-list>
<welcome-file>/index.jsp</welcome-file>
</welcome-file-list>

</web-app>

1. spring-mvc.xml配置

```
<?xml version="1.0" encoding="UTF-8"?>
```

<beans xmlns="[http://www.springframework.org/schema/beans"](http://www.springframework.org/schema/beans%22);
xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance"](http://www.w3.org/2001/XMLSchema-instance%22); xmlns:p="[http://www.springframework.org/schema/p"](http://www.springframework.org/schema/p%22);
xmlns:context="[http://www.springframework.org/schema/context"](http://www.springframework.org/schema/context%22);
xmlns:mvc="[http://www.springframework.org/schema/mvc"](http://www.springframework.org/schema/mvc%22);
xsi:schemaLocation="[http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
[http://www.springframework.org/schema/beans/spring-beans-4.0.xsd](http://www.springframework.org/schema/beans/spring-beans-4.0.xsd)
[http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)
[http://www.springframework.org/schema/context/spring-context-4.0.xsd](http://www.springframework.org/schema/context/spring-context-4.0.xsd)
[http://www.springframework.org/schema/mvc](http://www.springframework.org/schema/mvc)
[http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd"](http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd%22);>
<!-- 自动扫描 controller -->
<context:component-scan base-package="com.muyit.controller"/>

<!--避免IE执行AJAX时，返回JSON出现下载文件 -->
<bean id="mappingJacksonHttpMessageConverter"
class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
<property name="supportedMediaTypes">
<list>
<value>text/html;charset=UTF-8</value>
</list>
</property>
</bean>

<!-- 启动springMVC的注解功能，完成请求和注解POJO的映射 -->
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
<property name="messageConverters">
<list>
<ref bean="mappingJacksonHttpMessageConverter"></ref>
</list>
</property>
</bean>

<!-- 定义跳转的文件的前后缀 ，视图模式配置 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="/WEB-INF/jsp/" />
<property name="suffix" value=".jsp"/>
</bean>

<!-- 文件上传配置 -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
<!-- 默认编码 -->
<property name="defaultEncoding" value="UTF-8"/>
<!-- 上传文件大小限制为31M，31*1024*1024 -->
<property name="maxUploadSize" value="32505856"/>
<!-- 内存中的最大值 -->
<property name="maxInMemorySize" value="4096"/>
</bean>

</beans>

about:blank
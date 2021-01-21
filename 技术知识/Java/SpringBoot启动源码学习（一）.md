# SpringBoot启动源码学习（一）

**写在前面**

​		在Coding的这些日子里，自己总是遇到问题之后才去学习，在解决问题以后，又充满慈悲的把知识放生。除了一些业务解题以外，自己留下的知识比较少。一直以来自己很想阅读源码，但都是不了了之。总结一下除了业务太多、自己懒惰以外，还因为是没有找到很好的学习方式。

​		首先，我是一个比较浮躁的人，做事喜欢能够立竿见影的得到结果。所以在之前的阅读中，总是发现读了好多，什么也没有沉淀，就气急败坏，但源码阅读（知识学习）从来都不是一蹴而就的事情。急功近利只会一败涂地。

​		其次，我的学习是深度优先，所以就会在一个点上深入，到最后阅读半天发现还在前几行中，一天下来觉得进度寥寥。但源码的阅读不能这样，只有先有了全局意识，才能更加了解局部。构建脉络 --> 丰富细节！ 

​		最后，这篇文章仅作为自己源码学习的一个记录，如有不对请不吝指正！感谢大佬~



## SpringBoot

### 什么是SpringBoot？

> **Build Anything with Spring Boot：**Spring Boot is the starting point for building all Spring-based applications. Spring Boot is designed to get you up and running as quickly as possible, with minimal upfront configuration of Spring.

SpringBoot是Spring应用的起点。它更多的是一种设计理念和规则标准。它将很多东西封装起来，我们可以应用各种各样的starter，就可以拆包即用。

### SpringBoot的好处是什么？

1. 简化了Spring应用的开发难度；
2. 有大量的默认配置，使得可以无配置集成主流框架，减少了很多SSM搭建中的繁琐过程
   - web.xml
   - 数据库配置xml，配置日志文件
   - ...

## SpringBoot应用如何启动？

```java
// 该注解标识此类为项目的启动类，打完JAR包以后，去执行时，会执行此类中的main方法
@SpringBootApplication
public class WorkToolsApplication {

    public static void main(String[] args) {
      	// SpringApplication.run方法是程序的入口
        SpringApplication.run(WorkToolsApplication.class, args);
    }

}

```

**@SpringBootApplication**：标记此类是项目的启动类，只需要执行这个注解标记的类中的main方法即可启动类；

**SpringApplication.run()**：run方法是一个静态方法，主要的作用是返回一个SpringApplicationContext（理解为一个应用容器）

## run方法

```java
// 首先进入次方法，官方解释：静态助手，可以使用默认设置从指定的源运行一个SpringApplication
public static ConfigurableApplicationContext run(Class<?> primarySource, String... args) {
		return run(new Class<?>[] { primarySource }, args);
}

// 上面方法调用的最终方法
public static ConfigurableApplicationContext run(Class<?>[] primarySources, String[] args) {
		return new SpringApplication(primarySources).run(args);
}
```

根据上面的代码，我们可以看出run方法只干了两件事：

1. new了一个SpringApplication对象；
2. 执行了SpringApplication对象的run方法，返回了一个可配置的应用上下文（或者说一个Spring应用）；

## 生成一个Spring应用（new SpringApplication()）

```java
// primarySources 为传入的WorkToolsApplication.class
public SpringApplication(Class<?>... primarySources) {
		this(null, primarySources);
}

// 调用的构造方法
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
  	// 根据调用方法得知，此处的resourceLoader 为NULL
		this.resourceLoader = resourceLoader;
 		// 校验传入的primarySources是否为空，此处是WorkToolsApplication.class
		Assert.notNull(primarySources, "PrimarySources must not be null");
    // 将传入的class放入SpringApplication的成员变量中，使用地方待补充 
		this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
    // 获取应用的类型，并赋值给成员变量。获取方法后续讲解
		this.webApplicationType = WebApplicationType.deduceFromClasspath();
    // 初始化引导程序
		this.bootstrappers = new ArrayList<>(getSpringFactoriesInstances(Bootstrapper.class));
    // 设置初始化器
		setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
    // 设置监听器
		setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
    // 推断并设置主要应用的class
		this.mainApplicationClass = deduceMainApplicationClass();
}
```

## SpringApplication的run方法

```java
/**
	 * Run the Spring application, creating and refreshing a new
	 * {@link ApplicationContext}.
	 * @param args the application arguments (usually passed from a Java main method)
	 * @return a running {@link ApplicationContext}
	 */
// 官方解释为 运行一个Spring的应用程序，创建、刷新一个新的ApplicationContext
public ConfigurableApplicationContext run(String... args) {
		StopWatch stopWatch = new StopWatch();
		stopWatch.start();
    // 创建一个引导程序的上下文 
		DefaultBootstrapContext bootstrapContext = createBootstrapContext();
		ConfigurableApplicationContext context = null;
    // 设置无头属性
		configureHeadlessProperty();
    // 获取运行的监听器
		SpringApplicationRunListeners listeners = getRunListeners(args);
  	// 不太理解
		listeners.starting(bootstrapContext, this.mainApplicationClass);
		try {
			ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
      // 创建一个可配置的环境信息
			ConfigurableEnvironment environment = prepareEnvironment(listeners, bootstrapContext, applicationArguments);
      // 在环境信息中配置可以忽视的bean的信息
			configureIgnoreBeanInfo(environment);
      // 横幅打印
			Banner printedBanner = printBanner(environment);
      // 创建一个应用程序的上下文
			context = createApplicationContext();
      // TODO
			context.setApplicationStartup(this.applicationStartup);
      // 准备上下文
			prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
			// 刷新应用上下文，即调用对应程序的上下文的refresh方法
      refreshContext(context);
      // 刷新上下文以后的操作
			afterRefresh(context, applicationArguments);
			stopWatch.stop();
			if (this.logStartupInfo) {
				new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), stopWatch);
			}
			listeners.started(context);
      // 找到所有的runner（实现ApplicationRunner，CommandLineRunner），并执行对应run方法。
			callRunners(context, applicationArguments);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, listeners);
			throw new IllegalStateException(ex);
		}

		try {
      // 所有监听器runing起来，设置其上下文
			listeners.running(context);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, null);
			throw new IllegalStateException(ex);
		}
  	// 返回上下文
		return context;
	}
```

其中比较重要的是：

1. createBootstrapContext 创建引导程序上下文；
2. getRunListeners 获取监听器；
3. prepareEnvironment 准备环境信息；
4. createApplicationContext 创建应用程序上下文；
5. prepareContext 准备上下文；
6. refreshContext 刷新上下文；
7. callRunners 调用runner的方法；



## 总结

这个文章是入门的第一章，通过这篇文章的阅读，我们可以了解，大致上一个SpringApplication 的运行步骤；

1. 利用@SpringBootApplication注解标记一个类，让类中main方法称为程序启动的钥匙；
2. main方法使用SpringAPplication.run() 来初始化一个Spring应用，并设置它的上下文；
   1. new一个SpringApplication对象；
      1. 设置资源自动器resourceLoader；
      2. 设置主要的资源类，即传入的入口类的class；
      3. 设置应用的类型；
      4. 设置引导器；
      5. 初始化并设置初始化器；
      6. 获取并设置监听器实例；
      7. 设置主要的应用class；
   2. 执行这个对象的run方法；
      1. 创建引导器的上下文；
      2. 配置无头属性（暂时觉得不重要）；
      3. 获取运行的监听器；
      4. 利用args创建一个applicationArguments对象；
      5. 使用监听器，引导程序上下文，applicationArguments去创建一个可配置的环境对象；
      6. 环境对象中配置可忽略的bean信息；
      7. 创建一个应用的上下文（最终要返回的上下文）；
      8. 准备这个上下文，利用引导上下文、环境对象、监听器、应用参数等；
      9. 刷新上下文；
      10. 回调用户配置的Runners；
      11. 让监听器，以context运行起来；
      12. 返回这个context；
3. 初始化并运行了一个SpringBoot容器；





## 资料学习

[SpringBoot 源码解析 （一）----- SpringBoot核心原理入门](https://www.cnblogs.com/java-chen-hao/p/11829056.html)

[SpringBoot 源码解析 （二）----- Spring Boot精髓：启动流程源码分析](https://www.cnblogs.com/java-chen-hao/p/11829344.html)


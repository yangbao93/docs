# 阿里巴巴开发规范检查总结

1. 复写方法要加@Override注解
2. if/else/for/while/do语句中必须加大括号，即使只有一行代码；

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/阿里巴巴开发规范检查总结/image_1.png)

1. 不要在foreach中进行元素的remove/add操作，remove元素请用iterator的方式
2. 使用正则表达式时，利用其预编译功能，可以有效加快正则匹配速度

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/阿里巴巴开发规范检查总结/image_2.png)

1. 一个接口的实现类命名最好是以impl为结尾
2. 避免通过一个类的对象引用访问静态变量或静态方法。（增加编译器解析成本，直接通过类名访问即可）

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/阿里巴巴开发规范检查总结/image_8.png) 6. 错误

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/阿里巴巴开发规范检查总结/image_3.png) 6. 正确 7. 类名使用UpperCamelCase风格，必须遵从驼峰形式，但以下情形例外：（领域模型的相关命名）DO / BO / DTO / VO / DAO 8. 方法名、参数名、成员变量、局部变量都统一使用lowerCamelCase，必须遵从驼峰形式 9. 及时清理不再使用的代码段或配置信息。 10. 测试类命名以它要测试的类的名称开始，以Test结尾 11. 集合初始化时，指定集合初始值大小。如map，无法给定大小则默认16

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/阿里巴巴开发规范检查总结/image_6.png)

1. 所有的类都必须添加创建者信息。

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/阿里巴巴开发规范检查总结/image_7.png)

1. 线程池不允许使用Executors去创建，而是通过ThreadPoolExecutor的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/阿里巴巴开发规范检查总结/image_4.png)

1. 必须回收自定义的ThreadLocal变量，尤其在线程池场景下，线程经常会被复用，如果不清理自定义的 ThreadLocal变量，可能会影响后续业务逻辑和造成内存泄露等问题。尽量在代理中使用try-finally块进行回收。

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/阿里巴巴开发规范检查总结/image_5.png)

1. 不能使用过时的类或方法。
2. switch/case语句应该有default，哪怕default什么都没有做。
3. 避免用Apache Beanutils进行属性的copy。（Apache BeanUtils性能较差，可以使用其他方案比如Spring BeanUtils, Cglib BeanCopier。）


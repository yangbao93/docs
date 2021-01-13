# 你真的了解java的lambda吗？- java lambda用法与源码分析
## 你真的了解java的lambda吗？- java lambda用法与源码分析

[Java编程精选]()  
 点击上方“ **Java编程精选** ”，选择“置顶公众号”

关键时刻，第一时间送达！

![](%E4%BD%A0%E7%9C%9F%E7%9A%84%E4%BA%86%E8%A7%A3java%E7%9A%84lambda%E5%90%97%EF%BC%9F-%20java%20lambda%E7%94%A8%E6%B3%95%E4%B8%8E%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/640.jpg)
 
![](%E4%BD%A0%E7%9C%9F%E7%9A%84%E4%BA%86%E8%A7%A3java%E7%9A%84lambda%E5%90%97%EF%BC%9F-%20java%20lambda%E7%94%A8%E6%B3%95%E4%B8%8E%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/640.gif)
 

> https://www.cmlanche.com/2018/07/22/lambda用法与源码分析/  
> Java编程精选整理发布，转载请联系作者获得授权  

# **用法**

 
 

### 示例：最普遍的一个例子，执行一个线程

```
newThread(() -> System.out.print("hello world")).start();
```

->我们发现它指向的是 Runnable接口

```
@FunctionalInterfacepublicinterfaceRunnable {    /**     * When an object implementing interface <code>Runnable</code> is used     * to create a thread, starting the thread causes the object's     * <code>run</code> method to be called in that separately executing     * thread.     * <p>     * The general contract of the method <code>run</code> is that it may     * take any action whatsoever.     *     * @see     java.lang.Thread#run()     */    publicabstractvoid run();}
```

### 
### 

### **分析**

1. ->这个箭头是lambda表达式的关键操作符

2. ->把表达式分成两截，前面是函数参数，后面是函数体。

3. Thread的构造函数接收的是一个Runnable接口对象，而我们这里的用法相当于是把一个函数当做接口对象传递进去了，这点理解很关键，这正是函数式编程的含义所在。

4. 我们注意到Runnable有个注解 @FunctionalInterface，它是jdk8才引入，它的含义是函数接口。它是lambda表达式的协议注解，这个注解非常重要，后面做源码分析会专门分析它的官方注释，到时候一目了然。

```
/* @jls 4.3.2. The Class Object* @jls 9.8 Functional Interfaces* @jls 9.4.3 Interface Method Body* @since 1.8*/@Documented@Retention(RetentionPolicy.RUNTIME)@Target(ElementType.TYPE)public@interfaceFunctionalInterface {}
```

### **由此引发的一些案例**

#### **有参数有返回值的实例：集合排序**

```
List<String> list = newArrayList<>();Collections.sort(list, (o1, o2) -> {    if(o1.equals(o2)) {        return1;    }    return -1;})
```

我们知道Collections.sort方法的第二个参数接受的是一个 Comparator<T>的对象，它的部分关键源码是这样的：

```
@FunctionalInterfacepublicinterfaceComparator<T> {    int compare(T o1, T o2);}
```

如上已经去掉注释和部分其他方法。

我们可以看到sort的第二个参数是Comparator的compare方法，参数类型是T，分别是o1和o2，返回值是一个int。

### **疑问**

 
 

* **上面的示例我们看到接口都有个 @FunctionalInterface的注解，但是我们在实际编程中并没有加这个注解也可以实现lambda表达式，例如：**

```
publicclassMain {interfaceITest {int test(String string);       }staticvoidPrint(ITest test) { test.test("hello world");       }publicstaticvoid main(String[] args) {     Print(string -> {          System.out.println(string);        return0;           });       }   }
```

如上所示，确实不需要增加 @FunctionInterface注解就可以实现。

 
 

* **如果在1中的示例的ITest接口中增加另外一个接口方法，我们会发现不能再用lambda表达式。**

我们带着这两个疑问来进入源码解析。

# **源码解析**

### 必须了解注解 @FunctionInterface

上源码：

```
package java.lang;import java.lang.annotation.*;/** * An informative annotation type used to indicate that an interface * type declaration is intended to be a <i>functional interface</i> as * defined by the Java Language Specification. * * Conceptually, a functional interface has exactly one abstract * method.  Since {@linkplain java.lang.reflect.Method#isDefault() * default methods} have an implementation, they are not abstract.  If * an interface declares an abstract method overriding one of the * public methods of {@code java.lang.Object}, that also does * <em>not</em> count toward the interface's abstract method count * since any implementation of the interface will have an * implementation from {@code java.lang.Object} or elsewhere. * * <p>Note that instances of functional interfaces can be created with * lambda expressions, method references, or constructor references. * * <p>If a type is annotated with this annotation type, compilers are * required to generate an error message unless: * * <ul> * <li> The type is an interface type and not an annotation type, enum, or class. * <li> The annotated type satisfies the requirements of a functional interface. * </ul> * * <p>However, the compiler will treat any interface meeting the * definition of a functional interface as a functional interface * regardless of whether or not a {@code FunctionalInterface} * annotation is present on the interface declaration. * * @jls 4.3.2. The Class Object * @jls 9.8 Functional Interfaces * @jls 9.4.3 Interface Method Body * @since 1.8 */@Documented@Retention(RetentionPolicy.RUNTIME)@Target(ElementType.TYPE)public@interfaceFunctionalInterface {}
```

我们说过这个注解用来规范lambda表达式的使用协议的，那么注释中都说了哪些呢？

* **一种给interface做注解的注解类型，被定义成java语言规范。**

```
* An informative annotation type used to indicate that an interface* type declaration is intended to be a <i>functional interface</i> as* defined by the JavaLanguageSpecification.
```

* **一个被它注解的接口只能有一个抽象方法，有两种例外。**

第一是接口允许有实现的方法。

这种实现的方法是用default关键字来标记的（java反射中java.lang.reflect.Method#isDefault()方法用来判断是否是default方法），例如：

![](%E4%BD%A0%E7%9C%9F%E7%9A%84%E4%BA%86%E8%A7%A3java%E7%9A%84lambda%E5%90%97%EF%BC%9F-%20java%20lambda%E7%94%A8%E6%B3%95%E4%B8%8E%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/640.jpg)
当然这是jdk8才引入的特性，到此我们才知道，知识是一直在变化的，我们在学校中学到interface接口不允许有实现的方法是错误的，随着时间推移，一切规范都有可能发生变化。

如果声明的方法和java.lang.Object中的某个方法一样，它可以不当做未实现的方法，不违背这个原则：一个被它注解的接口只能有一个抽象方法

例如同样是Compartor接口中，它重新声明了equals方法：

![](%E4%BD%A0%E7%9C%9F%E7%9A%84%E4%BA%86%E8%A7%A3java%E7%9A%84lambda%E5%90%97%EF%BC%9F-%20java%20lambda%E7%94%A8%E6%B3%95%E4%B8%8E%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/640.jpg)

这些是对如下注释的翻译和解释。

```
* Conceptually, a functional interface has exactly one abstract* method.  Since {@linkplain java.lang.reflect.Method#isDefault()* default methods} have an implementation, they are not abstract.  If* an interface declares an abstract method overriding one of the* public methods of {@code java.lang.Object}, that also does* <em>not</em> count toward the interface's abstract method count* since any implementation of the interface will have an* implementation from {@code java.lang.Object} or elsewhere.
```

* **如果一个类型被这个注解修饰，那么编译器会要求这个类型必须满足如下条件：**

1. 这个类型必须是一个interface，而不是其他的注解类型、枚举enum或者类class

2. 这个类型必须满足function interface的所有要求，如你个包含两个抽象方法的接口增加这个注解，会有编译错误。

>   

```
* <p>If a type is annotated with this annotation type, compilers are* required to generate an error message unless:** <ul>* <li> The type is an interface type and not an annotation type, enum, or class.* <li> The annotated type satisfies the requirements of a functional interface.* </ul>
```

* **编译器会自动把满足function interface要求的接口自动识别为function interface，所以你才不需要对上面示例中的 ITest接口增加@FunctionInterface注解。**

```
* <p>However, the compiler will treat any interface meeting the* definition of a functional interface as a functional interface* regardless of whether or not a {@codeFunctionalInterface}* annotation is present on the interface declaration.
```

通过了解function interface我们能够知道怎么才能正确的创建一个function interface来做lambda表达式了。接下来的是了解java是怎么把一个函数当做一个对象作为参数使用的。

### **穿越：对象变身函数**

让我们重新复盘一下上面最开始的实例：

```
newThread(() -> System.out.print("hello world")).start();
```

我们知道在jdk8以前我们都是这样来执行的：

```
Runnable r = newRunnable(){    System.out.print("hello world");};newThread(r).start();
```

我们知道两者是等价的，也就是说 r 等价于 ()->System.out.print("hello world")，一个接口对象等于一个lambda表达式？那么lambda表达式肯定做了这些事情（未看任何资料，纯粹推理，有误再改正）：

1. 创建接口对象

2. 实现接口对象

3. 返回接口对象

### **关于 UnaryOperator**

上篇文章（聊一聊JavaFx中的TextFormatter以及一元操作符UnaryOperator）关于 UnaryOperator草草收尾，在这里给大家重新梳理一下，关于它的使用场景以及它与lambda表达式的关系。

##### **使用场景**

要先理解它的作用，它是接受一个参数并返回与该类型同的值，来看一个List怎么用它的，java.util.List中的replaceAll就用它了：

```
defaultvoid replaceAll(UnaryOperator<E> operator) {        Objects.requireNonNull(operator);        finalListIterator<E> li = this.listIterator();        while (li.hasNext()) {            li.set(operator.apply(li.next()));        }    }
```

我们可以看到这个方法的目的是把list中的值经过operator操作后重新返回一个新值，例如具体调用。

```
List<String> list = newArrayList<>();        list.add("abc");        list.replaceAll(s -> s + "efg");        System.out.println(list);
```

其中lambda表达式 s->s+"efg"就是这个operator对象，那么最终list中的值就变成了["abcefg"]，由此我们可以知道它的作用就是对输入的值再加工，并返回同类型的值，怎么用就需要你自己扩展发挥了。

##### **与lambda表达式的关系？**

在我看来，它跟lambda表达式的关系并不大，只是它是jdk内置的一种标准操作，类似的二元操作符 BinaryOperator它可以接受两个同类型参数，并返回同类型参数的值。

### **关于UnaryOperator，我们百尺竿头更进一步，深入到核心**

先贴出它的源码：

```
@FunctionalInterfacepublicinterfaceUnaryOperator<T> extendsFunction<T, T> {    /**     * Returns a unary operator that always returns its input argument.     *     * @param <T> the type of the input and output of the operator     * @return a unary operator that always returns its input argument     */    static <T> UnaryOperator<T> identity() {        return t -> t;    }}
```

我们看到这个function interface居然没有抽象方法，不，不是没有，我们继续看Function接口。

```
@FunctionalInterfacepublicinterfaceFunction<T, R> {    /**     * Applies this function to the given argument.     *     * @param t the function argument     * @return the function result     */    R apply(T t);    /**     * Returns a composed function that first applies the {@code before}     * function to its input, and then applies this function to the result.     * If evaluation of either function throws an exception, it is relayed to     * the caller of the composed function.     *     * @param <V> the type of input to the {@code before} function, and to the     *           composed function     * @param before the function to apply before this function is applied     * @return a composed function that first applies the {@code before}     * function and then applies this function     * @throws NullPointerException if before is null     *     * @see #andThen(Function)     */    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {        Objects.requireNonNull(before);        return (V v) -> apply(before.apply(v));    }    /**     * Returns a composed function that first applies this function to     * its input, and then applies the {@code after} function to the result.     * If evaluation of either function throws an exception, it is relayed to     * the caller of the composed function.     *     * @param <V> the type of output of the {@code after} function, and of the     *           composed function     * @param after the function to apply after this function is applied     * @return a composed function that first applies this function and then     * applies the {@code after} function     * @throws NullPointerException if after is null     *     * @see #compose(Function)     */    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {        Objects.requireNonNull(after);        return (T t) -> after.apply(apply(t));    }    /**     * Returns a function that always returns its input argument.     *     * @param <T> the type of the input and output objects to the function     * @return a function that always returns its input argument     */    static <T> Function<T, T> identity() {        return t -> t;    }}
```

既然他们都被注解为 @FunctionInterface了，那么他们肯定有一个唯一的抽象方法，那就是 apply。

我们知道 ->lambda表达式它是不需要关心函数名字的，所以不管它叫什么， apply也好， apply1也好都可以，但jdk肯定要叫一个更加合理的名字，那么我们知道 s->s+"efg"中 ->调用的就是 apply方法。

而且我们注意到这里有一个 identity()的静态方法，它返回一个Function对象，它其实跟lambda表达式关系也不大，它的作用是返回当前function所要表达的lambda含义。相当于创建了一个自身对象。

Function算是lambda的一种扩展应用，这个Function的的作用，是 Representsafunctionthat accepts one argumentandproduces a result.意思是接受一个参数，并产生（返回）一个结果（类型可不同）。

类似的还有很多Function，都在包java.util.Function中。

![](%E4%BD%A0%E7%9C%9F%E7%9A%84%E4%BA%86%E8%A7%A3java%E7%9A%84lambda%E5%90%97%EF%BC%9F-%20java%20lambda%E7%94%A8%E6%B3%95%E4%B8%8E%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/640.jpg)

你也可以创建自己的Function，它是用来表达操作是怎样的。如传入的参数是什么，返回的是什么。

其实你只要明白它抽象的是操作就可以了。

到此就知道，原来UnaryOperator没啥神秘的，jdk把这些操作放在java.util.function中也正说明了它是一个工具类，是为了提取重复代码，让它可以重用，毕竟需要用到这样的操作的地方太多了，提取是有必要的。

 
![](%E4%BD%A0%E7%9C%9F%E7%9A%84%E4%BA%86%E8%A7%A3java%E7%9A%84lambda%E5%90%97%EF%BC%9F-%20java%20lambda%E7%94%A8%E6%B3%95%E4%B8%8E%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/640.gif)
 

 
![](%E4%BD%A0%E7%9C%9F%E7%9A%84%E4%BA%86%E8%A7%A3java%E7%9A%84lambda%E5%90%97%EF%BC%9F-%20java%20lambda%E7%94%A8%E6%B3%95%E4%B8%8E%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/640.gif)
[【点击成为源码大神】](https://mp.weixin.qq.com/s?__biz=MzI0NjYxMDQ4OQ==&amp;mid=2247484655&amp;idx=4&amp;sn=b63f0a1e32df2fed7ba7e0f0e838e3f9&amp;scene=21#wechat_redirect)

 
 

********▼点击「**** ****阅读原文**** ****」进入程序员商城********

 [阅读原文](https://mp.weixin.qq.com/s?__biz=MzI4MDYwMDc3MQ==&amp;mid=2247485148&amp;idx=1&amp;sn=d508694af4c226d9689bae994c12e105&amp;chksm=ebb74f70dcc0c6668152c18c78bcc0db17d457d41fd917a9da9c047aa80cd1f229d4bf7f9829&amp;mpshare=1&amp;scene=1&amp;srcid=0812ix4ThXIDOIZdfXGBB4AG##)

http://mp.weixin.qq.com/s?__biz=MzI4MDYwMDc3MQ==&mid=2247485148&idx=1&sn=d508694af4c226d9689bae994c12e105&chksm=ebb74f70dcc0c6668152c18c78bcc0db17d457d41fd917a9da9c047aa80cd1f229d4bf7f9829&mpshare=1&scene=1&srcid=0812ix4ThXIDOIZdfXGBB4AG#rd
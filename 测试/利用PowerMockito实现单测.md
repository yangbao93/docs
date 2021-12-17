# 利用PowerMockito实现单测

## PowerMock的用途

PowerMock主要是用于在单测中Mock类，方法。隔绝外部的依赖，注重自己代码的逻辑的工具。 对比Mockito的优势在于可以mock静态类，私有方法

## maven依赖

现有代码中使用依赖如下，后续会根据实际情况增减。

```markup
    <!-- junit包 -->
    <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
        <!-- mockito的包 -->
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>${mockito.version}</version>
            <scope>test</scope>
        </dependency>
        <!-- powermock包 -->
        <dependency>
            <groupId>org.powermock</groupId>
            <artifactId>powermock-module-junit4</artifactId>
            <version>${powermock.version}</version>
            <scope>test</scope>
        </dependency>
        <!-- powermock包 -->
        <dependency>
            <groupId>org.powermock</groupId>
            <artifactId>powermock-api-mockito2</artifactId>
            <version>${powermock.version}</version>
            <scope>test</scope>
        </dependency>
```

## 使用

### mock静态类

1. 指定RunWith为 PowerMockRunner.class；
2. PrePareForTest 要测试的静态类的class，比如我指定了CommonUtil.class；
3. 使用之前需要PowerMockito.mockStatic 需要mock的静态类；
4. 后续使用PowerMockito.when\(方法\).thenReturn\(结果\)即可；

**备注 待验证一个的问题**

**场景**：项目中方法 需要使用很多个静态方法，为了方便开发；写了一个mock类统一在test方法第一步中去mock这些方法，结果在循环中执行了步骤3，发现当mock第二个方法的时候，第一个mock的结果为null了。

**假想**：在第二次执行步骤3时，会重新mock一个静态类，并清除之前的桩点信息，导致结果为null

```java
import com.autonavi.dc.cms.common.mapobj.MapObject;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.Mockito;
import org.powermock.api.mockito.PowerMockito;
import org.powermock.core.classloader.annotations.PrepareForTest;
import org.powermock.modules.junit4.PowerMockRunner;
import tool.common.util.CommonUtil;

@RunWith(PowerMockRunner.class)
@PrepareForTest({CommonUtil.class})
public class StaticMockDemo {

    @Before
    public void init() {
        PowerMockito.mockStatic(CommonUtil.class);
    }

    @Test
    public void testStr2Map() {
        PowerMockito.when(CommonUtil.str2MapObj(Mockito.any())).thenReturn(new MapObject());
    }
}
```

### mock私有方法

在使用过程中也需要对一些私有方法进行mock 1. 利用PowerMockito.spy\(\)方法去mock这个类； 2. 利用when\(spy,"方法名"，参数...\)方法来执行这个私有方法

```java
   @Test
    public void testMockPrivateMethod() {
        MockDemo spy = PowerMockito.spy(new MockDemo());
        PowerMockito.doReturn(new ArrayList<>()).when(spy, "privateMethod", Mockito.any());
    }
```

### mock私有void方法

在一些操作中，我们需要对私有方法进行mock，这个私有方法可能还是void方法，他们只是对一些数据进行处理；

我们会使用这种方法去mock

```java
@Test
public void testMockPrivateVoidMethod(){
  ProductSaveStep demo = PowerMockito.spy(new MockDemo());
  PowerMockito.when(demo, "privateVoidMethod", param1, param2);
}
```

这样的方法mock会导致一个错误，就是抛出未完成异常。在网上都没找到对应的解决方案。故自己研究了一下

```java
// 无法catch住这个异常，导致单测失败
org.mockito.exceptions.misusing.UnfinishedStubbingException: 
Unfinished stubbing detected here:

E.g. thenReturn() may be missing.
Examples of correct stubbing:
    when(mock.isOk()).thenReturn(true);
    when(mock.isOk()).thenThrow(exception);
    doThrow(exception).when(mock).someVoidMethod();
Hints:
 1. missing thenReturn()
 2. you are trying to stub a final method, which is not supported
 3. you are stubbing the behaviour of another mock inside before 'thenReturn' instruction is completed


	at org.junit.runner.notification.RunNotifier$7.notifyListener(RunNotifier.java:191)
	at org.junit.runner.notification.RunNotifier$SafeNotifier.run(RunNotifier.java:61)
	at org.junit.runner.notification.RunNotifier.fireTestFinished(RunNotifier.java:188)
	at org.junit.internal.runners.MethodRoadie.run(MethodRoadie.java:52)
	at org.junit.internal.runners.ClassRoadie.runUnprotected(ClassRoadie.java:33)
	at org.junit.internal.runners.ClassRoadie.runProtected(ClassRoadie.java:45)
	at org.junit.runner.JUnitCore.run(JUnitCore.java:160)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:69)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:33)
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:220)
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:53)
```

告诉我们必须thenReturn()或者是someVoidMethod()才可以。**！！！这个异常无法catch住！！！**

1. 首先我们是void方法，并没有返回值；
2. 其次我们是用的PowerMockito没有someVoidMethod（我没有找到）；

难道我们就只能修改原来的方法，让他成为public 或者 是带return值的吗？

**当然是有其他方法可以处理的**，就是在执行完后增加thenThrow方法，并catch这个异常，抢在框架抛规范异常前，先抛出异常！然后再catch住我们抛出的异常即可。不过需要在catch的时候，细分出我们的异常去catch，不要让流程中的异常也被我们catch掉了~

```java
@Test
public void testMockPrivateVoidMethod(){
  ProductSaveStep demo = PowerMockito.spy(new MockDemo());
  try{
    PowerMockito.when(demo, "privateVoidMethod", param1, param2).thenThrow(new Exception());
  }catch(Exception e){
    // ...
  }
}
```



### 给私有成员变量赋值

```java
@Test
    public void test() {
        MockDemo spy = PowerMockito.spy(new MockDemo());
        //给私有成员变量赋值
        MemberModifier.field(MockDemo.class, "privateFieldX").set(spy, "privateFieldXValue");
    }
```

### 获取私有成员变量的值

```java
@Test
    public void test() {
        MockDemo spy = PowerMockito.spy(new MockDemo());
        MemberModifier.field(MockDemo.class, "privateFieldX").set(spy, "privateFieldXValue");
        // 获取私有变量的值
        String fileValue = (String) MemberModifier.field(MockDemo.class, "privateFieldX").get(spy);
    }
```



## PowerMockito中使用过程中的一些问题

### doReturn\(\).when\(\) 和 when\(\).thenReturn\(\)

**场景：**

 在使用PowerMockito写单测的时候，由于有个方法我需要mock掉，不能进入实际的方法中。于是使用when.. thenReturn的写法，结果一直进入到需要mock的方法中去，报错！

 后来查阅资料后发现，when...doReturn... 会在返回结果之前走真正的方法，只是在结果返回的时候给出return的的数据；如果不希望进入某个方法，可以使用doreturn...when的方式，这样会跳过这个方法执行，直接返回结果。

**参考资料 Stack Overflow** **：**[https://stackoverflow.com/questions/20353846/mockito-difference-between-doreturn-and-when](https://stackoverflow.com/questions/20353846/mockito-difference-between-doreturn-and-when)

> 以下用例也取自Stack Overflow中

**Example：**

```java
// example
public class MyClass {
     protected String methodToBeTested() {
           return anotherMethodInClass();
     }

     protected String anotherMethodInClass() {
          throw new NullPointerException();
     }
}
```

**Test：**

```java
@Spy
private MyClass myClass;

// ...

// would work fine
doReturn("test").when(myClass).anotherMethodInClass();

// would throw a NullPointerException
when(myClass.anotherMethodInClass()).thenReturn("test");
```

**注意：**doReturn\(\).when\(\) 使用，如果when中是私有方法，依旧会去执行这个方法，不会直接跳过



## 常见问题

### UnfinishedStubbingException

**错误日志**

~~~java
org.mockito.exceptions.misusing.UnfinishedStubbingException: 
Unfinished stubbing detected here:
-> at ****(****.java:46)

E.g. thenReturn() may be missing.
Examples of correct stubbing:
    when(mock.isOk()).thenReturn(true);
    when(mock.isOk()).thenThrow(exception);
    doThrow(exception).when(mock).someVoidMethod();
Hints:
 1. missing thenReturn()
 2. you are trying to stub a final method, which is not supported
 3. you are stubbing the behaviour of another mock inside before 'thenReturn' instruction is completed
~~~

**原因**

​	这个也是困扰我很久的问题，我对一个方法使用了doReturn...when... 然后就会一直是这个错误，目前没有找到合适的解决方案。只好把方法给拆开，把这个单独拎开try catch

### InvalidUseOfMatchersException

**错误日志**

~~~java
org.mockito.exceptions.misusing.InvalidUseOfMatchersException: 
Invalid use of argument matchers!
2 matchers expected, 1 recorded:

// 代码
PowerMockito.when(helper.queryName(Mockito.any(), "alias")).thenReturn("for test");
~~~

**原因**

在使用Mockito的传入参数的时候，如果一个使用了，其他的都需要mock。不能一个是正常的，一个是mock的。即所有的参数要么都Matcher，要么都不用Matcher。



其它使用方法待更新...


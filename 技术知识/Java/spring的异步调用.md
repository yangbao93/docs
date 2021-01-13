# spring的异步调用
什么是异步？

例如一个操作，要调用三个方法A,B,C。默认情况下是A完成后执行B，B完成后执行C。如果设置B为异步方法，则不需要B执行完成就开始执行C

异步的配置：

    参考见： [http://www.jianshu.com/p/15aa3cd1a622](http://www.jianshu.com/p/15aa3cd1a622) （spring @Async异步调用，实现异步发邮件）
    参考见： [https://my.oschina.net/u/265943/blog/358223](https://my.oschina.net/u/265943/blog/358223) （spring4 @Async异步实例）
    参考见： [http://fanli7.net/a/bianchengyuyan/C__/20130922/426466.html](http://fanli7.net/a/bianchengyuyan/C__/20130922/426466.html) （spring @Async中Future的使用）

异步的注解：

@EnableAsync 类上的注解，表示该类支持异步调用。

@Async 方法上的注解，表示该方法可以异步调用。

注意的问题：

在同一个类中@Async是不起作用的

future可以获取异步方法的结果：

example：

## ASyncServiceImpl实现类：

package osuya.example;
import java.util.concurrent.Future;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.scheduling.annotation.Async;
import org.springframework.scheduling.annotation.AsyncResult;
import org.springframework.stereotype.Service;
@Service
public class ASyncServiceImpl implements ASyncService {
@Autowired NormalService normalService;
@Async
@Override
public Future<Boolean> async() {
// Demonstrate that our beans are being injected
System.out.println("Managed bean injected: " + (normalService != null));
try {
Thread.sleep(5000);
} catch (InterruptedException e) {
e.printStackTrace();
}
System.out.println("I'm done!");
return new AsyncResult<Boolean>(true);
}
}

调用
package osuya.example;
import java.util.concurrent.Future;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class Spring4Tasks {
@Autowired ASyncService asyncService;
@Autowired NormalService normalService;
public static void main(String[] args) {
ApplicationContext context = new ClassPathXmlApplicationContext("/spring.xml");
Spring4Tasks app = context.getBean(Spring4Tasks.class);
app.start();
System.exit(0);
}
public void start() {
normalService.notAsync();
Future<Boolean> result = asyncService.async();
for(int i = 0; i < 5; i++) normalService.notAsync();
while(!result.isDone()){
// we wait
}
}
}

输出结果

```
Not in a thread

Not in a thread
```

Not in a thread
Not in a thread
Not in a thread
Not in a thread
Managed bean injected: true
I'm done!

http://www.jianshu.com/p/15aa3cd1a622
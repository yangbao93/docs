# 设置bat文件启动时jdk

环境变量中jdk1.6，项目是需要1.7 可以使用如下方法编辑jar

```text
set PATH=C:\Program Files\Java\jdk1.5.0_07\bin;C:\WINDOWS;C:\WINDOWS\COMMAND

set classpath=.;C:\Program Files\Java\jdk1.5.0_07\lib\tools.jar;C:\Program Files\Java\jdk1.5.0_07\lib\dt.jar

java -jar myfile.jar

只需要将红色部分改为自己设置的即可
设置只对当前窗口有效。
```

[https://zhidao.baidu.com/question/87121332.html?fr=push](https://zhidao.baidu.com/question/87121332.html?fr=push)


偶尔需要打包去执行Jar中的某一个类

执行方法如下
# 方法1
```bash
java -classpath 包名 class全路径

# java -classpath work.jar com.jiaban.workday.NoLive
```

# 方法2
```bash
java -cp work.jar class全路径.class

# java -cp work.jar com.jiaban.workday.NoLive.class

```
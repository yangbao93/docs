# Linux的命令操作

查看进程 ps -ef \| grep java --&gt; 红色表示搜寻的东西

杀死进程 kill -9 pid

启动jar包 java -jar \*.jar --&gt; 页面启动，关闭页面或者是ctrl+z会终止

nohup java -jar \*.jar & --&gt;后台启动

启动tomcat ./startup.sh

查看端口占用

```text
netstat -tunlp | grep 端口号
```

## 查找文件中的关键字

> grep “something” 文件位置


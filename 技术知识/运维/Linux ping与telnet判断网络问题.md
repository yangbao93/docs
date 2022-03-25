# 服务器的网络问题

## ping 命令

可以判断当前机器对某个ip是否网络正常

```bash
ping ip 
```

正常：显示延迟信息
不正常：显示 connect refused
## telnet 命令

telnet可以判断当前机器对某个ip的端口是否网络正常

```bash
telnet ip 端口
```

正常：显示conncet to ...
不正常：显示 connect refused

## 参考资料

[Linux ping 测试IP地址与 telnet 测试IP端口 - 拾月凄辰 - 博客园 (cnblogs.com)](https://www.cnblogs.com/FengZeng666/p/15093267.html)
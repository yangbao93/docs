# nginx安装配置域名转发
#### 1.安装pcre

 
![](nginx%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE%E5%9F%9F%E5%90%8D%E8%BD%AC%E5%8F%91/copycode.gif)
 

```
1.[root@localhost home]# tar zxvf pcre-8.10.tar.gz   //解压缩  
2.[root@localhost home]# cd pcre-8.10                //切换到该目录下  
3.[root@localhost pcre-8.10]#./configure            //配置  
4.[root@localhost pcre-8.10]#make    
5.[root@localhost pcre-8.10]#make install           //安装  
6. [root@localhost home]#tar -xvzf zlib-1.2.3.tar.gz
7. [root@localhost home]#cd zlib-1.2.3.tar.gz
8．[root@localhost home]#./configure
9. [root@localhost home]#make
```

 
![](nginx%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE%E5%9F%9F%E5%90%8D%E8%BD%AC%E5%8F%91/copycode.gif)
 

# 2.安装zlib

```
10. [root@localhost home]#sudo make install
```

# 3.安装nginx

 
![](nginx%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE%E5%9F%9F%E5%90%8D%E8%BD%AC%E5%8F%91/copycode.gif)
 

```
11.[root@localhost home]# tar zxvf nginx-1.0.2.tar.gz    

 12.[root@localhost home]#cd nginx-1.0.2    

 13.[root@localhostnginx-1.0.2]#./configure

 14.[root@localhost nginx-1.0.2]#make &&make install
```

 
![](nginx%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE%E5%9F%9F%E5%90%8D%E8%BD%AC%E5%8F%91/copycode.gif)
 

# 4.配置Nginx开机启动

```
15. [root@localhost nginx-1.0.2]#vi /etc/rc.d/rc.local

16. 在文件末尾添加“/usr/local/nginx/sbin/nginx”
```

# 5.启动操作

```
17. /usr/nginx/sbin/nginx (/usr/nginx/sbin/nginx -t 查看配置信息是否正确) 

18. [root@localhost nginx-1.0.2]#cd/usr/nginx/sbin/

19.  [root@localhost nginx-1.0.2]#./nginx
```

# 6.停止操作

停止操作是通过向nginx进程发送信号（什么是信号请参阅linux文 章）来进行的
步骤1：查询nginx主进程号

```
ps -ef | grep nginx
```

在进程列表里 面找master进程，它的编号就是主进程号了。
步骤2：发送信号
从容停止Nginx：
kill -QUIT 主进程号
快速停止Nginx：
kill -TERM 主进程号
强制停止Nginx：
pkill -9 nginx

# 7.平滑重启

```
20. /usr/nginx/sbin/nginx -s reload
```

# 8.配置nginx反向代理用做内网域名转发

1. 
![](nginx%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE%E5%9F%9F%E5%90%8D%E8%BD%AC%E5%8F%91/copycode.gif)
1. 

```
21.     location / {
22.         proxy_pass   http://127.0.0.1:8686/;
23.         proxy_redirect off;
24.         proxy_set_header X-Real-IP $remote_addr;
25.         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
26.     }
```

1. 
![](nginx%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE%E5%9F%9F%E5%90%8D%E8%BD%AC%E5%8F%91/copycode.gif)
1. 

### 注意：

一、配置https需要

```
# cd nginx-1.0.3
# ./configure--with-http_ssl_module –with-openssl=/soft/openssl(此处为openssl解压路径，无需安装)
# make
# make install
```

二、启动nginx提示：error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory，意思是找不到libpcre.so.1这个模块，而导致启动失败。

```
1.    [root@localhost nginx-1.0.2]# ./usr/local/webserver/nginx/sbin/nginx
2.    nginx: error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory
```

要解决这个方法非常容易

如果是32位系统

```
3.    [root@localhost nginx-1.0.2]# ln -s /usr/local/lib/libpcre.so.1 /lib
```

如果是64位系统

```
4.    [root@localhost nginx-1.0.2]# ln -s /usr/local/lib/libpcre.so.1 /lib64
```

然后在启动nginx就OK了

```
5.    [root@localhost nginx-1.0.2]# /usr/local/webserver/nginx/sbin/nginx
```

http://common.cnblogs.com/images/copycode.gif
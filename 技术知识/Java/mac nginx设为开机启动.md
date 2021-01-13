# mac nginx设为开机启动
# 在mac上装好nginx环境，想设为开机自动启动。但是linux上的方式不适用于mac。只能利用Mac系统里的Launchctl来做这个事。

**1、新建文件**
sudo vim /Library/LaunchDaemons/com.nginx.plist

加入

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
<key>Label</key>
<string>com.nginx.plist</string>
<key>ProgramArguments</key>
<array>
<string>/usr/local/nginx/sbin/nginx</string>
</array>
<key>KeepAlive</key>
<false/>
<key>RunAtLoad</key>
<true/>
<key>StandardErrorPath</key>
<string>/usr/local/nginx/logs/error.log</string>
<key>StandardOutPath</key>
<string>/usr/local/nginx/logs/access.log</string>
</dict>
</plist>

**2、修改权限**
sudo chmod 644 /Library/LaunchDaemons/com.nginx.plist

**3、注册为系统服务**
sudo launchctl load -w /Library/LaunchDaemons/com.nginx.plist
卸载为sudo launchctl unload -w /Library/LaunchDaemons/com.nginx.plist

**4、重启后查看nginx是否启动**

```
normanygtekimbp:LaunchDaemons normanyang$ ps -ef | grep nginx
    0   169     1   0  3:07下午 ??         0:00.00 nginx: master process /usr/local/nginx/sbin/nginx
    0   170   169   0  3:07下午 ??         0:00.00 nginx: worker process
  501   928   399   0  3:43下午 ttys000    0:00.00 grep nginx
```

已经启动了，成功！

https://blog.csdn.net/u013091013/article/details/53393406
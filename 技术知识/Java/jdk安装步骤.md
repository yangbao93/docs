# jdk安装步骤
[百度经验](https://jingyan.baidu.com/)  |  [百度知道](http://zhidao.baidu.com/)  |  [百度首页](http://www.baidu.com/)  |  [登录](https://passport.baidu.com/v2/?login)  |  [注册](https://passport.baidu.com/v2/?reg&amp;tpl=exp&amp;u=https%3A%2F%2Fjingyan.baidu.com%2Farticle%2Fa681b0de124c143b184346f2.html)
 
![](jdk%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4/image_2.png)
 
[新闻](http://news.baidu.com/)
[网页](http://www.baidu.com/)
[贴吧](http://tieba.baidu.com/)
[知道](http://zhidao.baidu.com/)
经验
[音乐](http://music.baidu.com/)
[图片](http://image.baidu.com/)
[视频](http://video.baidu.com/)
[地图](http://map.baidu.com/)
[百科](http://baike.baidu.com/)
[文库](http://wenku.baidu.com/)

[帮助](http://www.baidu.com/search/jingyan_help.html)

[发布经验](http://jingyan.baidu.com/edit/content)
首页

分类

任务

回享

商城

特色

知道

[百度经验](https://jingyan.baidu.com/)  >  [游戏/数码](https://jingyan.baidu.com/list/10)  >  [电脑](https://jingyan.baidu.com/list/11)  > 电脑软件

# 如何安装JDK和配置环境变量

# 听语音

|
浏览：1721
|
更新：2015-06-07 01:31
|
标签： [jdk](https://jingyan.baidu.com/tag?tagName=jdk)  

 
![](jdk%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4/image_6.png)
 
1

 
![](jdk%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4/image_8.png)
 
2

 
![](jdk%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4/image_1.png)
 
3

 
![](jdk%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4/image_3.png)
 
4

 
![](jdk%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4/image_7.png)
 
5

 
![](jdk%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4/image_5.png)
 
6

 
![](jdk%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4/image_4.png)
 
7

分步阅读

在Java或android的学习过程中，搭建JDK环境是必须的。今天，小北和大家一块搭建一下JDK环境，并配置环境变量。小北系统是Win8.1，其他系统安装配置步骤也类似。

## 工具/原料

## jdk-8u45-windows-x64

## 步骤一、安装JDK

## 第一步，准备工作，去官网下载JDK安装包（小北是win8.1,64位系统，所有下载了jdk-8u45-windows-x64。

## 第二步，双击jdk安装包，进入安装向导，点击下一步；

## 3
## 第三步，选择安装路径，点击下一步进行安装；

## 4
## 第四步，选择jre安装目录，点击下一步进行安装；安装完成后点击关闭；

## END

## 步骤二、配置环境变量

## 1
## 第一步，右击我的电脑，点击属性（如下图），进入属性面板；

## 2
## 第二步，点击左侧高级系统设置，进入系统属性设置

## 3
## 第三步，点击高级，点击环境变量

## 4
## 第四步，下面系统变量，点击新建；
## 变量名：JAVA_HOME
## 变量值：JDK的安装路径（小北的是E:\Program Files\Java\jdk1.8.0_45）

## 5
## 第五步，系统变量，点击新建；
## 变量名：CLASSPATH
## 变量值：（.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar）
## 前面括号内可复制。注意不要忘记前面的点和中间的分号，分号记得英文输入法。

## 6
## 第六步，系统变量里找到Path（或者path）变量，双击Path（或者选中--编辑），由于原来的变量值已经存在，故应在已有的变量后加上（;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin）括号内可复制。
## 注意前面的分号（将原有的路径与添加的用;隔开）。

## 7
## 第七步，环境变量已经配置完毕。
## 验证方法：Win+R打开运行框，输入cmd命令回车；
## 输入java-version回车，显示java版本信息；输入javac，出现如下画面便说明配置成功；

## END

## 注意事项

## 配置环境变量时记得将原有的和新加的用;隔开，不要出现中文；号

## 经验内容仅供参考，如果您需解决具体问题(尤其法律、医学等领域)，建议您详细咨询相关领域专业人士。
## 举报作者声明：本篇经验系本人依照真实经历原创，未经许可，谢绝转载。
## 投票(3)

## 有得(0)

## 我有疑问(0)

### [换一批](https://jingyan.baidu.com/article/a681b0de124c143b184346f2.html) 相关经验

[win7 64位 安装java jdk1.8 ，修改配置环境变量](https://jingyan.baidu.com/article/d169e186afd233436611d828.html) 1952014.08.30
[怎么安装配置java jdk环境变量](https://jingyan.baidu.com/article/fedf073772643535ac897727.html) 112014.07.15
[Linux下安装jdk并配置环境变量](https://jingyan.baidu.com/article/48a42057f1f0a4a925250464.html) 302016.03.03
[win10下jdk的安装、环境变量的配置与使用](https://jingyan.baidu.com/article/e9fb46e16afa5c7521f7660a.html) 772016.10.25
[windows下安装jdk并配置环境变量](https://jingyan.baidu.com/article/77b8dc7f9d1b1f6174eab6c5.html) 02016.03.01
相关标签 [jdk](https://jingyan.baidu.com/tag?tagName=jdk)

今日支出元
写经验 有钱赚 >>

[enjoy北林野老](https://jingyan.baidu.com/user/npublic?uid=e06c9f77d0d5eee5ced4a08b)

个性签名：个人小站： [xiaowangyun.com](http://xiaowangyun.com/)

### 作者的经验

[如何解决“启用windows功能NetFx3时...](https://jingyan.baidu.com/article/cb5d6105e6d39c005c2fe092.html)
[C#如何解决“是否缺少using指令或程...](https://jingyan.baidu.com/article/bea41d439078dcb4c51be6fb.html)
[SQL Server 2012 附加数据库失败如何...](https://jingyan.baidu.com/article/14bd256e7c337ebb6d26120e.html)
[SVN图标突然消失了怎么办](https://jingyan.baidu.com/article/90895e0fda6b3664ec6b0b8e.html)
[SQL Server 2012如何附加数据库](https://jingyan.baidu.com/article/495ba841e3c23938b30ede35.html)

如要投诉，请到 [百度经验投诉中心](http://tousu.baidu.com/jingyan/add#2) ，如要提出意见、建议， 请到 [百度经验管理吧](http://tieba.baidu.com/f?kw=%B0%D9%B6%C8%BE%AD%D1%E9%B9%DC%C0%ED) 反馈。

## 热门杂志

第1期

你不知道的iPad技巧
3645次分享

第1期

win7电脑那些事
6417次分享

第2期

新人玩转百度经验
1314次分享

第1期

Win8.1实用小技巧
2609次分享

第1期

小白装大神
1824次分享

[新手帮助](https://jingyan.baidu.com/article/09ea3eded6febbc0aede391a.html) [意见反馈](http://jingyan.baidu.com/feedback?fr=pc) [投诉举报](https://help.baidu.com/add?prod_id=13#1)
©2017Baidu   [使用百度前必读](http://www.baidu.com/duty/)    [百度经验协议](http://www.baidu.com/search/jingyan_help.html#经验协议)    [作者创作作品协议](http://www.baidu.com/search/jingyan_editor.html)   京ICP证030173号-1 京网文【2013】0934-983号

登录

1
2

https://jingyan.baidu.com/article/a681b0de124c143b184346f2.html
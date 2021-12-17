# Sublime Text 文本排序&查找重复行&删除重复行\_Linux教程\_Linux公社-Linux系统门户网站

## Sublime Text 文本排序&查找重复行&删除重复行\_Linux教程\_Linux公社-Linux系统门户网站

## Sublime Text 文本排序&查找重复行&删除重复行

\[日期：2016-05-29\] 来源：Linux社区 作者：final \[字体： [大](sublime-text-wen-ben-pai-xu-cha-zhao-zhong-fu-hang-shan-chu-zhong-fu-hang-linux-jiao-cheng-linux-gon.md) [中](sublime-text-wen-ben-pai-xu-cha-zhao-zhong-fu-hang-shan-chu-zhong-fu-hang-linux-jiao-cheng-linux-gon.md) [小](sublime-text-wen-ben-pai-xu-cha-zhao-zhong-fu-hang-shan-chu-zhong-fu-hang-linux-jiao-cheng-linux-gon.md) \]

### 排序

按F9或者选择菜单： `Edit > Sort Lines` ，对每行文本进行排序

### 查找重复行

1. 排序好后，按Ctrl+F，调出查找面板
2. 查找字符串：

```text
^(.+)$[\r\n](^\1$[\r\n]{0, 1})+
```

1. 注意：确保正则模式开关打开；若不可用，按Alt+R进行切换
2. 点击 `Find`

### 删除重复行

1. 排序好后，按Ctrl+H，调出替换面板
2. 查找字符串：

```text
^(.+)$[\r\n](^\1$[\r\n]{0, 1})+
```

1. 注意：确保正则模式开关打开；若不可用，按Alt+R进行切换
2. 替换字符串：

```text
\1
```

1. 点击 `Replace`

**更多Sublime Text相关资讯阅读读** ：

开发者最常用的 8 款 Sublime Text 3 插件 [http://www.linuxidc.com/Linux/2016-02/128719.htm](https://www.linuxidc.com/Linux/2016-02/128719.htm)

[Ubuntu](http://www.linuxidc.com/topicnews.aspx?tid=2) 安装代码编辑器 Sublime Text 3 \(Build 3083\) [http://www.linuxidc.com/Linux/2015-03/115534.htm](https://www.linuxidc.com/Linux/2015-03/115534.htm)

动图展示16个Sublime Text快捷键用法 [http://www.linuxidc.com/Linux/2014-12/110930.htm](https://www.linuxidc.com/Linux/2014-12/110930.htm)

**Ubuntu 12.10 安装破解Sublime Text 2** [http://www.linuxidc.com/Linux/2013-07/86898.htm](https://www.linuxidc.com/Linux/2013-07/86898.htm)

Ubuntu 13.04安装Sublime Text 2 [http://www.linuxidc.com/Linux/2013-05/84228.htm](https://www.linuxidc.com/Linux/2013-05/84228.htm)

编码神器——Sublime Text 包管理工具及扩展大全 [http://www.linuxidc.com/Linux/2013-10/91701.htm](https://www.linuxidc.com/Linux/2013-10/91701.htm)

如何开发 Sublime Text 2 的插件 [http://www.linuxidc.com/Linux/2013-09/90046.htm](https://www.linuxidc.com/Linux/2013-09/90046.htm)

Windows Mac Linux下安装以及破解Sublime Text 2编辑器 [http://www.linuxidc.com/Linux/2013-08/89452.htm](https://www.linuxidc.com/Linux/2013-08/89452.htm)

文本编辑器Sublime Text 使用体验 [http://www.linuxidc.com/Linux/2013-08/89326.htm](https://www.linuxidc.com/Linux/2013-08/89326.htm)

**Sublime Text 的详细介绍** ： [请点这里](https://www.linuxidc.com/Linux/2014-09/106492.htm) **Sublime Text 的下载地址** ： [请点这里](https://www.linuxidc.com/down.aspx?id=1500)

**本文永久更新链接地址** ： [http://www.linuxidc.com/Linux/2016-05/131851.htm](https://www.linuxidc.com/Linux/2016-05/131851.htm)

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Sublime%20Text%20文本排序&查找重复行&删除重复行_Linux教程_Linux公社-Linux系统门户网站/logo.gif)

[https://www.linuxidc.com/linuxfile/logo.gif](https://www.linuxidc.com/linuxfile/logo.gif)


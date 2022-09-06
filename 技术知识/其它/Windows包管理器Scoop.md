# 什么是Scoop？
Scoop是一个windows系统下的包管理器，作用类似于MacOS中的Homebrew。目的是为了方便用户更好的安装，更新，卸载一些软件。减少用户网页搜索，下载，安装，使用，卸载等流程。方便管理电脑上的软件。

**日常安装软件的一些问题**
1. 平时安装软件的时候，我们搜索软件很容易出现各种非官方的网址，很容易下载一些错误的安装包；
2. 软件安装安装时候各种捆绑安装；
3. 没有一个统一的地方提示更新；
4. 卸载的时候，很容易出现卸载不干净的问题；

除了Scoop软件以外，还有winget，Microsoft store等类似的工具。

# 为什么需要包管理器？

- 减少软件安装过程中的搜索、甄别耗时；
- 减少安装过程中的引导选择耗时；
- 统一安装路径、利用命令行方便更新；
- 方便软件的卸载清理，避免残留安装文件夹等；

更高级的使用：回滚到指定版本，软件的备份迁移等。

# Scoop的安装与使用
[Scoop的github链接](https://github.com/ScoopInstaller/Scoop)

## 安装
### Scoop安装前置条件

根据官方的条件要求，你需要：

1.  Windows 7 SP1+或Windows Server 2008+（当然你用主流的Windows 10或Windows 11更没有什么问题）。
2.  [PowerShell 5](https://link.zhihu.com/?target=https%3A//www.microsoft.com/en-us/download/details.aspx%3Fid%3D54616)（及以上，包含[PowerShell Core](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows%3Fview%3Dpowershell-7.2%26viewFallbackFrom%3Dpowershell-6)）和[.NET Framework 4.5](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows%3Fview%3Dpowershell-7.2%26viewFallbackFrom%3Dpowershell-6)（及以上）（当然我相信读者朋友们肯定都满足了，实在未满足可通过文本链接前去下载）。

需要powershell可以运行shell脚本的权限

```shell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### 正式安装
默认安装目录：C:\Users\<username>\scoop

执行命令可以选择：

**命令一**
```powershell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
```
或
**命令二**
```powershell
iwr -useb get.scoop.sh | iex
```

需要更改默认的安装目录，则需要在执行以上命令前添加环境变量的定义，通过执行以下命令完成
```powershell
$env:SCOOP='D:\Applications\Scoop'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
```
## 使用
常见的命令：

```shell
# 查询安装包
scoop search 包名

# 安装安装包
scoop install 包名
scoop install bucket/包名 # 安装指定来源桶下面的软件

# 查看已经安装的包
scoop list 

# 更新安装的包
scoop update 包名
scoop update *  # 更新所有安装的包

# 卸载安装的包
scoop unintall 包名

# 万能使用
scoop help
```

# 其他

## 常见问题
**问题1**
```
raw.githubusercontent.com 解析错误
```
科学上网！！！

**问题2**
```
administor 权限问题
```
不能使用管理员权限安装scoop

## 资料

[# Scoop——也许是Windows平台最好用的软件（包）管理器](https://zhuanlan.zhihu.com/p/463284082)

[# Windows | Scoop软件包管理神器](https://www.limufang.com/post/569.html)
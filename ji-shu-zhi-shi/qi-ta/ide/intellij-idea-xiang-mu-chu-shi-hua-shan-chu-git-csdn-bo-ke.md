# IntelliJ Idea 项目\(初始化删除\) Git - CSDN博客

## IntelliJ Idea 项目\(初始化/删除\) Git - CSDN博客

## IntelliJ Idea 项目\(初始化/删除\) Git

原创 2017年11月17日 20:54:22 标签： [git](http://so.csdn.net/so/search/s.do?q=git&amp;t=blog) / [intellij idea](http://so.csdn.net/so/search/s.do?q=intellij%20idea&amp;t=blog) . .

一. 已有项目导入git 假设项目已存在，项目名为test-git.

1. 创建git仓库
2. 以码云为例，创建test-git项目的仓库.

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 取消沟选"使用Readme文件初始化这个项目"， 点创建。
2. 创建完后，复制仓库地址：[https://gitee.com/sson/test-git.git](https://gitee.com/sson/test-git.git)

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. IntelliJ Idea 项目导入git仓库
2. 先启用版本控制: VCS --&gt; Enable Version Control Integration
3. 选择Git

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 确定之后， 项目中的文件名会变成红色。

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 加入.gitignore文件
2. 右键项目：Git --&gt; Add
3. 这时项目中的文件名会变成绿色。

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 右键项目：Git --&gt; Commit Directory
2. 在文本框Commit Message中输入一段话，比如：init
3. 选择 Commit and Push

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 这时会弹出Code Analysis，点击Commit

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 下一步Push Commits弹框中需要设置项目的git仓库地址
2. 点击Define remote，在Define Remote弹框中输入git仓库的地址

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 点击OK，这时可以选择项目分支了，默认是master
2. 点击Push，完成项目导入

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 这时候，项目中的文件名会恢复成白色

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 到此，项目导入\(初始化\)git完成。
2. 二. 删除项目中的git信息
3. 打开菜单：Preferences --&gt; Version Control
4. 选中项目，删除

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

1. 这时并没有真正删除项目的git信息，还需要到项目的根目录中，删除两个目录：.git 和 .idea

!\[\]\(IntelliJ%20Idea%20%E9%A1%B9%E7%9B%AE\(%E5%88%9D%E5%A7%8B%E5%8C%96%E5%88%A0%E9%99%A4\)%20Git%20-%20CSDN%E5%8D%9A%E5%AE%A2/SouthEast.png\)

[http://img.blog.csdn.net/20171117200918855?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTXJfcmFpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast](http://img.blog.csdn.net/20171117200918855?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTXJfcmFpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


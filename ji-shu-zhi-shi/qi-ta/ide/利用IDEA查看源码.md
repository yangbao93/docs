​		工作中基本上不用关心Java的字节码，但在知识学习的时候，还是会想看看，字节码是什么样子的。那如何使用IDEA快速的查看字节码？**使用javap命令** 

参考资料：[idea （mac 版本）查看java字节码](https://blog.csdn.net/weixin_43235751/article/details/100189174)

**步骤为：**

1. 安装好Java环境；
2. 查看javap命令位置；

```bash
whereis javap
#/usr/bin/javap
```

3. 到IDEA中 --> 打开preference --> 找到Tools --> External Tools -->找到添加；

<img src="https://cdn.jsdelivr.net/gh/yangbao93/pic@main/Documents/1616137084303-1616137084294.png" alt="具体路径" style="zoom:50%;" />

4. 添加后如下图所示，填写名称、程序路径、参数、工作文件夹等信息即可；

![具体图片](https://cdn.jsdelivr.net/gh/yangbao93/pic@main/Documents/1616137272618-1616137272615.png)

```java
// Arguments 填写
-c $FileClass$
// Working directory 填写
$OutputPath$
```

5. 在需要看的代码文件上，右键呼出菜单 --> 选择External Tools --> 选择我们刚才设置的工具

<img src="https://cdn.jsdelivr.net/gh/yangbao93/pic@main/Documents/1616137558458-1616137558455.png" style="zoom:50%;" />

6. 结果如下

<img src="https://cdn.jsdelivr.net/gh/yangbao93/pic@main/Documents/1616137510528-1616137510520.png" style="zoom:67%;" />


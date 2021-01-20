# IDEA 完成对JSON、xml等字符串的编辑



## 背景

在使用idea的时候，我们需要对一些复杂的字符串进行处理，比如JSON格式，XML格式，SQL格式等；由于这些类型复杂，会让我们在单纯String 处理中费时费力。所以，利用好工具，减少这种问题的发生，就比较重要。

我们以JSON举例：

**痛苦的地方：**

1. 增加转译符号；
2. JSON字符串过长的时候，无法知道添加的层级是否正确。如，增加在school.class.studentNo下；

比如：

```java
String tmpJsonStr = "{\"name\":\"zhangsan\"}" ;

// 当我们需要维护的时候，如增加age=18 需要增加转译符号\"age\":18
String tmpJsonStr2 = "{\"name\":\"zhangsan\",\"age\":18}" ;
```



## 解决方案

### 方案一

​	使用IDEA中自带的JSON格式化能力 **Inject language or reference**

#### 使用方法（MAC举例）

##### 方式1 快捷键

1. 在需要格式化的String上 按住 option+enter 键 呼出菜单 选择 Inject language or reference ；

![呼出](https://cdn.jsdelivr.net/gh/yangbao93/pic@main/Documents/1611111872583-1611111872521.png)

2. 点击后弹出语言选择框，此时可以滚动选择支持的格式，也可输入json快捷找到；

![呼出菜单](https://cdn.jsdelivr.net/gh/yangbao93/pic@main/Documents/1611112243463-1611112243461.png)

3. 编辑时，只需要光标在string中，然后按住option+enter ，呼出edit菜单

   ![便捷如下](https://cdn.jsdelivr.net/gh/yangbao93/pic@main/Documents/1611112592320-1611112592316.png)

4. 便捷修改界面如下

   ![编辑界面](https://cdn.jsdelivr.net/gh/yangbao93/pic@main/Documents/1611112639684-1611112639678.png)

**tips**：如果没有Inject language or reference 选项，需要手动去开启；

<img src="https://cdn.jsdelivr.net/gh/yangbao93/pic@main/Documents/1611112765161-1611112765157.png" alt="编辑菜单" style="zoom:100%;" />

**开启方法**：

- option+enter 呼出菜单 
- 选择任一选项，点击右箭头
- 选择Edit intention settings
- 找到并开启 Inject language or reference



##### 方式2  注解的方式

```java
//language="JSON"
String tmpJsonStr = "{\"name\":\"zhangsan\"}" ;

@Language("JSON")
String tmpJsonStr = "{\"name\":\"zhangsan\"}" ;
```



### 方案二

使用sublime text 或 其它网页的json-format 能力去修改；在此文中不做详细描述

举例：http://json.cn/

### 
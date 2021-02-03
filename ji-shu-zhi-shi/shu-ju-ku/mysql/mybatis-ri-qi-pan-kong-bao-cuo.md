# mybatis日期判空报错

 createtime = ＃{createtime}

如上代码会出现：invalid comparison: java.util.Date and java.lang.String 错误

改正方法，只保留非空判断，去掉空字符串判断，即：

 createtime = ＃{createtime}

[http://blog.csdn.net/gang\_strong/article/details/53508104](http://blog.csdn.net/gang_strong/article/details/53508104)


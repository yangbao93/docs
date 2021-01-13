# mybatis日期判空报错
<if test="createtime != null and createtime != '' ">
createtime = ＃{createtime}
</if>

如上代码会出现：invalid comparison: java.util.Date and java.lang.String 错误

改正方法，只保留非空判断，去掉空字符串判断，即：
<if test = "createtime != null">
createtime = ＃{createtime}
</if>

http://blog.csdn.net/gang_strong/article/details/53508104
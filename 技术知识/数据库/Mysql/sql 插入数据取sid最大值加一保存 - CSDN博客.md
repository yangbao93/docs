# sql 插入数据取sid最大值加一保存 - CSDN博客
原

# sql 插入数据取sid最大值加一保存

2016年08月04日 22:07:05  阅读数：2432

```
insert into A(id,sid) 
 values(111111,(select case  when max(sid) IS NULL then '1' else max(sid)+1 end from A)) 
这里要考虑到数据库表内无数据，所以使用max(sid)要先判断下它是否为空，是空就赋值为1，不是空就在max(sid)的基础上+1，
还有就是你的SELECT语句是作为一个值来进行插入的，所以要用括号括上
```

版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/dyzhen/article/details/52123601
个人分类： [SQLServer](https://blog.csdn.net/dyzhen/article/category/3257567) 
相关热词： [sql在与sql](https://blog.csdn.net/yahoo1994/article/details/70568642) [sql的or](https://blog.csdn.net/luman1991/article/details/52450893) [sql的not](https://blog.csdn.net/cho3en1/article/details/53261547) [sql与](https://blog.csdn.net/xujiangdong1992/article/details/77097097) [sql┌](https://blog.csdn.net/vagabond6/article/details/79556968) 
▼查看关于本篇文章更多信息

[上一篇ASP.NET C#各种数据库连接字符串大全——SQLServer、Oracle、Access](https://blog.csdn.net/dyzhen/article/details/51913593) 
[下一篇基本数据结构-队列](https://blog.csdn.net/dyzhen/article/details/52155189) 

https://blog.csdn.net/dyzhen/article/details/52123601
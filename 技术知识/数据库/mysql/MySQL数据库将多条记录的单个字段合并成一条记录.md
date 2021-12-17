# MySQL数据库将多条记录的单个字段合并成一条记录 - Rainyn - 博客园

## MySQL数据库将多条记录的单个字段合并成一条记录 - Rainyn - 博客园

## MySQL数据库将多条记录的单个字段合并成一条记录

念念不忘，必有回响

原SQL

```text
SELECT acc.id,acc.acc_username,acc.acc_showname,T_PM_ROLE.role_name FROM  T_ACCOUNT acc,T_ACCOUNT_R_ROLE accRole ,T_PM_ROLE  WHERE acc.is_active =1 AND (accRole.is_active =1 AND  acc.id = accRole.acc_id) AND accRole.role_id = T_PM_ROLE.id
ORDER BY acc.id
```

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MySQL数据库将多条记录的单个字段合并成一条记录%20-%20Rainyn%20-%20博客园/1115933-20171022175935881-1860546340.png)

结果,有一个人有两个角色，如果想要将两个角色合并该如何呢？

答案：使用 group\_concat函数

**注：group\_concat只有与group by语句同时使用才能产生效果**

```text
SELECT acc.id,acc.acc_username,acc.acc_showname,GROUP_CONCAT(T_PM_ROLE.role_name) FROM  T_ACCOUNT acc,T_ACCOUNT_R_ROLE accRole ,T_PM_ROLE  WHERE acc.is_active =1 AND (accRole.is_active =1 AND  acc.id = accRole.acc_id) AND accRole.role_id = T_PM_ROLE.id

GROUP BY acc_id

ORDER BY acc.id
```

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/数据库/Mysql/MySQL数据库将多条记录的单个字段合并成一条记录%20-%20Rainyn%20-%20博客园/1115933-20171022180403396-1383521139.png)

参考：[http://www.cnblogs.com/wangtao\_20/archive/2011/02/23/1961860.html](http://www.cnblogs.com/wangtao_20/archive/2011/02/23/1961860.html)

[https://www.2cto.com/database/201302/188404.html](https://www.2cto.com/database/201302/188404.html)

```text
感谢您的阅读，如果您觉得阅读本文对您有帮助，请点一下“推荐”按钮。本文欢迎各位转载，但是转载文章之后必须在文章页面中给出作者和原文连接。
```

Measure Measure

[https://images2017.cnblogs.com/blog/1115933/201710/1115933-20171022180403396-1383521139.png](https://images2017.cnblogs.com/blog/1115933/201710/1115933-20171022180403396-1383521139.png)


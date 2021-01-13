# mybatis/mysql数据库知识整理
1. CONCAT:::字符串的拼接::

SELECT
COUNT(id) AS fileCount
FROM
rec_file
WHERE
CONCAT(account_year, account_month) & gt;
= CONCAT(
＃{beginYear},＃{beginMonth})
AND CONCAT(account_year, account_month) & lt;
= CONCAT(
＃{endYear},＃{endMonth})
AND org_id = ＃{orgId} AND status = ＃{status}

1. STRCMP:::字符串的比较:: ，strcmp(expr1,expr2)

expr1 == expr2 的时候，返回0；
expr1 <  expr2 的时候，返回-1；
other，返回1；
1. 数据库因为 ::错误链接过多被锁:: ，要求flust host；
	1. 找能登录的进入数据库，打开查询输入flush hosts;
	2. 参考 [http://www.cnblogs.com/susuyu/archive/2013/05/28/3104249.html](http://www.cnblogs.com/susuyu/archive/2013/05/28/3104249.html)

1. ::1093:: -You can’t specify target table for update in FROM clause

不能对进行查询操作的表进行update操作，也就说我们的where条件中进行了子查询，并且子查询也是针对需要进行update操作的表的，mysql不支持这种查询修改的方式。可以考虑修改为一下方式

update table_a set table_a.a1 = '123' where a1 in (select a1 from (select a1 from table_a where id = 'x') as temp_table)

1. mybatis中部分 ::符号的转义::
	1. 通过转义
		* 大于号（>）：&gt;  意思：greater than
		* 小于号（<）：&lt; 意思：less than
		* 和（&）：&amp;
		* 单引号（'）：&apos;
		* 双引号（"）：&quot;
		* 不等：ne/neq  意思：no equal
	2. 通过<![CDATA[ ]]>进行说明

1. explain 分析代码效率
	1. 关于参数type的介绍
		* all，即为全表扫描
		* index，按索引次序扫描，先读索引，再读实际行，虽然是全表扫描，但避免了排序，因为索引排序好了
		* range，以范围形式扫描
		* ref，非唯一索引访问（只有普通索引）
		* eq_ref，使用唯一索引查找，主键或唯一索引
		* const，常量查询
		* system，系统查询
		* null，优化过程中已经得到了结果，不在访问表或索引
		* possible_key，可能用到索引
	2. 参考请见 [http://blog.csdn.net/b1303110335/article/details/51174540](http://blog.csdn.net/b1303110335/article/details/51174540)

2. 格式化日期，date_formate(date,'formate')
	1. 举例：date_formate（date，'%Y-%m-%d'）
	2. 参数选择

%a
缩写星期名
%b
缩写月名
%c
月，数值
%D
带有英文前缀的月中的天
%d
月的天，数值(00-31)
%e
月的天，数值(0-31)
%f
微秒
%H
小时 (00-23)
%h
小时 (01-12)
%I
小时 (01-12)
%i
分钟，数值(00-59)
%j
年的天 (001-366)
%k
小时 (0-23)
%l
小时 (1-12)
%M
月名
%m
月，数值(00-12)
%p
AM 或 PM
%r
时间，12-小时（hh:mm:ss AM 或 PM）
%S
秒(00-59)
%s
秒(00-59)
%T
时间, 24-小时 (hh:mm:ss)
%U
周 (00-53) 星期日是一周的第一天
%u
周 (00-53) 星期一是一周的第一天
%V
周 (01-53) 星期日是一周的第一天，与 %X 使用
%v
周 (01-53) 星期一是一周的第一天，与 %x 使用
%W
星期名
%w
周的天 （0=星期日, 6=星期六）
%X
年，其中的星期日是周的第一天，4 位，与 %V 使用
%x
年，其中的星期一是周的第一天，4 位，与 %v 使用
%Y
年，4 位
%y
年，2 位

3. ifnull(exp1,exp2)
3. 如果函数exp1，则赋值exp2

http://www.cnblogs.com/lmjob/archive/2008/03/18/1111978.html
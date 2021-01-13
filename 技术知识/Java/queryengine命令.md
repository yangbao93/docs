# queryengine命令
#### 进入queryengine服务

```
# 1.进入qe
queryengine路径/bin/queryengine --sessionconf engine=wing
# 2.退出qe
exit;
```

#### 执行sql语句

```
queryengine路径/bin/queryengine --sessionconf engine=wing -e "sql语句"
#执行sql并输出结果到文件
queryengine路径/bin/queryengine --sessionconf engine=wing -e "sql语句" > 文件路径
```

#### 执行sql文件

```
queryengine路径/bin/queryengine --sessionconf engine=wing -f some_sql_file.sql
#执行sql文件并输出结果到文件
queryengine路径/bin/queryengine --sessionconf engine=wing -f some_sql_file.sql > 文件路径
```

%23%23%23%23%20%E8%BF%9B%E5%85%A5queryengine%E6%9C%8D%E5%8A%A1%0A%60%60%60shell%0A%23%201.%E8%BF%9B%E5%85%A5qe%0Aqueryengine%E8%B7%AF%E5%BE%84%2Fbin%2Fqueryengine%20--sessionconf%20engine%3Dwing%0A%23%202.%E9%80%80%E5%87%BAqe%0Aexit%3B%0A%60%60%60%0A%23%23%23%23%20%E6%89%A7%E8%A1%8Csql%E8%AF%AD%E5%8F%A5%0A%60%60%60shell%0Aqueryengine%E8%B7%AF%E5%BE%84%2Fbin%2Fqueryengine%20--sessionconf%20engine%3Dwing%20-e%20%22sql%E8%AF%AD%E5%8F%A5%22%0A%23%E6%89%A7%E8%A1%8Csql%E5%B9%B6%E8%BE%93%E5%87%BA%E7%BB%93%E6%9E%9C%E5%88%B0%E6%96%87%E4%BB%B6%0Aqueryengine%E8%B7%AF%E5%BE%84%2Fbin%2Fqueryengine%20--sessionconf%20engine%3Dwing%20-e%20%22sql%E8%AF%AD%E5%8F%A5%22%20%3E%20%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%0A%0A%60%60%60%0A%0A%23%23%23%23%20%E6%89%A7%E8%A1%8Csql%E6%96%87%E4%BB%B6%0A%60%60%60shell%0Aqueryengine%E8%B7%AF%E5%BE%84%2Fbin%2Fqueryengine%20--sessionconf%20engine%3Dwing%20-f%20some_sql_file.sql%0A%23%E6%89%A7%E8%A1%8Csql%E6%96%87%E4%BB%B6%E5%B9%B6%E8%BE%93%E5%87%BA%E7%BB%93%E6%9E%9C%E5%88%B0%E6%96%87%E4%BB%B6%0Aqueryengine%E8%B7%AF%E5%BE%84%2Fbin%2Fqueryengine%20--sessionconf%20engine%3Dwing%20-f%20some_sql_file.sql%20%3E%20%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%0A%60%60%60
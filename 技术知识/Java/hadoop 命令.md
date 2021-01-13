# hadoop 命令
##### 1. 查看文件

```
hadoop fs -ls hadoop文件路径
```

##### 2. 文件大小

```
hadoop fs -du hadoop文件路径
```

##### 3. 下载文件

```
hadoop fs -get hadoop文件路径 本地文件路径
```

##### 4. 上传文件

```
hadoop fs -put 本地文件路径 hadoop文件路径
```

#### 5. 删除文件

```
hadoop fs -rm hadoop文件路径
```

%23%23%23%23%23%201.%20%E6%9F%A5%E7%9C%8B%E6%96%87%E4%BB%B6%0A%60%60%60shell%0Ahadoop%20fs%20-ls%20hadoop%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%0A%60%60%60%0A%23%23%23%23%23%202.%20%E6%96%87%E4%BB%B6%E5%A4%A7%E5%B0%8F%0A%60%60%60shell%0Ahadoop%20fs%20-du%20hadoop%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%0A%60%60%60%0A%23%23%23%23%23%203.%20%E4%B8%8B%E8%BD%BD%E6%96%87%E4%BB%B6%0A%60%60%60shell%0Ahadoop%20fs%20-get%20hadoop%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%20%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%0A%60%60%60%0A%23%23%23%23%23%204.%20%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6%0A%60%60%60shell%0Ahadoop%20fs%20-put%20%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%20hadoop%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%0A%60%60%60%0A%23%23%23%23%205.%20%E5%88%A0%E9%99%A4%E6%96%87%E4%BB%B6%0A%60%60%60shell%0Ahadoop%20fs%20-rm%20hadoop%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%0A%60%60%60
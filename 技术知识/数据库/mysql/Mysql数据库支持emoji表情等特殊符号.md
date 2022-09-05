## 数据库的格式
常见为utf-8

## 如果这个格式会有什么问题？
常见的字符都可以存入，但是如果用户输入了特殊字符，入emoji表情等，![[6620C53B2893C152701F6EEB7077E649.png]]就会出现问题，数据库存入时候回抛出异常

```shell
INSERT INTO emoj VALUES (
1366 Incorrect string value:'\xF0\x9F\x98\xB3'for column 'EMO]'at row 1
>时间：0s
```

**原因**
因为utf-8支持的是3字节的存储，但是如果需要存储4字节的内容就会出现错误。


## 解决办法

需要将utf-8的格式修改为utf-8mb4的格式（most byte 4），这样就可以解决emoji的存放问题。
这种格式提升了内容的丰富性，但会增加额外的存储空间
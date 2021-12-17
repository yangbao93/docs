MySQL在新版中支持了对JSON格式的支持，用此文章来记录如何在MySQL中使用JSON。一些常见的操作函数。

> 更多资料请参考官方文档 https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html

## 建立JSON格式的表

```sql
CREATE TABLE `t1` (
  `id` INT NOT NULL,
  `f1` VARCHAR(45) NULL,
  `f2` JSON NULL,
	PRIMARY KEY (`id`)
);
```

## 对于JSON字段的操作

### 1.普通的insert

```sql
INSERT INTO `t1` VALUES (1,'1','{"a":1, "b":"2"}');
```

### 2. 自带函数

#### JSON_OBJECT

```sql
-- 插入json object到数据库
INSERT INTO `t1` VALUES (1, '1', JSON_OBJECT("a", 1, "b", "2"));
```

#### JSON_ARRAY

```sql
-- 插入json array到数据库
INSERT INTO `t1` VALUES (2, '2', JSON_ARRAY("arr", 1, 2));
```

#### JSON_MERGE

```sql
-- 合并两个json 
INSERT INTO `t1` VALUES (
	3, '3',
  JSON_MERGE(
  	'{"a":3, "b":"3"}',
    '{"c":3, "d":"3"}',
  )
);

-- 方法2
INSERT INTO `t1` VALUES (
	4, '4',
  JSON_MERGE(
  	JSON_OBJECT("a", 4, "b", "4"),
    JSON_OBJECT("c", 4, "d", "4")
  )
);

-- 方法3
INSERT INTO `t1` VALUES (
	5, '5',
  JSON_MERGE(
  	'{"a":5, "b":"5"}',
    JSON_OBJECT("c", 5, "d", "5")
  )
);
```

#### JSON_EXTRACT

```sql
-- 用于获取json中的某个字段值
-- 获取f2中base底下的值
SELECT id,JSON_EXTRACT(f2, '$.base') FROM `t1` WHERE id = 1;
-- 获取f2中base底下namelist（jsonarray）的第一个值
SELECT id,JSON_EXTRACT(f2, '$.base.namelist[0]') FROM `t1` WHERE id = 1;
-- 简化形式
SELECT f2 FROM `t1` where f2->'$.base' = 2;
```

#### JSON_INSERT

向字段中增加一个key-value，如果这个key存在，则不做任何操作

```sql
-- 让id=1的f2字段，增加age字段
UPDATE `t1` SET f2 = JSON_INSERT(f2, '$.age', 10) WHERE id = 1;
```

#### JSON_REPLACE

替换内容，如果key不存在，则不做任何操作

```sql
-- 将id=1的f2字段中，age字段的值替换为9
UPDATE `t1` SET f2 = JSON_REPLACE(f2, '$.age', 9) WHERE id = 1;
```

#### JSON_SET

设置内容，如果key存在，则替换value值，如果key不存在，则增加key-value

```sql
-- 将id=1的f2字段中，age字段的值设置为8
UPDATE `t1` SET f2 = JSON_REPLACE(f2, '$.age', 8) WHERE id = 1;
```

#### JSON_REMOVE

删除内容

```sql
-- 将id=1的f2字段中，age字段移除
UPDATE `t1` SET f2 = JSON_REMOVE(f2, '$.age') WHERE id = 1;
```


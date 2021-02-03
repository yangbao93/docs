# Mysql特殊的联结方式

## 常见的联接方式

## 1. 左联

```text
select * from tablea left join tableb on tablea.id = tableb.id
```

左边基准表有则有

## 2. 右联

```text
select * from tablea right join tableb on tablea.id = tableb.id
```

右边基准表有则有

## 3. 内联

```text
select * from tablea inner join tableb on tablea.id = tableb.id
```

类似交集情况，均有则有

## 4. 特殊的联接方式

```text
SELECT    temp.id,    temp.name,    temp.type,    cl.tinyname FROM    ( SELECT id, name, type FROM tablea  WHERE id    = 1109 ) AS temp,    tableb AS cl WHERE    cl.pid = temp.id
```

此种方式类似于内联接，当一方为空时则查出来数据为空

%23%23%23%23%20%E5%B8%B8%E8%A7%81%E7%9A%84%E8%81%94%E6%8E%A5%E6%96%B9%E5%BC%8F%0A%23%23%23%23%201.%20%E5%B7%A6%E8%81%94%0A%60%60%60%0Aselect%20_%20from%20tablea%20%0Aleft%20join%20tableb%20on%20tablea.id%20%3D%20tableb.id%0A%60%60%60%0A%E5%B7%A6%E8%BE%B9%E5%9F%BA%E5%87%86%E8%A1%A8%E6%9C%89%E5%88%99%E6%9C%89%0A%23%23%23%23%202.%20%E5%8F%B3%E8%81%94%0A%60%60%60%0Aselect%20_%20from%20tablea%20%0Aright%20join%20tableb%20on%20tablea.id%20%3D%20tableb.id%0A%60%60%60%0A%E5%8F%B3%E8%BE%B9%E5%9F%BA%E5%87%86%E8%A1%A8%E6%9C%89%E5%88%99%E6%9C%89%0A%23%23%23%23%203.%20%E5%86%85%E8%81%94%0A%60%60%60%0Aselect%20\*%20from%20tablea%20%0Ainner%20join%20tableb%20on%20tablea.id%20%3D%20tableb.id%0A%60%60%60%0A%E7%B1%BB%E4%BC%BC%E4%BA%A4%E9%9B%86%E6%83%85%E5%86%B5%EF%BC%8C%E5%9D%87%E6%9C%89%E5%88%99%E6%9C%89%0A%0A%23%23%23%23%204.%20%E7%89%B9%E6%AE%8A%E7%9A%84%E8%81%94%E6%8E%A5%E6%96%B9%E5%BC%8F%0A%60%60%60%0ASELECT%0A%09temp.id%2C%0A%09temp.name%2C%0A%09temp.type%2C%0A%09cl.tinyname%20%0AFROM%0A%09\(%20SELECT%20id%2C%20name%2C%20type%20FROM%20tablea%20%20WHERE%20id%09%3D%201109%20\)%20AS%20temp%2C%0A%09tableb%20AS%20cl%20%0AWHERE%0A%09cl.pid%20%3D%20temp.id%0A%60%60%60%0A%E6%AD%A4%E7%A7%8D%E6%96%B9%E5%BC%8F%E7%B1%BB%E4%BC%BC%E4%BA%8E%E5%86%85%E8%81%94%E6%8E%A5%EF%BC%8C%E5%BD%93%E4%B8%80%E6%96%B9%E4%B8%BA%E7%A9%BA%E6%97%B6%E5%88%99%E6%9F%A5%E5%87%BA%E6%9D%A5%E6%95%B0%E6%8D%AE%E4%B8%BA%E7%A9%BA


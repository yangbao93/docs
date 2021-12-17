# MyBatis的foreach语句详解

## MyBatis的foreach语句详解

## MyBatis的foreach语句详解

**foreach** 的主要用在构建in条件中，它可以在SQL语句中进行迭代一个集合。foreach元素的属性主要有item，index，collection，open，separator，close。item表示集合中每一个元素进行迭代时的别名，index指定一个名字，用于表示在迭代过程中，每次迭代到的位置，open表示该语句以什么开始，separator表示在每次进行迭代之间以什么符号作为分隔符，close表示以什么结束，在使用foreach的时候最关键的也是最容易出错的就是collection属性，该属性是必须指定的，但是在不同情况下，该属性的值是不一样的，主要有一下3种情况：

* 如果传入的是单参数且参数类型是一个List的时候，collection属性值为list
* 如果传入的是单参数且参数类型是一个array数组的时候，collection的属性值为array
* 如果传入的参数是多个的时候，我们就需要把它们封装成一个Map了， **当然单参数也可以封装成map** ，实际上如果你在传入参数的时候，在 [breast](mybatis-de-foreach-yu-ju-xiang-jie.md) 里面也是会把它封装成一个Map的，map的key就是参数名，所以这个时候collection属性值就是传入的List或array对象在自己封装的map里面的key

  下面分别来看看上述三种情况的示例代码：

  1.单参数List的类型：

   select \*from t\_blog where id in ＃{item}

上述collection的值为list，对应的Mapper是这样的 public ListdynamicForeachTest\(Listids\); 测试代码： @Test public voiddynamicForeachTest\(\) { SqlSessionsession = Util.getSqlSessionFactory\(\).openSession\(\); BlogMapperblogMapper = session.getMapper\(BlogMapper.class\); List ids = newArrayList\(\); ids.add\(1\); ids.add\(3\); ids.add\(6\); List blogs =blogMapper.dynamicForeachTest\(ids\); for \(Blogblog : blogs\) System.out.println\(blog\); session.close\(\); }

2.单参数array数组的类型：

 select \*from t\_blog where id in ＃{item}

上述collection为array，对应的Mapper代码： public ListdynamicForeach2Test\(int\[\] ids\); 对应的测试代码： @Test public voiddynamicForeach2Test\(\) { SqlSessionsession = Util.getSqlSessionFactory\(\).openSession\(\); BlogMapperblogMapper = session.getMapper\(BlogMapper.class\); int\[\] ids =new int\[\] {1,3,6,9}; List blogs =blogMapper.dynamicForeach2Test\(ids\); for \(Blogblog : blogs\) System.out.println\(blog\); session.close\(\); }

3.自己把参数封装成Map的类型

 select \*from t\_blog where title like "%"＃{title}"%" and id in ＃{item}

上述collection的值为ids，是传入的参数Map的key，对应的Mapper代码： public ListdynamicForeach3Test\(Map params\); 对应测试代码：

```text
@Test
     public voiddynamicForeach3Test() {
        SqlSessionsession = Util.getSqlSessionFactory().openSession();
        BlogMapperblogMapper = session.getMapper(BlogMapper.class);
        finalList<Integer> ids = newArrayList<Integer>();
       ids.add(1);
       ids.add(2);
       ids.add(3);
       ids.add(6);
       ids.add(7);
       ids.add(9);
       Map<String, Object> params = newHashMap<String, Object>();
       params.put("ids", ids);
       params.put("title", "中国");
       List<Blog> blogs =blogMapper.dynamicForeach3Test(params);
        for (Blogblog : blogs)
          System.out.println(blog);
       session.close();
     }
```

在mybatis的mapper配置文件中，可以利用标签实现sql条件的循环，可完成类似批量的sql

mybatis接受的参数分为：（1）基本类型（2）对象（3）List（4）数组（5）Map 无论传哪种参数给mybatis，他都会将参数放在一个Map中： 如果传入基本类型：变量名作为key，变量值作为value 此时生成的map只有一个元素。 如果传入对象： 对象的属性名作为key，属性值作为value， 如果传入List： "list"作为key，这个List是value （这类参数可以迭代，利用标签实现循环） 如果传入数组： "array"作为key，数组作为value（同上） 如果传入Map： 键值不变。

标签的用法： 六个参数： collection：要循环的集合 index：循环索引（不知道啥用。。） item：集合中的一个元素（item和collection，按foreach循环理解） open：以什么开始 close：以什么结束 separator：循环内容之间以什么分隔 例如： 1. 2. UPDATE BMC\_SUBPLATE 3. SET PLSTATUS = '02' 4. WHERE 5. 6. PLID = ＃{plid} 7. 8. &lt;/update&gt;

collection的值其实就是mybatis把参数转化成Map以后，这个Map的key，但是这个key对应的value必须是一个集合， 可以是数组，也可以是List 生成的动态sql

 SELECT a.id, a.weight, b.price FROM p\_recycle\_product\_evaluate\_item a LEFT JOIN p\_recycle\_product b ON a.recycle\_product\_id = b.id WHERE a.id IN ＃{item}

[http://www.voidcn.com/article/p-vccfqsgj-bdp.html](http://www.voidcn.com/article/p-vccfqsgj-bdp.html)


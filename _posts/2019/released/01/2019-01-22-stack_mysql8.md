---
layout:     post
title:      技术栈2 - MySQL(8) - Schema与数据类型优化总结(优)
no-post-nav: true
category: stack
tags: [stack]
excerpt: 
---


### Schema与数据类型优化


####一、范式

**码是数据系统中的基本概念。所为码就是能唯一标识实体的属性。**

第一范式(1NF)：
MySQL
+	数据库表中的字段都是单一属性的，不可再分。这个单一属性由基本数据类型构成
   包括整数型、实数、字符型、逻辑型、日期型。
+ 在当前的任何DBMS中，傻瓜也不可能做出不符合第一范式的数据库。
+ 因为这些DBMS不允许你把数据库表的一列再分为二列或多列。

第二范式(2NF)：
2NF在1NF的基础上，消除了非主属性对于码的部分函数依赖。

```
系名 → 系主任
学号 → 系主任
（学号，课名） → 分数
```

```
对于（学号，课名） → 姓名，有 学号 → 姓名，存在非主属性 姓名 对码（学号，课名）的部分函数依赖。
对于（学号，课名） → 系名，有 学号 → 系名，存在非主属性 系名 对码（学号，课名）的部分函数依赖。
对于（学号，课名） → 系主任，有 学号 → 系主任，存在非主属性 对码（学号，课名）的部分函数依赖
```

第三范式(3NF)：
3NF在2NF的基础上，消除了非主属性对于码的传递函数依赖。也就是说，如果存在非主属性对于码的传递函数依赖，则不符合3NF的要求。

由此可见，符合3NF要求的数据库设计，基本上解决了数据冗余过大，插入异常、修改异常、删除异常的问题。当然，在实际中，往往为了性能上或者应对扩展的需要，经常做到2NF或者1NF。


[解释一下关系数据库的第一第二第三范式](http://www.zhihu.com/question/24696366)

####二、通常情况下选择正确存储数据的最小类型数据

查看表空间的命令
show table status like 't1' \G

以下两个属性
 ** Data_length **
 ** Data_free **
表示表空间的大小

查看表空间的命令

```
SELECT CONCAT(ROUND(SUM(data_length)/(1024*1024*1024), 2), ' GB') AS 'Total Data Size'
FROM information_schema.TABLES WHERE table_schema LIKE 'database';
```

[MySQL表空间及索引的查看](http://www.cnblogs.com/jiaxiaoai/archive/2011/07/18/2109298.html)

####三、手机号应该用什么类型的存储
int 的最大值为21亿， 2**31，所以并不能保存手机号的百亿位数据。


```
mysql> create table big_int_test ( int_01 int(11), b_01 bigint(11), v_01 varchar(11));
Query OK, 0 rows affected (0.03 sec)

mysql> show create table big_int_test;
+--------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table        | Create Table                                                                                                                                                            |
+--------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| big_int_test | CREATE TABLE `big_int_test` (
  `int_01` int(11) DEFAULT NULL,
  `b_01` bigint(11) DEFAULT NULL,
  `v_01` varchar(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
+--------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> insert into big_int_test (int_01, b_01, v_01) value (13522221111, 13522221111,13522221111);
ERROR 1264 (22003): Out of range value for column 'int_01' at row 1
mysql> insert into big_int_test (b_01, v_01) value (13522221111,13522221111);
Query OK, 1 row affected (0.00 sec)

mysql>

```

####四、尽量避免使用NULL

+ null 是一个未知的值

+ null 和任何值都不相等，包括它自己

```
mysql> select null >0, null < 0 from dual;
+---------+----------+
| null >0 | null < 0 |
+---------+----------+
|    NULL |     NULL |
+---------+----------+
1 row in set (0.01 sec)

mysql> select null = null from dual;
+-------------+
| null = null |
+-------------+
|        NULL |
+-------------+
1 row in set (0.07 sec)

```

+ 将某个值与Null进行比较的正确方法是使用is关键字，以及is not 操作符

```

mysql> select * from areas where id in (3144, 3143, null, 3133);
+------+--------+--------------------------------+--------+
| id   | areaid | name                           | cityid |
+------+--------+--------------------------------+--------+
| 3133 | 654226 | 和布克赛尔蒙古自治县           | 654200 |
| 3143 | 659003 | 图木舒克市                     | 659000 |
| 3144 | 659004 | 五家渠市                       | 659000 |
+------+--------+--------------------------------+--------+
3 rows in set (0.02 sec)

select * from areas where id = 3144 or id = 3143 or id =3133 or id = null;

mysql> select * from areas where id not in (3144, 3143, null, 3133);
Empty set (0.00 sec)

转换成  select * from areas where id != 3144 and id != 3143 and id !=3133 and id != null;


```

+ Null 与排序

	**在排序时，null值被认为是最大的，在降序排序时会让你非常头大，因为null值排在了最前面。解决方法是使用 coalesce 进行**


```
– 在输出时将 null 转换为 0 :

select name, coalesce(points, 0)

from users

order by 2 desc;

– 输出时保留 null, 但排序时转换为 0 :

select name, points

from users

order by coalesce(points, 0) desc;
```




http://www.codeceo.com/article/sql-null.html



####五、DATETIME 和 TIMESTAMP 的区别

DATETIME：

+ 占用 8 个字节
+ 这个类型能保存大范围的值，从 1001 年到 9999 年，精度为秒。
+ 封存到 'YYYYMMDDHHMMSS' 的整数中，与时区无关。

TIMESTAMP:

+ 占用4个字节
+ 保存了1970.1.1 午夜到现在的秒数，它和UNIX时间戳相同。
+ 只能表示从 1970 年到  2038 年。
+ FROM_UNIXTIME()  UNIX_TIMESTAMP() 函数把时间戳和日期相互转换。
+ 存储时对当前的时区进行转换，检索时在转换回当前的时区。
+ 如果插入时没有指定第一个TIMESTAMP 列的值，MySQL则设置这个列的值为当前时间。在插入一行记录时，MySQL默认也会更新第一个TIMESTAMP的列（除非在UPDATE语句中明确指定的值）。
+ TIMESTAMP列的默认值为NOT NULL 这和其他数据类型不一样。


####六、数据库中价格应该怎么存的，为什么不用浮点数，如何证明浮点数不精确

```
mysql> CREATE TABLE test (c1 float(10,2),c2 decimal(10,2));
Query OK, 0 rows affected (0.29 sec)

mysql> insert into test values(131072.32,131072.32);
Query OK, 1 row affected (0.07 sec)

mysql> select * from test;
+-----------+-----------+
| c1        | c2        |
+-----------+-----------+
| 131072.31 | 131072.32 |
+-----------+-----------+
1 row in set (0.00 sec)

从上面的例子中我们看到c1列的值由131072.32变成了131072.31，这就是浮点数的不精确性造成的。

在MySQL中float、double（或real）是浮点数，decimal（或numberic）是定点数。
```
```

>> (10015.8*100.0).to_i
=> 1001579
>> 10015.8*100.0
=> 1001579.9999999999
>>  1001580.0.to_i
=> 1001580

```


今后关于浮点数和定点数的应用中，大家要记住以下几点：

+ 浮点数存在误差问题。

+ 对货币等对精度敏感的数据，应该用定点数表示或存储；也可以用BIGINT 代替DECIMAL，将需要存储的货币单位根据最小的位数乘以相应的倍数即可。假设要存储财务数据精确到万分之一分，则可以把所有金额乘以一百万。

+ 编程中，如果用到浮点数，要特别注意误差问题，并尽量避免做浮点数比较。

+ 要注意浮点数中一些特殊值的处理。




[为MySQL选择合适的数据类型](http://c.biancheng.net/cpp/html/1466.html)
[浮点数结构详解](http://wenku.baidu.com/view/11d8f46527d3240c8447efa8.html)
[浮点数的二进制表示](http://www.ruanyifeng.com/blog/2010/06/ieee_floating-point_representation.html)
[浮点数的二进制表示学习笔记  ](http://blog.163.com/yql_bl/blog/static/847851692008112013117685/)



####七、int 后面的数值和 varchar 后面的数值的区别

+ MySQL 可以为整数类型指定宽度，例如INT（11）， 对大多数应用来说是没有意义的。它不会限制值的合法范围，只是规定了MySQL 的一些交互工具，用来显示字符的个数。对于存储和计算来说，INT(1) 和 INT(20) 是相同的

+ 但是对于varchar 类型来说，其后面显示的数值，表示其可以插入多少个字符串。

```
mysql> create table int_test ( int_01 int(1), int_02 int(20));
Query OK, 0 rows affected (0.14 sec)

mysql> insert into int_test (int_01, int_02) value ( 123456, 123456);
Query OK, 1 row affected (0.04 sec)

mysql> select * from int_test;
+--------+--------+
| int_01 | int_02 |
+--------+--------+
| 123456 | 123456 |
+--------+--------+
1 row in set (0.02 sec)
```


```
mysql> create table varchar_test ( varchar_01 varchar(5), varchar_02 varchar(10));
Query OK, 0 rows affected (0.04 sec)

mysql> insert into varchar_test (varchar_01, varchar_02) value ("11111", "1111111");
Query OK, 1 row affected (0.01 sec)

mysql> insert into varchar_test (varchar_01, varchar_02) value ("111112", "1111111");
ERROR 1406 (22001): Data too long for column 'varchar_01' at row 1
mysql>
```



####八、varchar 和 char 的区别

VARCHAR：

  + VARCHAR类型用于存储可变长字符串，是最常见的字符串数据类型。
      它比定长类型更节省空间，因为它仅使用必要的空间
  + VARCHAR需要使用1-2个额外的字节记录字符串的长度。
      如果列的最大长度小于或等于255个字节，则只使用1个字节来表示长度，否则使用2个字节。
      所以 VARCHAR(1000) 的列则需要1002个字节。
  + 在5.0或更好版本，MySQL在存储和检索时会保留末尾空格。
  + InnoDB则更灵活，它可以把过长的VARCHAR存储为BLOB

CHAR:

  + CHAR 类型是定长的，MySQL总是根据定义的字符串长度分配足够的空间。
  + CHAR适合存储很短的字符串，或者所有值都很接近的一个长度。例如
      CHAR就非常合适存储密码的MD5值，因为这个是定长的。
  + 对于经常变更的数据，CHAR也比VARCHAR
      更好，因为定长的CHAR类型不容易产生碎片
  + 对于非常短的列，CHAR比VARCHAR在存储空间上也更有效率。例如用
      CHAR(1)
      来存储只有Y和N的只，如果采用单字节字符集只需要一个字节，但是
      VARCHAR(1) 却需要两个字节，因为还有一个用来记录长度。

```
CREATE TABLE char_test(
    char_col1 CHAR(1),
    char_col2 CHAR(10),
    varchar_col1 CHAR(1),
    varchar_col2 CHAR(10)) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO char_test VALUES ('Y', 'string1', "Y", "string1"), ('Y', ' string1', "Y", " string1"), ('Y', 'string1 ', "Y", "string1 ");


SELECT LENGTH(char_col1), LENGTH(varchar_col1), CONCAT("'", char_col2, "'") FROM char_test;
SELECT LENGTH(char_col1), LENGTH(varchar_col1), CONCAT("'", varchar_col2, "'") FROM char_test;
```


####九、varchar 最大的存储空间

字符是指计算机中使用的字母、数字、汉字和符号。

字节是计算机信息技术中存储容量的一种计量单位，也表示一些计算机编程语言中的数据类型和语言字符。


UTF-8 一个汉字=3个字节，英文是一个字节
GBK   一个汉字=2个字节，英文是一个字节

**由于MySQL的记录行长度是有限制的，不是无限长的，这个长度是64K，即65535个字节，对所有的表都是一样的。**

在UTF-8状态下，汉字最多可以存 21844个字符串, 英文也为 21844个字符串。
在GBK状态下，汉字最多可以存 32766个字符串，英文也为 32766个字符串。


varchar(n) 其中n表示字符，无论汉字和英文，MySQL都能存入n个字符，但实际存入其中的字节长度有所区别。


+ MySQL对于变长类型的字段会有1-2个字节来保存字符长度。
+ 当字符数小于等于255时，MySQL只用1个字节来记录，因为2的8次方减1只能存到255。
  当字符数多余255时，就得用2个字节来存长度了。
+ 在UTF-8状态下的varchar，最大只能到 (65535 - 2) / 3 = 21844 余 1。
+ 在GBK状态下的varchar, 最大只能到 (65535 - 2) / 2 = 32766 余 1。


[MySQL 数据库 varchar 到底可以存多少个汉字，多少个英文呢?我们来搞搞清楚](https://ruby-china.org/topics/24920)


####十、数据库碎片是什么东西，有什么影响

MySQL具有相当多不同种类的存储引擎来实现列表中的数据存储功能。**每当MySQL从你的
列表中删除了一行内容，该段空间就会被留空。而在一段时间内的大量删除操作，
会使这种留空的空间变得比存储列表内容所使用的空间更大。**
当MySQL对数据进行扫描时，它扫描的对象实际是列表的容量需要求的上限
也就是数据被写入的区域中处于峰值位置的部分。如果进行新的插入操作，
MySQL将尝试利用这些留空的区域，但仍然无法将其彻底占用。

这就是为什么不建议删除数据库的数据。但是你可以使用
一个状态来描述，描述该状态是否可用

**data_free 表示我们删除后所产生的留空空间。**

[浅析MySQL数据碎片的产生](http://database.51cto.com/art/201103/251967.htm)

查看线上数据库中Table
Infomation时发现有一个日志表数据大小和索引大小有915M，但实际上行数只有92行。
该表需要频繁插入并且会定时去删掉旧的记录。该表上建立了索引，所以应该是产生了大量的碎片
使用Optimize table
表明，优化后大小变为2.19M，少了很多，同时可以看出该表上的索引建的多余，因为插入操作比查询操作要多
很多，而且查询不多，查询的数量也变小。

[mysql-suipian-youhua](http://pengbotao.cn/mysql-suipian-youhua.html)


####十一、什么情况下会使用binary 查询


**BINARY 不是函数，是类型转换运算符，它用来强制它后面的字符串为一个二进制字符串，可以理解为在字符串比较的时候区分大小写**

**因为有的MySQL特别是4.x以前以前的对于字符串检索不准确，所以在检索的时候加上
binary**

**MySQL 中 字符串和字符串的比较是不靠谱的，在二进制层次上比较靠谱**

> It an efficient have of comparing byte to byte instead of character to character


```
create table user_test(
  id int(9) unsigned NOT NULL auto_increment,
  username varchar(30) NOT NULL default '',
  primary key (id)
);


INSERT INTO user_test (username) VALUES('美文');
INSERT INTO user_test (username) VALUES('美国项目');
INSERT INTO user_test (username) VALUES('李文');
INSERT INTO user_test (username) VALUES('老唐');
INSERT INTO user_test (username) VALUES('梦漂');
INSERT INTO user_test (username) VALUES('龙武');
INSERT INTO user_test (username) VALUES('夏');


mysql> select * from user_test;
+----+--------------+
| id | username     |
+----+--------------+
|  1 | 美文         |
|  2 | 美国项目     |
|  3 | 李文         |
|  4 | 老唐         |
|  5 | 梦漂         |
|  6 | 龙武         |
|  7 | 夏           |
+----+--------------+
7 rows in set (0.00 sec)


mysql> select * from user_test where username =  '夏';
+----+----------+
| id | username |
+----+----------+
|  7 | 夏       |
+----+----------+
1 row in set (0.00 sec)

mysql> select * from user_test where username =  '夏 ';
+----+----------+
| id | username |
+----+----------+
|  7 | 夏       |
+----+----------+
1 row in set (0.00 sec)

mysql> select * from user_test where username = BINARY '夏';
+----+----------+
| id | username |
+----+----------+
|  7 | 夏       |
+----+----------+
1 row in set (0.00 sec)

mysql> select * from user_test where username = BINARY '夏 ';
Empty set (0.00 sec)


```


[MySQL的binary解决MySQL数据大小写敏感问题的方法](http://www.jb51.net/article/41546.htm)
[Why the 'BINARY' in a SELECT statement?](http://stackoverflow.com/questions/11133257/why-the-binary-in-a-select-statement)
[charset-binary-op](http://dev.MySQL.com/doc/refman/5.0/en/charset-binary-op.html)



####十二、VARCHAR（5） 和 VARCHAR（200）存储上的区别，排序上的区别

+ 相比较而言，varchar(5) 所占用的空间相对比较小。
+ 在排序时，varchar(5) 所占用的空间相对比 varchar(200) 要少。

####十三、1000w的数据，如果在ORDER BY 中使用到varchar(1000)这个列，并且查询扫描整个表，为了排序就需要超过30GB的临时表

最坏情况下的长度分配对于排序也是一样的。
意思就是在排序的时候使用的是分配长度的最大长度。

```
1 GB = 1000 MB = 1 000 000 KB = 1 000 000 000 B
1 MB = 1 000KB = 1 000 000 B

3000 * 10 000 000 = 30 000 000 000 B = 30GB
```

####十四、排序 使用 FIND_IN_SET 进行按顺序排序

```
  mysql>  explain
      ->  select id, title
      ->  from deals t
      ->  where t.id in (1014694, 1014693, 1014697, 1014691)
      ->  order by FIND_IN_SET(t.id, '1014694,1014693,1014697,1014691');
  +----+-------------+-------+-------+---------------+---------+---------+------+------+-----------------------------+
  | id | select_type | table | type  | possible_keys | key     | key_len | ref  | rows | Extra                       |
  +----+-------------+-------+-------+---------------+---------+---------+------+------+-----------------------------+
  |  1 | SIMPLE      | t     | range | PRIMARY       | PRIMARY | 4       | NULL |    4 | Using where; Using filesort |
  +----+-------------+-------+-------+---------------+---------+---------+------+------+-----------------------------+
  1 row in set (0.01 sec)

  4、
  mysql>  explain
      ->  select id, title
      ->  from deals t
      ->  where t.id in (1014694, 1014693, 1014697, 1014691);
  +----+-------------+-------+-------+---------------+---------+---------+------+------+-------------+
  | id | select_type | table | type  | possible_keys | key     | key_len | ref  | rows | Extra       |
  +----+-------------+-------+-------+---------------+---------+---------+------+------+-------------+
  |  1 | SIMPLE      | t     | range | PRIMARY       | PRIMARY | 4       | NULL |    4 | Using where |
  +----+-------------+-------+-------+---------------+---------+---------+------+------+-------------+
  1 row in set (0.01 sec)

```

####十五、通用的设计实践，在“查找表”时采用整数主键而避免采用基于字符串的值进行关联

  由于根据字符串的关联，在查找排序的时候，需要占用更多的空间，消耗更多的资源。
  [theory-of-mysql-index](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)

####十六、选择标识符

  + 标识列选择数据类型时，应该选择跟关联表中的对应列一样的类型。

  + 混用不同数据类型可能导致性能问题，及时没有性能影响，在比较操作时饮食类型转换也可以能导致很难发现的错误。
    对于完全“随机”的字符串也需要多加注意，

  + 例如MD5（），SHA1（）或UUID()产生的字符串，

  这些函数生成的新值会任意分布在很大的空间内，这会导致INSERT以及一些SELECT语句变得很慢
  使用UUID()并非一无是处，在分布式数据库中，使用UUID()是一个不错的选择。

  [MySQL 用 UUID 作为主键，实际使用中有什么问题](https://ruby-china.org/topics/24509)

下面例子中关联查询的时候，使用一个字符串和int型主键ID进行串联。会导致索引使用不上。

```
mysql> EXPLAIN SELECT *
    -> FROM tb_shops AS t
    -> INNER JOIN candidate_deals AS c_d
    -> WHERE t.id = c_d.cid \G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: t
         type: ALL
possible_keys: PRIMARY
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 1
        Extra: NULL
*************************** 2. row ***************************
           id: 1
  select_type: SIMPLE
        table: c_d
         type: ALL
possible_keys: idx_cid
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 1
        Extra: Range checked for each record (index map: 0x2)
2 rows in set (0.07 sec)
```

  [MySQL 为什么需要一个主键](https://ruby-china.org/topics/26352)


####十七、特殊类型数据

MySQL 提供INET_ATON()和INET_NEOA()函数在IP地址上进行转换

```
  mysql> SELECT INET_ATON('209.207.224.40'), INET_NTOA(3520061480) FROM dual;
  +-----------------------------+-----------------------+
  | INET_ATON('209.207.224.40') | INET_NTOA(3520061480) |
  +-----------------------------+-----------------------+
  |                  3520061480 | 209.207.224.40        |
  +-----------------------------+-----------------------+
  1 row in set (0.02 sec)
```

####十八、JOIN 驱动表

+ inner join驱动顺序由优化器自己制定，如果优化器选择有误可以使用straight_join 自己指定驱动顺序以达到优化的目的。

+ left join驱动顺序是固定的，left join左边的表为驱动表，右边为匹配表，right join则刚好相反。

+ 存在group by或者order by 子句的关联查询中，如果引用的字段是驱动表的字段那么分组或者排序是可以使用到索引的。但是如果引用的是其他匹配表的字段，那么分组或者排序动作则无法使用索引。

+ 内连接就是检索出与连接条件完全匹配的数据行；而外连接则保留了所有驱动表的数据，匹配表中无法匹配的数据则以null输出。

+ 驱动表的结果集，越小越好。

** 有一点需要注意的是：我们前面说要尽量缩小驱动表的结果集，
  但是这里却选择了一个超大表来做为驱动表，
  但其实两者是在一定程度上是不相悖的。
  因为缩小驱动表结果集实际上是为了减少nested loop的次数，
  而我们修改了驱动表以后，排序动作直接在索引完成，
  优化器直接按照logintime desc的顺序进行扫描，
  只要返回记录数等于100时就可以完成查询操作，
  实际的nested loop次数是很少的。而未修改前的SQL之所以需要执行那么久是因为它的排序无法使用索引，
  所以必须先等待临时结果集生成，而且是全量生成，也就是说需要在约3000万行的表里进行10万次查询，
  然后把结果聚集到一张临时表再进行排序选出100行记录。**


[join-query-in-mysql](http://hidba.ga/2014/09/26/join-query-in-mysql/)

####十九、从父表冗余一些数据到子表的理由是排序的需要。

工单87334

deals 表和 tb_shops 表的关联查询，但是最后使用了 tb_shops.grade 做desc


```
EXPLAIN SELECT  `deals`.*
FROM `deals`
  INNER JOIN `tb_shops`
  ON `tb_shops`.`id` = `deals`.`tb_shop_id`
    LEFT JOIN candidate_deals
    on deals.candidate_deal_id = candidate_deals.id
      LEFT JOIN deal_infos
      on deal_infos.deal_id = deals.id
WHERE ( deals.bg_tag_id > 0 )
  AND (deals.complete_status = 1)
ORDER BY tb_shops.grade DESC, deals.id DESC LIMIT 20 OFFSET 0

*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: deals
         type: range
possible_keys: index_deals_on_tb_shop_id_and_id,idx_bg_tag_pub_beg,idx_bg_tag_pub_end
          key: idx_bg_tag_pub_beg
      key_len: 4
          ref: NULL
         rows: 1
        Extra: Using index condition; Using where; Using temporary; Using filesort  <- 坏味道，temporary && filesort

```

将`tb_shops`表上的 `grade` 赋值到 `deals` 上，在代码中将`tb_shops` 表 `grade` 逻辑也同步到`deals`上
然后加索引。这样排序，会快一些。

```
ALTER TABLE deals ADD COLUMN grade int(11) not null;
ALTER TABLE deals ADD INDEX `idx_of_grade` (grade, id);
```

```
EXPLAIN SELECT  `deals`.*
FROM `deals`
  INNER JOIN `tb_shops`
  ON `tb_shops`.`id` = `deals`.`tb_shop_id`
    LEFT JOIN candidate_deals
    on deals.candidate_deal_id = candidate_deals.id
      LEFT JOIN deal_infos
      on deal_infos.deal_id = deals.id
WHERE ( deals.bg_tag_id > 0 )
  AND (deals.complete_status = 1)
ORDER BY deals.grade DESC, deals.id DESC LIMIT 20 OFFSET 0

*************************** 2. row ***************************
           id: 1
  select_type: SIMPLE
        table: deals
         type: ref
possible_keys: index_deals_on_tb_shop_id_and_id,idx_bg_tag_pub_beg,idx_bg_tag_pub_end
          key: index_deals_on_tb_shop_id_and_id
      key_len: 4
          ref: tao800_test9.tb_shops.id
         rows: 1
        Extra: Using where
```

####二十、MySQL 视图

+ 什么是视图？视图是由查询结果形成的一张虚拟表。
+ 什么时候要用到视图？如果某个查询结果出现的非常频繁，也就是说，要经常拿到这个查询结果来做子查询。
+ 视图的创建语法：

	```
		create view 视图名 as select 语句；
	```
+ 使用视图有什么好处？
	**	1、简化查询语句。将经常用到的子查询做成视图。**
	**	2、可以进行权限控制。	视图里只开放部分数据列。**
	**	3、大数据分表时可以用到。**


[MySQL视图学习总结](http://www.cnblogs.com/wangtao_20/archive/2011/02/24/1964276.html)
[MySQL之视图](http://www.cnblogs.com/zzwlovegfj/archive/2012/06/23/2559596.html)

####二十一、更快地读，更慢的写。高性能数据库的设计

为了提升查询的速度，经常会需要见一些额外的索引，增加冗余列，甚至是创建缓存表和汇总表。这些方法会增加写查询的负担，也需要额外的维护任务，但是在设计高性能数据库时，这些都是最常见的技巧。虽然写操作变得更慢了，但更显著地提高了读操作的性能

####二十二、加快ALTER TBALE 操作的速度

**ALTER TABLE操作需要花费数小时时间甚至数天才能完成。**
**大部分ALTER TABLE操作将导致MySQL服务中断。**
**不是所有的ALTER TABLE操作都会引起表重建。**

```
ALTER TABLE sakila.film MODIFY COLUMN rental_duration TINYINT(3) NOT NULL DEFAULT 5;

所有的MODIFY COLUMN 操作都将导致表重建。所以这种操作是很慢的。

```

```
ALTER TABLE sakila.film ALTER COLUMN rental_duration TINYINT(3) NOT NULL DEFAULT 5;

这个语句会直接修改.frm 文件而不设计表数据。所以，这个操作是非常快的。

```

ALTER TABLE 允许使用 ALTER COLUMN、MODIFY COLUMN和CHANGE COLUMN语句修改列，这三种操作都是不一样的。



####二十三、给大表添加索引或字段该如何操作

**在构建索引的工作被延迟到数据完全载入以后，这个时候已经可以通过排序来构建索引了。这样做会快很多，并且使得索引树的碎片更少、更紧凑。**

1、clone 新表

2、先删除所有的非唯一性索引，然后增加新的列，最后重新创建删除掉的索引。

3、交换新表和旧表

本笔记为阿里云天池龙珠计划SQL训练营的学习内容，链接为：https://tianchi.aliyun.com/specials/promotion/aicampsql；

# 知识点总结

## 视图

### 创建视图

    CREATE VIEW <视图名称>(<列名1>,<列名2>,...) AS <SELECT语句>
    
注意：
- 视图名在数据库中需要是唯一的，不能与其他视图和表重名。
- 视图不仅可以基于真实表，我们也可以在视图的基础上继续创建视图。虽然在视图上继续创建视图的语法没有错误，但是我们还是应该尽量避免这种操作。这是因为对多数 DBMS 来说， 多重视图会降低 SQL 的性能。
- 在一般的DBMS中定义视图时不能使用ORDER BY语句。因为视图和表一样，**数据行都是没有顺序的**。
- 在 MySQL中视图的定义是允许使用 ORDER BY 语句的，但是若从特定视图进行选择，而该视图使用了自己的 ORDER BY 语句，则视图定义中的 ORDER BY 将被忽略。

### 修改视图

    ALTER VIEW <视图名> AS <SELECT语句>

### 更新视图

视图是一个虚拟表，所以对视图的操作就是对底层基础表的操作，所以在修改时只有满足底层基本表的定义才能成功修改。

对于一个视图来说，如果包含以下结构的任意一种都是不可以被更新的：

- 聚合函数 SUM()、MIN()、MAX()、COUNT() 等。
- DISTINCT 关键字。
- GROUP BY 子句。
- HAVING 子句。
- UNION 或 UNION ALL 运算符。
- FROM 子句中包含多个表。

视图只是原表的一个窗口，所以它修改也只能修改透过窗口能看到的内容。

虽然可以通过视图对表进行修改，但是并不推荐这种使用方式。而且我们在创建视图时也尽量使用限制不允许通过视图来修改表。

### 删除视图

    DROP VIEW <视图名1> [ , <视图名2> …]

## 子查询

子查询指一个查询语句嵌套在另一个查询语句内部的查询，在 SELECT 子句中先计算子查询，子查询结果作为外层另一个查询的过滤条件，查询可以基于一个表或者多个表。

子查询就是将用来定义视图的 SELECT 语句直接用于 FROM 子句当中。而且由于子查询是一次性的，所以子查询不会像视图那样保存在存储介质中， 而是在 SELECT 语句执行之后就消失了。

### 嵌套子查询

与在视图上再定义视图类似，子查询也没有具体的限制，**虽然嵌套子查询可以查询出结果，但是随着子查询嵌套的层数的叠加，SQL语句不仅会难以理解而且执行效率也会很差，所以要尽量避免这样的使用**。

### 标量子查询

标量就是单一的意思，那么标量子查询也就是单一的子查询，所谓单一就是要求我们执行的SQL语句只能返回一个值，也就是要返回表中具体的**某一行的某一列**。

查询出销售单价高于平均销售单价的商品：

    SELECT product_id, product_name, sale_price
      FROM product
     WHERE sale_price > (SELECT AVG(sale_price) FROM product);

### 关联子查询

关联子查询就是通过一些标志将内外两层的查询连接起来起到过滤数据的目的。

选取出各商品种类中高于该商品种类的平均销售单价的商品。SQL语句如下：

    SELECT product_type, product_name, sale_price
      FROM product ASp1
     WHERE sale_price > (SELECT AVG(sale_price)
       FROM product ASp2
                          WHERE p1.product_type =p2.product_type
       GROUP BY product_type);

关联查询的执行过程简要的概括为：

- 首先执行不带WHERE的主查询。
- 根据主查询讯结果匹配product_type，获取子查询结果。
- 将子查询结果再与主查询结合执行完整的SQL语句。

## 函数

### 算数函数

- ABS：绝对值。当 ABS 函数的参数为NULL时，返回值也是NULL。
- MOD：求余数。`MOD( 被除数，除数 )`。注意：主流的 DBMS 都支持 MOD 函数，只有SQL Server 不支持该函数，其使用%符号来计算余数。
- ROUND：四舍五入。`语法：ROUND( 对象数值，保留小数的位数 )`

### 字符串函数

- CONCAT：拼接。`CONCAT(str1, str2, str3)`
- LENGTH：字符串长度。`LENGTH( 字符串 )`
- LOWER：小写转换。UPPER 函数用于大写转换。
- REPLACE：字符串的替换。`REPLACE( 对象字符串，替换前的字符串，替换后的字符串 )`
- SUBSTRING：字符串的截取。`SUBSTRING （对象字符串 FROM 截取的起始位置 FOR 截取的字符数）`。截取的起始位置从字符串最左侧开始计算，索引值起始为1。
- SUBSTRING_INDEX：字符串按索引截取。`SUBSTRING_INDEX (原始字符串， 分隔符，n)`。该函数用来获取原始字符串按照分隔符分割后，第 n 个分隔符之前（或之后）的子字符串，支持正向和反向索引，索引起始值分别为 1 和 -1。

### 日期函数

- CURRENT_DATE – 获取当前日期
- CURRENT_TIME – 当前时间
- CURRENT_TIMESTAMP – 当前日期和时间
- EXTRACT – 截取日期元素。`EXTRACT(日期元素 FROM 日期)`

### 转换函数

- CAST – 类型转换。`CAST（转换前的值 AS 想要转换的数据类型）`
- COALESCE – 将NULL转换为其他值。`COALESCE(数据1，数据2，数据3……)`。COALESCE 是 SQL 特有的函数。该函数会返回可变参数 A 中左侧开始**第 1个不是NULL的值**。参数个数是可变的。

## 谓词

### LIKE谓词

- 前方一致：
    
      SELECT * FROM samplelike WHERE strcol LIKE 'ddd%';

- 中间一致：

      SELECT * FROM samplelike WHERE strcol LIKE '%ddd%';
      
- 后方一致：

      SELECT * FROM samplelike WHERE strcol LIKE '%ddd';
      
中间一致同时包含前方一致和后方一致的查询结果。

- 下划线匹配任意 1 个字符

### BETWEEN谓词

BETWEEN 的特点就是结果中会包含两个临界值，也就是闭区间。如果不想让结果中包含临界值，那就必须使用 < 和 >。

### IS NULL、 IS NOT NULL 

### IN谓词

    SELECT product_name, purchase_price FROM product WHERE purchase_price IN (320, 500, 5000);

### EXIST 谓词

EXIST 可以用来替换 IN ， NOT IN 可以用NOT EXIST来替换。

## CASE 表达式

CASE表达式的语法分为简单CASE表达式和搜索CASE表达式两种。

搜索CASE表达式语法：

    CASE WHEN <求值表达式> THEN <表达式>
         WHEN <求值表达式> THEN <表达式>
         WHEN <求值表达式> THEN <表达式>
         .
         .
         .
    ELSE <表达式>
    END  

ELSE 子句也可以省略不写，这时会被默认为 ELSE NULL。但为了防止有人漏读，还是希望大家能够显示地写出 ELSE 子句。

# 练习题

## 1 

创建出满足下述三个条件的视图（视图名称为 ViewPractice5_1）。使用 product（商品）表作为参照表，假设表中包含初始状态的 8 行数据。

- 条件 1：销售单价大于等于 1000 日元。
- 条件 2：登记日期是 2009 年 9 月 20 日。
- 条件 3：包含商品名称、销售单价和登记日期三列。

对该视图执行 SELECT 语句的结果如下所示。

    SELECT * FROM ViewPractice5_1;

执行结果

|product_name | sale_price | regist_date|
|-------------|------------|------------|
|T恤衫        |    1000    | 2009-09-20 |
|菜刀         |    3000    | 2009-09-20 |

    CREATE VIEW ViewPractice5_1 (product_name, sale_price, regist_date) AS 
    SELECT product_name, sale_price, regist_date FROM product WHERE sale_price>= 1000 AND regist_date == '2009-09-20';

## 2 

向习题一中创建的视图 ViewPractice5_1 中插入如下数据，会得到什么样的结果呢？

    INSERT INTO ViewPractice5_1 VALUES (' 刀子 ', 300, '2009-11-02');

不满足原表约束条件，报错。

## 3 

请根据如下结果编写 SELECT 语句，其中 sale_price_all 列为全部商品的平均销售单价。

|product_id | product_name | product_type | sale_price | sale_price_all       |
|-----------|--------------|--------------|------------|----------------------|
|0001       | T恤衫        | 衣服         | 1000       | 2097.5000000000000000|
|0002       | 打孔器       | 办公用品      | 500        | 2097.5000000000000000|
|0003       | 运动T恤      | 衣服          | 4000      | 2097.5000000000000000|
|0004       | 菜刀         | 厨房用具      | 3000       | 2097.5000000000000000|
|0005       | 高压锅       | 厨房用具      | 6800       | 2097.5000000000000000|
|0006       | 叉子         | 厨房用具      | 500        | 2097.5000000000000000|
|0007       | 擦菜板       | 厨房用具       | 880       | 2097.5000000000000000|
|0008       | 圆珠笔       | 办公用品       | 100       | 2097.5000000000000000|

    SELECT product_id, product_name, product_type, sale_price, AVG(sale_price) AS sale_price_all FROM product

## 4 

请根据习题一中的条件编写一条 SQL 语句，创建一幅包含如下数据的视图（名称为AvgPriceByType）。

|product_id | product_name | product_type | sale_price | avg_sale_price      |
|-----------|--------------|--------------|------------|---------------------|
|0001       | T恤衫        | 衣服         | 1000       |2500.0000000000000000|
|0002       | 打孔器       | 办公用品     | 500        | 300.0000000000000000|
|0003       | 运动T恤      | 衣服         | 4000       |2500.0000000000000000|
|0004       | 菜刀         | 厨房用具     | 3000       |2795.0000000000000000|
|0005       | 高压锅       | 厨房用具     | 6800       |2795.0000000000000000|
|0006       | 叉子         | 厨房用具     | 500        |2795.0000000000000000|
|0007       | 擦菜板       | 厨房用具     | 880        |2795.0000000000000000|
|0008       | 圆珠笔       | 办公用品     | 100        | 300.0000000000000000|

提示：其中的关键是 avg_sale_price 列。与习题三不同，这里需要计算出的 是各商品种类的平均销售单价。这与使用关联子查询所得到的结果相同。 也就是说，该列可以使用关联子查询进行创建。问题就是应该在什么地方使用这个关联子查询。

    
    CREATE VIEW AvgPriceByType AS 
    SELECT product_id, product_name, product_type, sale_price, 
          (SELECT AVG(sale_price)
           FROM product p2 
           WHERE p1.product_type = p2.product_type 
           GROUP BY p1.product_type) AS avg_sale_price 
    FROM product p1; 

## 5

运算或者函数中含有 NULL 时，结果全都会变为NULL ？（判断题）

正确。

## 6

对本章中使用的 product（商品）表执行如下 2 条 SELECT 语句，能够得到什么样的结果呢？

①

    SELECT product_name, purchase_price
      FROM product
     WHERE purchase_price NOT IN (500, 2800, 5000);

进货价格不为500, 2800, 5000的产品名称和进价。

②

    SELECT product_name, purchase_price
      FROM product
     WHERE purchase_price NOT IN (500, 2800, 5000, NULL);

查询结果为空，`NOT IN`的参数不能包含`NULL`。

## 7

按照销售单价（ sale_price）对练习 6.1 中的 product（商品）表中的商品进行如下分类。

- 低档商品：销售单价在1000日元以下（T恤衫、办公用品、叉子、擦菜板、 圆珠笔）
- 中档商品：销售单价在1001日元以上3000日元以下（菜刀）
- 高档商品：销售单价在3001日元以上（运动T恤、高压锅）

请编写出统计上述商品种类中所包含的商品数量的 SELECT 语句，结果如下所示。

执行结果

|low_price | mid_price | high_price |
|----------|-----------|------------|
|        5 |         1 |         2  |

    SELECT SUM(CASE WHEN sale_price < 1000 THEN 1 ELSE 0 END) AS low_price,
           SUM(CASE WHEN sale_price >= 1000 AND sale_price <= 3000 THEN 1 ELSE 0 END) AS mid_price,
           SUM(CASE WHEN sale_price > 3000 THEN 1 ELSE 0 END) AS high_price
    FROM product;

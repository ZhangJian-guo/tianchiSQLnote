本笔记为阿里云天池龙珠计划SQL训练营的学习内容，链接为：https://tianchi.aliyun.com/specials/promotion/aicampsql；

# 知识点总结

## 集合运算

SQL 语句的 UNION 会对两个查询的结果集进行合并和去重, 这种去重不仅会去掉两个结果集相互重复的, 还会去掉一个结果集中的重复行. 但在实践中有时候需要需要不去重的并集, 在 UNION 的结果中保留重复行的语法其实非常简单,只需要在 UNION 后面添加 ALL 关键字就可以了.

- 隐式类型转换：通常来说, 我们会把类型完全一致，并且代表相同属性的列使用 UNION 合并到一起显示, 但有时候, 即使数据类型不完全相同, 也会通过隐式类型转换来将两个类型不同的列放在一列里显示, 例如字符串和数值类型:

      SELECT product_id, product_name, '1'
        FROM product
       UNION
      SELECT product_id, product_name,sale_price
        FROM product2;

- MySQL 8.0 不支持交运算INTERSECT
- MySQL 8.0 还不支持 EXCEPT 运算，借助第六章学过的NOT IN 谓词, 我们同样可以实现表的减法。
- 对称差：两个集合A,B的对称差是指那些仅属于A或仅属于B的元素构成的集合。
- 两个集合的交可以看作是两个集合的并去掉两个集合的对称差。

## 连结(JOIN)

- 内连结：

      SELECT <1>, <2> FROM <tb_1> INNER JOIN <tb_2> ON <condition(s)>;
      
- 自然连结(NATURAL JOIN)：自然连结并不是区别于内连结和外连结的第三种连结, 它其实是内连结的一种特例–当两个表进行自然连结时, 会按照两个表中都包含的列名来进行等值内连结, 此时无需使用 ON 来指定连接条件。
- MySQL8.0 目前还不支持全外连结, 不过我们可以对左连结和右连结的结果进行 UNION 来实现全外连结。
- 非等值连结：包括比较运算符(<,<=,>,>=, BETWEEN)和谓词运算(LIKE, IN, NOT 等等)在内的所有的逻辑运算都可以放在 ON 子句内作为连结条件。
- 交叉连结—— CROSS JOIN(笛卡尔积)

# 练习题

# 4.1 

找出 product 和 product2 中售价高于 500 的商品的基本信息。

    SELECT * FROM product WHERE sale_price > 500 
    UNION 
    SELECT * FROM product2 WHERE sale_price > 500;

# 4.2

借助对称差的实现方式, 求product和product2的交集。

    SELECT * FROM
    (SELECT * FROM product
    UNION 
    SELECT * FROM product2)
    WHERE product_id NOT IN 
    (SELECT product_id FROM
    (SELECT * FROM product WHERE product_id NOT IN (SELECT product_id FROM product2)
    UNION
    SELECT * FROM product2 WHERE product_id NOT IN (SELECT product_id FROM product)));

# 4.3

每类商品中售价最高的商品都在哪些商店有售 ？

    SELECT shop_name FROM ShopProduct WHERE product_id IN
    (SELECT product_id 
    FROM product p1 
    WHERE sale_price = 
    (SELECT MAX(sale_price) 
    FROM product p2 
    WHERE p1.product_type = p2.product_type 
    GROUP BY p1.product_type));

# 4.4

分别使用内连结和关联子查询每一类商品中售价最高的商品。

    SELECT product_name
    FROM product p1
    INNER JOIN 
    (SELECT product_type, MAX(sale_price) AS sale_price FROM product GROUP BY product_type) AS p2
    ON p1.sale_price = p2.sale_price AND p1.product_type = p2.product_type;

    SELECT product_name
    FROM product p1 
    WHERE sale_price = 
    (SELECT MAX(sale_price) 
    FROM product p2 
    WHERE p1.product_type = p2.product_type 
    GROUP BY p1.product_type);

# 4.5

用关联子查询实现：在product表中，取出 product_id, product_name, sale_price, 并按照商品的售价从低到高进行排序、对售价进行累计求和。

    SELECT product_id, product_name, sale_price 
    FROM product p INNER JOIN 
    (SELECT p1.product_id, SUM(p2.sale_price) AS sum_sale_price 
    FROM product p1 CROSS JOIN product p1 
    WHERE p2.sale_price <= p1.sale_price) x
    ON p.product_id = x.product_id
    ORDER BY sale_price;
    
    SELECT product_id, product_name, sale_price, 
    (SELECT SUM(sale_price) 
    FROM product p2 
    WHERE p1.sale_price >= p2.sale_price) AS sum_sale_price
    FROM product p1 
    ORDER BY sale_price;

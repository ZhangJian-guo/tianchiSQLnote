本笔记为阿里云天池龙珠计划SQL训练营的学习内容，链接为：https://tianchi.aliyun.com/specials/promotion/aicampsql；

#  逻辑运算符：含有NULL时的真值

NULL的真值结果既不为真，也不为假

- TRUE and UNKNOWN is UNKNOWN, FALSE and UNKNOWN is FALSE
- TRUE or UNKNOWN is TRUE, FALSE or UNKNOWN is UNKNOWN

# 练习题-第一部分

## 练习题1

编写一条SQL语句，从product（商品）表中选取出“登记日期（regist在2009年4月28日之后”的商品，查询结果要包含product_name和regist_date两列。

    SELECT product_name, regist_date FROM product where regist_date > '2009-04-28';

## 练习题2

请说出对product 表执行如下3条SELECT语句时的返回结果。

①

    SELECT *
      FROM product
     WHERE purchase_price = NULL;

答：从product表中选出进货价格为空的数据

②

    SELECT *
      FROM product
     WHERE purchase_price <> NULL;

答：从product表中选出购买价格不为空的数据

③

    SELECT *
      FROM product
     WHERE product_name > NULL;
     
答：从product表中选出购买价格不为空的数据

## 练习题3

代码清单2-22（2-2节）中的SELECT语句能够从product表中取出“销售单价（saleprice）比进货单价（purchase price）高出500日元以上”的商品。请写出两条可以得到相同结果的SELECT语句。执行结果如下所示。

|product_name | sale_price | purchase_price |
|-------------|------------|----------------|
|T恤衫        |    1000    | 500            |
|运动T恤      |    4000    | 2800           |
|高压锅       |    6800    | 5000           |

    SELECT product_name, sale_price, purchase_price FROM product WHRER sale_price >= purchase_price + 500;
    SELECT product_name, sale_price, purchase_price FROM product WHRER sale_price - 500 >= purchase_price;

## 练习题4

请写出一条SELECT语句，从product表中选取出满足“销售单价打九折之后利润高于100日元的办公用品和厨房用具”条件的记录。查询结果要包括product_name列、product_type列以及销售单价打九折之后的利润（别名设定为profit）。

提示：销售单价打九折，可以通过saleprice列的值乘以0.9获得，利润可以通过该值减去purchase_price列的值获得。

    SELECT product_name, product_type, saleprice * 0.9 - purchase_price AS profit 
    FROM product 
    WHERE saleprice * 0.9 - purchase_price > 100 AND (product_type = '办公用品' OR product_type = '厨房用具')



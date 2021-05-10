# 1 初识数据库

数据库是将大量数据保存起来，通过计算机加工而成的可以 进行高效访问的数据集合。该数据集合称为数据库（Database，DB）。用来管理数据库的计算机系统称为数据库管理系统（Database Management System，DBMS）。

## 1.1 DBMS的种类


DBMS 主要通过数据的保存格式（数据库的种类）来进行分类，现阶段主要有以下 5 种类型：  

- 层次数据库（Hierarchical Database，HDB）  
- 关系数据库（Relational Database，RDB），这种类型的 DBMS 称为关系数据库管理系统（Relational Database Management System，RDBMS）。  
- 面向对象数据库（Object Oriented Database，OODB）  
- XML数据库（XML Database，XMLDB）  
- 键值存储系统（Key-Value Store，KVS），举例：MongoDB  

比较具有代表性的 RDBMS 有如下 5 种：  

  * Oracle Database：甲骨文公司的RDBMS
  * SQL Server：微软公司的RDBMS
  * DB2：IBM公司的RDBMS
  * PostgreSQL：开源的RDBMS
  * MySQL：开源的RDBMS

## 1.2 RDBMS的常见系统结构

RDBMS最常见的系统结构就是客户端 / 服务器类型（C/S类型）这种结构，此外还有

# 2 SQL介绍

##  2.1 SQL语言的类别

根据对 RDBMS 赋予的指令种类的不同，SQL 语句可以分为以下三类：

- DDL（Data Definition Language，数据定义语言） 用来创建或者删除存储数据用的数据库以及数据库中的表等对象。
- DML（Data Manipulation Language，数据操纵语言） 用来查询或者变更表中的记录。
- DCL（Data Control Language，数据控制语言） 用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对 RDBMS 的用户是否有权限操作数据库中的对象（数据库表等）进行设定。

## 2.2 SQL基本语句

### 2.2.1 创建数据库

    CREATE DATABASE < 数据库名称 > ;
    
### 2.2.2 创建表

    CREATE TABLE < 表名 >
    ( < 列名 1> < 数据类型 > < 该列所需约束 > ,
    < 列名 2> < 数据类型 > < 该列所需约束 > ,
    < 列名 3> < 数据类型 > < 该列所需约束 > ,
    < 列名 4> < 数据类型 > < 该列所需约束 > ,
    .
    .
    .
    < 该表的约束 1> , < 该表的约束 2> ,……);
    
创建表时，只能使用半角英文字母、数字、下划线（_）作为数据库、表和列的名称，名称必须以半角英文字母开头

数据类型

约束

### 2.2.3 删除和更新表

- 删除表：

      DROP TABLE < 表名 > ;

- 添加列：

      ALTER TABLE < 表名 > ADD COLUMN < 列的定义 >;

  如：添加一列可以存储100位的可变长字符串的 product_name_pinyin 列：

      ALTER TABLE product ADD COLUMN product_name_pinyin VARCHAR(100);

- 删除列：
      
      ALTER TABLE < 表名 > DROP COLUMN < 列名 >;

- 清空表内容：

      TRUNCATE TABLE TABLE_NAME;

  优点：相比`drop/delete`，`truncate`用来清除数据时，速度最快。

- 数据的更新：

      UPDATE <表名>
      SET <列名> = <表达式> [, <列名2>=<表达式2>...];  
      WHERE <条件>;  -- 可选，非常重要。
      ORDER BY 子句;  --可选
      LIMIT 子句; --可选

  使用 UPDATE 也可以将列更新为 NULL（该更新俗称为NULL清空）。此时只需要将赋值表达式右边的值直接写为 NULL 即可。
  和 INSERT 语句一样， UPDATE 语句也可以将 NULL 作为一个值来使用。
  **但是，只有未设置 NOT NULL 约束和主键约束的列才可以清空为NULL。**如果将设置了上述约束的列更新为 NULL，就会出错，这点与INSERT 语句相同。

- 多列更新

  UPDATE 语句的 SET 子句支持同时将多个列作为更新对象。

      UPDATE product
         SET sale_price = sale_price * 10,
             purchase_price = purchase_price / 2
       WHERE product_type = '厨房用具';  

- 插入数据

      INSERT INTO <表名> (列1, 列2, 列3, ……) VALUES (值1, 值2, 值3, ……);  
      
  对表进行全列 INSERT 时，可以省略表名后的列清单。这时 VALUES子句的值会默认按照从左到右的顺序赋给每一列。
  
# 3 练习题

## 3.1 

编写一条 CREATE TABLE 语句，用来创建一个包含表 1-A 中所列各项的表 Addressbook （地址簿），并为 regist_no （注册编号）列设置主键约束。

![表1-A](/images/表1-A.png,　"表 Addressbook （地址簿）中的列")

## 3.2

假设在创建练习1.1中的 Addressbook 表时忘记添加如下一列 postal_code （邮政编码）了，请把此列添加到 Addressbook 表中。  
列名 ： postal_code  
数据类型 ：定长字符串类型（长度为 8）  
约束 ：不能为 NULL

## 3.3

编写 SQL 语句来删除 Addressbook 表。

## 3.4

编写 SQL 语句来恢复删除掉的 Addressbook 表。

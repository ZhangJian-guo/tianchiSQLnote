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

##  2.1 SQL语言

根据对 RDBMS 赋予的指令种类的不同，SQL 语句可以分为以下三类：

- DDL（Data Definition Language，数据定义语言） 用来创建或者删除存储数据用的数据库以及数据库中的表等对象。
- DML（Data Manipulation Language，数据操纵语言） 用来查询或者变更表中的记录。
- DCL（Data Control Language，数据控制语言） 用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对 RDBMS 的用户是否有权限操作数据库中的对象（数据库表等）进行设定。


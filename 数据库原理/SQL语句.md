---
title: SQL语句
date: 2020-03-24 16:03:36
tags:
    - SQL
categories:
    - 数据库
---

# SQL 语句

## 单表查询-元组查询

+ 采用`DISTINCT`关键字去除重复，采用的集合操作去重。
  + 具体的`SELECT DISTINCT Sno FROM SC;`查询学生选课中的学号
+ 比较查询
  + `SELECT Sname FROM Students WHERE Sage<20` 
+ 范围查询
  + `select Sname, Sdept, Sage from Students where Sage (not) between 20 and 23` 
+ 确定集合查询
  + 查询信息系、数学系和计算机系学生的姓名和性别
    + `select Sname, Ssex from Students where Sdept (not) in (‘IS’, ‘MA’, ‘CS’);`
  + 数据是字符类型需要引号标注
+ 字符匹配查询
  + 用`LIKE`
  + `%`：任意长度的字符串。例如：a%b——acb, addgb,ab…
  + `_`：任意单个字符，例如：a_b——acb, adb…
  + 查询所有姓刘的学生的信息
    + `select Sname, Sno, Ssex from Students where Sname (not) like ‘刘%’；`
  + 查询以C开始和结束，并且有至少3个字母组成的系的学生.
    + `select Sno from Students where Sdept like ‘C_%C’;`
+ 空值查询
  + 查询缺考学生的学号和课号
    + `select Sno, Cno from SC where Grade is null`
+ 复合查询
  + 查询计算机系年龄在20岁以下的学生的姓名
    + `select Sname from Students where Sdept=‘CS’ and Sage<20;`
+ 对查询排序,asc 升序；desc降序,ORDER BY <列名> [asc | desc]
  + 查询选修了3号课程的学生学号和成绩，要求查询结果按成绩降序排列
    + `select Sno, Grade from SC where Cno=‘3’ order by Grade desc;`
  + 对于空值，若按升序排列，含空值的元组将在最后显示，若按降序排列，空值的元组将最先显示
  + 查询全体学生的情况，查询结果按系号升序排列，同一系的学生按年龄降序排列
    + `select * from Students order by Sdept asc, Sage desc;`

## 多表查询

+ 等值与非等值连接查询
  + 查询每个学生及其选修课的情况
    + `select Students.*, SC.* from Students, SC where Students.Sno = SC.Sno;`

+ 外连接
  + 查询学校内学生及雇员的情况。
    + (内)连接：既是学生，又是雇员。
      + `SELECT* FROM Students INNER JOIN Employee on ( Students.name = Employee.name )`
    + 左外连接：是学生，可以不是雇员。
      + `Students LEFT OUTER JOIN Employee`
    + 右外连接：可以不是学生，但，是雇员。
    + 全外连接：可以不是学生，可以不是雇员。
      + `Students FULL OUTER JOIN Employee`

## 嵌套查询

+ 不相关子查询(从里向外)
  + 带有in的子查询,可以嵌套多层
  + 查询与刘晨在一个系学习的生
    + 要求： 不使用连接查询
    + 
        ```
        SELECT * FROM STUDENTS WHERE Sdept IN (SELECT SDEPT FROM STUDENTS WHERE Sname="刘晨");
        ```
+ 相关子查询(从外向里)
  + exist


## 数据修改

+ 数据插入
  + `insert into TABLE_NAME values(xxx,xxx,xxx) `
  + `insert into TABLE_NAME(XX,XX) values(xx)`
+ 数据删除
  + `delete from TABLE_NAME where xxx`
  + 当不写where部分时就删除表中的所有元素
+ 数据修改
  + `update TABLE_NAME set xx=xx where xxx`


## 视图

视图是从一个或几个基本表（试图） 导出的表，视图时虚表，数据库只存放视图的定义（存放于数据字典中），不存放视图对应的数据。

### 视图的定义

+ 基于关系表建立视图
  + 创建信息系学生的视图
    + `create view IS_S as select * from Students where Stdep='IS'`
  + 查询转换
  + 更新一般涉及多个表则不进行


## 索引

查询相关

B+ 树索引

Hash（哈希）索引


## 数据库的完整性

数据库的完整性指数据的正确性和相容性，防止不含语义的数据进入数据库

整个机制
+ 定义功能
  + 实体完整性约束`PRIMARY KEY`
  + 参照完整性`FOREN KEY XX REFERENCE XX`
  + 用户定义完整性
    + 列值非空 `NOT NULL`
    + 列值唯一`UNIQUE`
    + 检查列值是否满足一个布尔表达式 `CHECK XXXXX`
+ 检查功能
  + 立即执行的约束，每执行一条检测一次
  + 延迟执行，执行一批检测一次
+ 违约反应
  + 级联删除：将参照关系中所有外码值与被参照关系中要删除元组主码值相对应的元组一起删除。
  + 受限删除：只有当参照关系中没有任何元组的外码值与要删除的被参照关系的元组的主码值相对应时，系统才执行删除操作，否则拒绝此删除操作。
  + 置空值删除：删除被参照关系的元组，并将参照关系中所有与被参照关系中被删除元组主码值相等的外码值置为空值
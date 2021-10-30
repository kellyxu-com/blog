---
layout: post
title: "SQL: How to Create and Execute a Stored Procedure"
author: "Kelly"
tags: [Database, SQL]
date: 2021-07-22 17:33:33 +0800
published: true
excerpt_separator: <!--more-->
---

<!--more-->
[toc]

# **基于ORACLE数据库存储过程的创建及调用**

大纲：
1. PLSQL编程:HELLO WORLD、程序结构、变量、流程控制、游标
2. 存储过程：概念、无参存储、有参存储（输入、输出）
3. JAVA调用存储存储过程


## **1. PLSQL编程**
### **1.1 概念和目的**

什么是PL/SQL?
1\. PL/SQL(Procedure Language/SQL)
2\. PLSQL是Oracle对sql语言的过程化扩展
3\. 指在SQL命令语言中增加了过程处理语句（如分支、循环等），使SQL语言具有过程处理能力。

### **1.2 程序结构**

通过PLSQL Developer工具的Test Window创建

提示：PLSQL是不区分大小写的

PLSQL可以分成三个部分：声明部分、可执行部分、异常处理部分

```SQL
-- Created on 2021/7/22 by KELLY

declare
  -- Local variables here
  --声明变量、游标
  i integer;

begin
  -- Test statements here
  -- 执行语句
  -- 异常处理部分

end;
```

DECALRE部分用来声明变量或者游标(结果集类型变量)，如果程序中无变量声明可以省略掉

### **1.3 Hello World**

```SQL
--打印HELLO WORLD

begin
  -- Test statements here
  DBMS_OUTPUT.put_line('Hello World');

end;
```

用cmd方式：

```SQL
SQL>
SQL> --打印HELLO WORLD
SQL> begin
  2    -- Test statements here
  3    DBMS_OUTPUT.put_line('Hello World');
  4  
  5  end;
  6  /

PL/SQL procedure successfully completed

SQL> set serveroutput on
SQL> --打印HELLO WORLD
SQL> begin
  2    -- Test statements here
  3    DBMS_OUTPUT.put_line('Hello World');
  4  
  5  end;
  6  /

Hello World

PL/SQL procedure successfully completed
```

### **1.4 变量**

PLSQL编程中常见的变量分为两大类：  
1. 普通数据类型（char, varchar2, date, number, boolean, long）  
2. 特殊变量类型（引用型变量、记录型变量）  
引用型：取决于表中字段的类型  
记录型：变量接受的不是一个字段的值，而是一整条的值

声明变量的方式为：   

```sql
1 变量名 变量类型（变量长度） 例如：v_name varchar2(20);
```

#### **1.4.1 普通变量**

变量赋值方式有两种：
1\. 直接赋值语句
2\. 语句赋值，使用select...into...赋值：（语法select值into变量）

【示例】打印人员个人信息，包括：姓名、薪水、地址

```SQL
-- 打印人员个人信息，包括：姓名、薪水、地址
declare
  -- 姓名
  v_name varchar2(20) := '张三';
  --薪水
  v_salary NUMBER;
  --地址
  v_addr varchar2(200);

begin
  -- 直接赋值
  v_salary := 1580;
  --语句赋值
  SELECT '深圳福田' INTO v_addr FROM dual;

  --打印输出
  dbms_output.put_line('姓名：' || v_name || '，薪水：' || v_salary || '，地址：' ||
                       v_addr);
end;
```

#### **1.4.2 引用型变量**

变量的类型和长度取决于表中字段的类型和长度
通过 **表明、列明%TYPE** 指定变量的类型和长度，例如：v_name emp.ename%TYPE;
【示例】查询emp表中7839号员工的个人信息，打印姓名和薪水   

```SQL
-- 查询emp表中7839号员工的个人信息，打印姓名和薪水
declare
  -- 姓名
  v_name EMP.ENAME%TYPE := ('张三'); --声明变量直接赋值
  --薪水
  v_salary EMP.SALARY%TYPE;

begin
  SELECT ename, salary INTO v_name, v_salary FROM emp WHERE empno = 7839;

  --打印输出
  dbms_output.put_line('姓名：' || v_name || '，薪水：' || v_salary);
end;
```

引用型变量的好处：

使用普通变量定义方式，需要知道表中的列的类型，而使用引用类型，不需要考虑列的类型，使用%TYPE是非常好的编程风格，因为它让PL/SQL更加灵活，更加适用于对数据库的定义的更新。

#### **1.4.3 记录型变量**

接受表中的一整行记录，相当于JAVA中的一个对象     

语法：变量名称、表名%ROWTYPE，例如：v_emp emp%rowtype;     

【示例】
查询并打印7839号员工的姓名和薪水

```SQL
-- 查询emp表中7839号员工的个人信息，打印姓名和薪水
declare
  --记录型变量
  v_emp emp%ROWTYPE;

begin
  SELECT * INTO v_emp FROM emp WHERE empno = 7839;

  --打印输出
  DBMS_OUTPUT.PUT_LINE('姓名：' || v_emp.ename || '，薪水：' || v_emp.salary);
end;
```

【慎用】如果有一个表，有100个字段，那么程序如果要使用100个字段的话，如果使用引用型变量一个个声明，会特别麻烦，记录型变量可以方便的解决这个问题

错误的使用：

1.  记录型变量只能储存一个完整的行数据
2.  返回的行太多了，记录型变量也接受不了

### **1.5 流程控制**

#### **1.5.1 条件控制**

语法：

```SQL
BEGIN
  IF 条件1 THEN 执行1

  ELSIF 条件2 THEN 执行2

  ELSE 执行3

  END IF;

END;
```

注意关键字：ELSIF

【示例】判断emp表中记录是否超过20条，10-20之间，或者10条一下

```SQL
-- 判断emp表中记录是否超过20条，10-20之间，或者10条一下
DECLARE
  --声明变量接受emp中的数量
  v_count NUMBER;

BEGIN
  SELECT count(1) INTO v_count FROM EMP;

  IF v_count > 20 THEN
    dbms_output.put_line('emp表中的记录数超过20条：' || v_count);
  ELSIF v_count >= 10 THEN
    dbms_output.put_line('emp表中的记录数超过10-20条：' || v_count);
  ELSE
    dbms_output.put_line('emp表中的记录数超过10条：' || v_count);

  END IF;

END;
```

#### **1.5.2循环**

在ORACLE中有三种循环方式，loop循环

```SQL
BEGIN
 LOOP
   EXIT WHEN 退出循环条件

 END LOOP;

END;
```

【示例】打印数字1-10

```SQL
-- 打印数字1-10
DECLARE
  --声明循环变量
  V_NUM NUMBER := 1;

BEGIN
  LOOP

    EXIT WHEN V_NUM > 10;

    DBMS_OUTPUT.put_line(V_NUM);

    --循环变量的自增

    V_NUM := V_NUM + 1;

  END LOOP;

END;
```

## **2. 游标**

### **2.1 什么是游标**

用于临时存储一个查询返回的多行数据（结果集，类似于JAVA的Jdbc连接返回的ResultSet集合），通过遍历游标，可以逐行访问处理该结果集的数据。

游标的使用方式：声明-->打开-->读取-->关闭

### **2.2 语法**

游标声明：

CURSOR 游标明[(参数列表)] IS 查询语句;

游标的打开：OPEN游标名;

游标的取值：FETCH游标名INTO变量列表;

游标的关闭：CLOSE游标名;

### **2.3 游标的属性**

| 游标的属性     | 返回值类型 | 说明                      |
| --------- | ----- | ----------------------- |
| %ROWCOUNT | 整型    | 获得FETCH语句返回的数据行数        |
| %FOUND    | 布尔型   | 最近的FETCH语句返回一行数据为真，否则为假 |
| %NOTFOUND | 布尔型   | 与%属性返回值相反               |
| %ISOPEN   | 布尔型   | 游标已经打开时值为真，否则为假         |


其中%NOTFOUND是在游标中找不到元素的时候返回TRUE，通常用来判断退出循环

### **2.4 创建和使用**

【示例】使用游标查询emp表中所有员工的姓名和工资，并将其依次打印出来。

```SQL
-- 使用游标查询emp表中所有员工的姓名和工资，并将其依次打印出来。
DECLARE
  -- 声明游标
  CURSOR c_emp IS
    SELECT ename, sal FROM emp;

  --声明变量接受游标中的数据
  v_ename emp.ename%Type;
  v_sal   emp.sal%Type;

BEGIN
  --打开游标
  OPEN c_emp;

  --遍历游标
  LOOP

    --获取游标中的数据
    FETCH c_emp
      INTO v_ename, v_sal;

    --退出循环条件
    EXIT WHEN c_emp%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE(V_ENAME || '-' v_sal);

  END LOOP;

  --关闭游标
  CLOSE c_emp;

end;
```
### **2.5 带参数的游标**
【示例】使用游标查询并打印某部门的员工的姓名和薪资，部门编号为运行时手动输入
```SQL
-- 使用游标查询并打印某部门的员工的姓名和薪资，部门编号为运行时手动输入
DECLARE
  -- 声明游标传递参数
  CURSOR c_emp(V_EMPNO EMP.EMPNO%TYPE) IS
    SELECT ename, sal FROM emp WHERE EMPNO = V_EMPNO;

  --声明变量接受游标中的数据
  v_ename emp.ename%Type;
  v_sal   emp.sal%Type;

BEGIN
  --打开游标
  OPEN c_emp(10);

  --遍历游标
  LOOP

    --获取游标中的数据
    FETCH c_emp
      INTO v_ename, v_sal;

    --退出循环条件
    EXIT WHEN c_emp%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE(V_ENAME || '-' v_sal);

  END LOOP;

  --关闭游标
  CLOSE c_emp;

end;
```
注意：%NOTFOUND属性的默认值为FALSE，所以在循环中要注意判断条件的位置。如果先判断在FETCH会导致最后一条的值被打印两次（多循环一次默认）


## **3. 存储过程**
### **3.1 概念作用**

> 存储过程（Stored Procedure）是在大型数据库系统中，一组为了完成特定功能的SQL 语句集，它存储在数据库中，一次编译后永久有效，用户通过指定存储过程的名字并给出参数（如果该存储过程带有参数）来执行它。存储过程是数据库中的一个重要对象。在数据量特别庞大的情况下利用存储过程能达到倍速的效率提升。

### **3.2 语法**
```SQL
CREATE OR REPLACE PROCEDURE 过程名称[(参数列表)] IS
BEGIN

END[过程名称];
```
根据参数的类型，我们将其分为3类讲解：      

| 不带参数的      
| 带参数的     
| 带输入输出参数（返回值）的      

### **3.3 无参存储**
#### **3.3.1 创建存储**

通过PLSQL Developer工具或者语句创建存储过程：
"Program Window->Procedure"

【示例】通过调用存储过程打印hello world

创建存储过程：

```SQL
create or replace procedure P_hello as
  --声明变量
begin
  dbms_output.put_line('hello world');
end P_hello;
```

#### **3.3.2 调用存储过程**            

1. 通过PLSQL程序调用

```SQL
-- 通过plsql调用存储过程
begin
  p_hello;
end;
```

2. 在SQLPLUS中通过EXEC命令调用:

```SQL
SQL> exec p_hello;
hello world
```

注意：
1. is和as是可以互用的，用哪个都没关系的
2. 过程中没有declare关键字，declare用在语句块中

### **3.4 带输入参数的存储过程**
【示例】查询并打印某个员工（如7839号员工）的姓名和薪水--存储过程：要求，调用的时候传入员工编号，自动控制台打印。        
```SQL
--查询并打印某个员工（如7839号员工）的姓名和薪水--存储过程：要求，调用的时候传入员工编号，自动控制台打印。
create or replace procedure p_querynameandsal(i_empno IN emp.empno%TYPE) as
--声明变量
v_name emp.ename%TYPE;
v_sal  emp.sal%TYPE;

BEGIN
--查询emp表中某个员工的姓名和薪水并赋值给变量

SELECT ename, sal INTO v_mame, v_sal FROM emp WHERE empno = i_empno;
dbms_output.put_line(V_NAME || '_' || V_SAL);
END p_querynameandsal;
```

### **3.5 带输出参数的存储过程**
【示例】输入员工号查询某个员工（7839号员工）信息，要求，将薪水作为返回值输出，给调用的程序使用。

```SQL
--查询并打印某个员工（如7839号员工）的姓名和薪水--存储过程：要求，调用的时候传入员工编号，自动控制台打印。
create or replace procedure p_querynameandsal_out(i_empno IN emp.empno%TYPE,
                                                  o_sal   OUT emp.sal%TYPE) as
--声明变量

BEGIN
--查询emp表中某个员工的姓名和薪水并赋值给变量

SELECT sal INTO o_sal FROM emp WHERE empno = i_empno;
END p_querynameandsal;
```

调用存储过程：

```SQL
-- 通过plsql调用存储过程

declare
  --声明变量接受存储过程中的输出参数
  v_sal emp.sal%TYPE;

begin
  p_querysal_out(7839, v_sal);

  dbms_output.put_line(v_sal);

end;
```

测试：D:\Desktop\XJY\Intern & Job\210705 CT Intern\Database\sql学习->student_query_input;student_query_output

### **3.6 JAVA程序调用存储过程**        
#### **3.7.1 分析jdk API**          
通过Connection对象的prepareCall方法可调用存储过程
得出结论：通过Connection对象调用prepareCall方法传递一个转义sql语句调用存储过程，输入参数直接调用set方法传递，输出参数需要注册后，执行存储过程，通过get方法获取，参数列表的下标是从1开始的

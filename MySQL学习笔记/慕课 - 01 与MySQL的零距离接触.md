# 慕课 - 01与MySQL的零距离接触

## 01 初涉MySQL

### MySQL 登录参数

|         参数          |        描述         |
| --------------------- | ------------------ |
| -D, --database=name   | 打开指定数据库      |
| --delimiter = name    | 指定分隔符          |
| -h, --host=name       | 服务器名称          |
| -p, --password[=name] | 密码                |
| -P, --port=#          | 端口号              |
| --prompt=name         | 设置提示符          |
| -u, --user=name       | 用户名              |
| -V, --version         | 输出版本信息并且退出 |

### MySQL 退出
```sql
mysql > exit;
mysql > quit;
mysql > \q;
```

### MySQL 提示符
#### 1. 修改方式
1. 登录时输入 `--prompt`
2. 进入后输入`prompt`
#### 2. 提示符参数

| 参数 |   描述    |
| ---- | --------- |
| \D   | 完整的日期 |
| \d   | 当前数据库 |
| \h   | 服务器名称 |
| \u   | 当前用户   |

#### 3. 参数组合
可以将多个参数组合
```sql
prompt \u@\h \d;
```

### MySQL 语句规范
 - 关键字与函数名称`全部大写`
 - 数据库名称、表名称、字段名称`全部小写`
 - SQL语句必须以`分号结尾`

### MySQL 常用系统命令
- 显示当前服务器版本 `SELECT VERSION();`
- 显示当前日期时间 `SELECT NOW();`
- 显示当前用户 `SELECT USER();`
- 更改客户端显示编码 `SET NAMES gbk/utf8;`  

### 操作数据库
#### 1. 创建数据库
```mysql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] charset_name
```
- `[IF NOT EXISTS]` 创建数据库时忽略错误的产生*。(查看警告信息：`SHOW WARNINGS;`)*
- `SHOW CREATE DATABASE t1` 查看创建数据库时的代码
#### 2. 查看数据库列表
```mysql
SHOW {DATABASES | SCHEMA} [LIKE 'pattern' | WHERE expr]
```
#### 3. 修改数据库
```mysql
ALTER {DATABASE | SCHEMA} [db_name] [DEFAULT] CHARACTER　SET [=] charset_name
```
#### 4. 删除数据库
```mysql
DROP {DATABASE | SCHEMA} [IF EXISTS] db_name
```

- - - - -

## 02 数据类型与操作数据表

### 数据类型
#### 1. 整形

|  数据类型  | 存储范围 | 字节 |
| --------- | ------- | ---- |
| TINYINT   |         | 1    |
| SMALLINT  |         | 2    |
| MEDIUMINT |         | 3    |
| INT       |         | 4    |
| BIGINT    |         | 8    |

#### 2. 浮点

|    数据类型    |                 存储范围                 |
| ------------- | --------------------------------------- |
| FLOAT[(M,D)]  | M是数字总位数，D是小数点后面的位数。`M>=D` |
| DOUBLE[(M,D)] |                                         |

#### 3. 日期时间型
很少用，会采用数字类型来取代。使用PHP的方式来存储

|  数据类型  | 存储范围 | 字节 |
| --------- | ------- | ---- |
| YEAR      |         | 1    |
| TIME      |         | 3    |
| DATE      |         | 3    |
| DATETIME  |         | 8    |
| TIMESTAMP |         | 4    |

#### 4. 字符型

|           数据类型           |                       存储范围                        |
| --------------------------- | ---------------------------------------------------- |
| CHAR(M)                     | M个字节，0<=M<=2                                      |
| VARCHAR(M)                  | L+1个字节，L<=M,且0<=M<=65535                         |
| TINYTEXT                    | L+1个字节，L < 2^8                                    |
| TEXT                        | L+2个字节，L < 2^16                                   |
| MEDIUMTEXT                  | L+3个字节，L < 2^24                                   |
| LONGTEXT                    | L+4个字节，L < 2^32                                   |
| ENUM('value1','value2',...) | 1或2个字节，取决于枚举值的个数（最多65535个值）          |
| SET('value1','value2',...)  | 1、2、3、4或者8个字节，取决于set成员的数目(最多64个成员) |

### 数据表操作
数据表是数据库最重要的组成部分之一，是其他对象的基础。
#### 1. 打开数据库
```sql
USE 数据库名称
```
```sql
mysql -uroot -proot -P3306 -h127.0.0.1
SHOW DATABASES;
USE test;
SELECT DATABASE();
```
#### 2. 创建数据表
```sql
CREATE TABLE [IF NOT EXISTS] table_name (
    column_name data_tyle,
    ...
)
```
```sql
CREATE TABLE tb1(
    username VARCHAR(20),
    age TINYINT UNSiGNED,
    salary FLOAT(8,2) UNSIGNED
);
```
#### 3. 查看数据表列表
```sql
SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr]
```
#### 4. 查看数据表结构
```sql
SHOW COLUMNS FROM tbl_name
```
### MySQL 记录的插入与查找
#### 1. 插入记录
```sql
INSERT [INTO] tbl_name [(col_name,...)] VALUES(VAL,...)
```
#### 2. 记录查找
```sql
SELECT expr, ... FROM tbl_name
```
### MySQL 列定义
#### 1. MySQL 空值与非空
`NULL` 字段值可以为空  
`NOT NULL` 字段值禁止为空

```sql
CREATE TABLE tb2(
    user name VARCHAR(20) NOT NULL,
    age TINYINT UNSIGNED NULL
);
```
#### 2. MySQL 自动编号
`AUTO_INCREMENT`
- 自动编号，必须与主键配合使用  
- 默认情况下，起始值为1，每次的增量为1  

```sql
CREATE TABLE tb3(
    id SMALLINT UNSIGNED  AUTO_INCREMENT,
    username VARCHAR(30) NOT NULL
);
```
*因为这里没有定义`自动编号`为`主键`，所以创建失败*
#### 3. 主键约束
`PRIMARY KEY`  
- 主键约束  
- 每张数据表只能存在一个主键  
- 主键保证记录的唯一性  
- 主键自动为`NOT NULL`  

```sql
CREATE TABLE tb3(
    id SMALLINT UNSIGNED  AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(30) NOT NULL
);
```
#### 4. 唯一约束
`UNIQUE KEY`
- 唯一约束
- 可以保证记录的唯一
- 可以存在`NULL`
- 每张表可以存在多个`唯一约束`

```sql
CREATE TABLE tb5（
    id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(20) NOT NULL UNIQUE KEY,
    age TINYINT UNSIGNED
）;
```
#### 5. 默认约束
`DEFAULT`
- 默认值
- 当插入记录时，自动赋予默认值

```sql
CREATE TABLE tb6(
    id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(20) NOT NULL UNIQUE KEY,
    sex ENUM('1','2','3') DEFAULT '3'
);
```

- - - - -

## 03 约束以及修改数据表

### 约束
1. 保证数据完整性和一致性
2. 分为`表级约束`和`列级约束`
3. 类型包括
    1. NOT NULL(非空约束)
    2. PRIMARY KEY(主键约束)
    3. UNIQUE KEY(唯一约束)
    4. DEFAULT(默认约束)
    5. FOREIGN KEY(外键约束)
### 外键约束
`FOREIGN KEY`
#### 1. 要求解析
1. 父表和子表使用相同的存储引擎，禁止使用临时表
2. 数据表的存储引擎只能为`InnoDB`
3. 外键列和参照列必须使用相似的数据类型。数字的长度或符号位必须相同;字符的长度可以不同
4. 外键列和参照列必须创建索引。MySQL会自动为外键列创建索引。(这里老师讲的有问题，和PPT不同)
> MySQL配置文件
`default-storage-engine=INNODB`

```sql
USE test
CREATE TABLE provinces(
    id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    pname VARCHAR(20) NOT NULL
);
SHOW CREATE TABLE provinces;
CREATE TABLE users(
    id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(10) NOT NULL,
    pid SMALLINT UNSIGNED,
    FOREIGN KEY (pid) REFERENCES provinces (id)
);
SHOW INDEXES FROM provinces;
SHOW INDEXES FROM provinces\G; //以表格形式呈现
SHOW INDEXES FROM users\G;
SHOW CREATE TABLE users;
```

#### 2. 参照操作
1. `CASCADE` : 从父表删除或更新且自动删除或更新子表中匹配的行
2. `SET NULL` : 从父表删除或更新行，并设置子表中的外键列为NULL。如果使用该选项，必须保证子表列没有制定`NOT NULL`
3. `RESTRICT` : 拒绝对父表的删除或更新操作
4. `NO ACTION` : 标准SQL关键字，在MySQL中与`RESTRICT`相同
##### 2.1. CASCADE
```sql
CREATE TABLE users1(
    id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(10) NOT NULL,
    pid SMALLINT UNSIGNED,
    FOREIGN KEY (pid) REFERENCES provinces (id) ON DELETE CASCADE
);
```
```sql
INSERT provinces(pname) VALUES('A');
INSERT provinces(pname) VALUES('B');
INSERT provinces(pname) VALUES('C');
SELECT * FROM provinces;
INSERT users1(username, pid) VALUES('Tom', 3);
SELECT * FROM users1;
DELETE FROM provinces WHERE id=3;
SELECT * FROM provinces;
SELECT * FROM users1;
```
#### 3. 表级约束与列级约束
- 对一个数据列建立的约束，成为列级约束
- 对多个数据列建立的约束，成为表级约束
- 列级约束级可以在列定义时声明，也可以在列定义后声明
- 表级约束只能在列定义后声明

### 修改数据表
#### 1. 添加、删除列
##### 1.1. 添加单列
```sql
ALTER TABLE tbl_name ADD [COLUMN] col_name column_definition [FIRST|AFTER col_name]
```
```sql
SHOW COLUMNS FROM users1;
ALTER TABLE users1 ADD age TINYINT UNSIGNED NOT NULL DEFAULT 10;
SHOW COLUMNS FROM users1;
ALTER TABLE users1 ADD password VARCHAR(32) NOT NULL AFTER username;
SHOW COLUMNS FROM users1;
ALTER TABLE users1 ADD truename VARCHAR(20) NOT NULL FIRST;
SHOW COLUMNS FROM users1;
```
##### 1.2. 添加多列
```sql
ALTER TABLE tbl_name ADD [COLUMN] (col_name column_definition, ...)
```
##### 1.3. 删除列
```sql
ALTER TABLE tbl_name DROP [COLUMN] col_name
```
```sql
SHOW COLUMNS FROM users1;
ALTER TABLE users1 DROP truename; 
SHOW COLUMNS FROM users1;
ALTER TABLE users1 DROP password, DROP age;
SHOW COLUMNS FROM users1;
ALTER TABLE users1 DROP username, ADD nickname VARCHAR(32) NOT NULL;
SHOW COLUMNS FROM users1;
```
#### 2. 添加约束
##### 2.1. 添加主键约束
```sql
ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] PRIMARY KEY [index_type] (index_col_name, ...)
```
```sql
CREATE TABLE users2(
    username VARCHAR(10) NOT NULL,
    pid SMALLINT UNSIGNED
);
SHOW CREATE TABLE users2;
ALTER TABLE users2 ADD id SMALLINT UNSIGNED;
SHOW COLUMNS FROM users2;
ALTER TABLE users2 ADD CONSTRAINT PK_users2_id PRIMARY KEY (id);
SHOW COLUMNS FROM users2;
```
##### 2.2. 添加唯一约束
```sql
ALTER TABLE tbl_name ADD [CONSTRAINT [SYMBOL]] UNQIUE [INDEX | KEY] [index_name] [index_type] (index_col_name, ...)
```
```sql
ALTER TABLE users2 ADD UNIQUE (username);
SHOW CREATE TABLE users2\G;
ALTER TABLE users2 ADD FOREIGN KEY (pid) REFERENCES provinces (id);
SHOW CREATE TABLE users2\G;
```
##### 2.3. 添加/删除默认约束
```sql
ALTER TABLE tbl_name ALTER [COLUMN] col_name {SET DEFAULT literal | drop default}
```
```sql
ALTER TABLE users2 ADD age TINYINT UNSIGNED NOT NULL;
SHOW COLUMNS FROM users2;
ALTER TABLE users2 ALTER age SET DEFAULT 15;
SHOW COLUMNS FROM users2;
```
#### 3. 删除约束
##### 3.1. 删除主键约束
```sql
ALTER TABLE tbl_name DROP PRIMARY KEY
```
```sql
ALTER TABLE users2 DROP PRIMARY KEY;
SHOW COLUMNS FROM users2;
SHOW INDEXES FROM users2\G;
ALTER TABLE users2 DROP INDEX username;
SHOW INDEXES FROM users2\G;
```
##### 3.2. 删除外键约束
```sql
ALTER TABLE tbl_name DROP FOREIGN KEY fk_symbol
```
```sql
SHOW CREATE TABLE users2;
ALTER TABLE users2 DROP FOREIGN KEY users2_ibfk_1;
SHOW CREATE TABLE users2;
ALTER TABLE users2 DROP INDEX pid;
SHOW CREATE TABLE users2;
```
#### 4. 修改数据表
##### 4.1. 修改列定义
```sql
ALTER TABLE tbl_name MODIFY [COLUMN] col_name column_definition [FIRST | AFTER col_name]
```
```sql
SHOW COLUMNS FROM users2;
ALTER TABLE users2 MODIFY id SMALLINT UNSIGNED NOT NULL FIRST;
SHOW COLUMNS FROM users2;
ALTER TABLE users2 MODIFY id TINYINT UNSIGNED NOT NULL;
SHOW COLUMNS FROM users2;
```
##### 4.2. 修改列名称
```sql
ALTER TABLE tbl_name CHANGE [COLUMN] old_col_name new_col_name column_definition [FIRST | AFTER col_name]
```
```sql
SHOW COLUMNS FROM users2;
ALTER TABLE users2 CHANGE pid p_id TINYINT UNSIGNED NOT NULL;
SHOW COLUMNS FROM users2;
SHOW COLUMNS FROM users2;
```
##### 4.3. 修改表名称
```sql
ALTER TABLE tbl_name RENAME [TO | AS] new_tbl_name
```
```sql
RENAME TABLE tbl_name TO new_tbl_name [, tbl_name2 TO new_tbl_name2] ...
```
```sql
ALTER TABLE users2 RENAME users3;
SHOW TABLES;
RENAME TABLE users3 TO users2;
SHOW TABLES;
```

- - - - -

## 04 操作数据表中的记录

### 插入记录
#### 1. INSERT
```sql
INSERT [INTO] tbl_name [(col_name)] {VALUES | VALUE} ({expr | DEFAULT}, ...),(...)...
```
```sql
USE test;
CREATE TABLE users(
    id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(20) NOT NULL,
    password VARCHAR(32) NOT NULL,
    age TINYINT UNSIGNED NOT NULL DEFAULT 10,
    sex BOOLEAN
);
INSERT users VALUES(NULL, 'Tom', '123', 25, 1);
SELECT * FROM users;
INSERT users VALUES(DEFAULT, 'John', '123', 25, 1);
SELECT * FROM users;
INSERT users VALUES(DEFAULT, 'John', '123', 3*7-5, 1);
SELECT * FROM users;
INSERT users VALUES(DEFAULT, 'John', '123', DEFAULT, 1);
SELECT * FROM users;
INSERT users VALUES(DEFAULT, 'John', '123', DEFAULT, 1),(NULL, 'Rose', md5('123'), DEFAULT, 1);
SELECT * FROM users;
```
#### 2. INSERT SET-SELECT
##### 2.1. SET
这种方法可以使用子查询(`SubQuery`)

```sql
INSERT [INTO] tbl_name SET col_name={expr | DEFAULT}, ...
```
```sql
INSERT users SET username='Ben', password='456';
SELECT * FROM users;
```
##### 2.2. SELECT
此方法可以将查询结果插入到指定数据表
```sql
INSERT [INTO] tbl_name [(col_name,...)] SELECT ...
```

### 单表更新记录 UPDATE
```sql
UPDATE [LOW_PRIORITY] [IGNORE] table_reference SET col_name1={expr1 | DEFAULT} [, col_name2={expr2 | DEFAULT}] ... [WHERE where_condition]
```
```sql
UPDATE users SET age = age + 5;
SELECT * FROM users;
UPDATE users SET age = age - id, sex = 0;
SELECT * FROM users;
UPDATE users SET age = age + 10 WHERE id % 2 = 0;
SELECT * FROM users;
```
### 单表删除记录 DELETE
```sql
DELETE FROM tbl_name [WHERE where_condition]
```
```sql
DELETE FROM users WHERE id = 6;
SELECT * FROM users;
INSERT users VALUES(NULL, '111','222', 33, NULL);
SELECT * FROM users;
```

### 查询表达式解析
#### 1. SELECT
```sql
SELECT select_expr [, select_expr]
[
    FROM table_references
    [WHERE where_condition]
    [GROUP BY {col_name | position} [ASC | DESC], ...]
    [HAVING where_condition]
    [ORDER BY {col_name | expr | position} [ASC | DESC], ...]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
]
```
#### 2. select_expr
查询表达式
> 每一个表达式表示想要的一列，必须有至少一个。  
> 多个列之间以英文逗号分隔。  
> 星号(`*`)表示所有列。`tbl_name.*` 可以表示命名表的所有列。  
> 查询表达式可以使用[ AS ] alias_name 为其赋予别名。  
> 别名可用于 `GROUP BY`, `ORDRE BY` 或 `HAVING` 子句。

```sql
SHOW COLUMNS FROM users;
SELECT id, username FROM users;
SELECT username, id FROM users;
SELECT users.id, users.username FROM users;
SELECT id AS userId, username AS uname FROM users;
```

### WHERE 语句进行条件查询
条件表达式。省略 `WHERW` 则显示所有记录。  
其中可以使用函数和表达式。
### GROUP BY 语句对查询结果分组
```sql
[GROUP BY {col_name | position} [ASC | DESC], ...]
```
```sql
SELECT sex FROM users GROUP BY sex;
SELECT sex FROM users GROUP BY 1; //按 SELECT 中第一个出现的字段分组
```
### HAVING 语句设置分组条件
```sql
[HAVING where_condition]
```
```sql
SELECT sex, age FROM users GROUP BY 1 HAVING age > 35;
SELECT sex FROM users GROUP BY 1 HAVING count(id) >= 2;
```
### ORDER BY 语句对查询结果排序
```sql
 [ORDER BY {col_name | expr | position} [ASC | DESC], ...]
```
```sql
SELECT * FROM users;
SELECT * FROM users ORDER BY id DESC;
SELECT * FROM users ORDER BY age;
SELECT * FROM users ORDER BY age, id DESC;
```
### LIMIT 语句限制查询数量
*可以用于 PHP 的分页显示*
```sql
[LIMIT {[offset,] row_count | row_count OFFSET offset}]
```
```sql
SELECT * FROM users;
SELECT * FROM users LIMIT 2;
SELECT * FROM users LIMIT 3, 2; //排位与编号没关系。编号从 0 开始
SELECT * FROM users ORDER BY id DESC LIMIT 2, 2;
```

### INSERT SELECT
```sql
INSERT [INTO] tbl_name SET col_name={expr | DEFAULT}, ...
```
```sql
CREATE TABLE test(
    id TINYINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(20)
);
INSERT test(username) SELECT username FROM users WHERE age >= 30;
SELECT * FROM test;
```

- - - - -

## 05 子查询与连接

### 数据准备
```sql 
CREATE TABLE IF NOT EXISTS tdb_goods(
    goods_id    SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    goods_name  VARCHAR(150) NOT NULL,
    goods_cate  VARCHAR(40)  NOT NULL,
    brand_name  VARCHAR(40)  NOT NULL,
    goods_price DECIMAL(15,3) UNSIGNED NOT NULL DEFAULT 0,
    is_show     BOOLEAN NOT NULL DEFAULT 1,
    is_saleoff  BOOLEAN NOT NULL DEFAULT 0
);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('R510VC 15.6英寸笔记本','笔记本','华硕','3399',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('Y400N 14.0英寸笔记本电脑','笔记本','联想','4899',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('G150TH 15.6英寸游戏本','游戏本','雷神','8499',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('X550CC 15.6英寸笔记本','笔记本','华硕','2799',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('X240(20ALA0EYCD) 12.5英寸超极本','超级本','联想','4999',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('U330P 13.3英寸超极本','超级本','联想','4299',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('SVP13226SCB 13.3英寸触控超极本','超级本','索尼','7999',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('iPad mini MD531CH/A 7.9英寸平板电脑','平板电脑','苹果','1998',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('iPad Air MD788CH/A 9.7英寸平板电脑 （16G WiFi版）','平板电脑','苹果','3388',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES(' iPad mini ME279CH/A 配备 Retina 显示屏 7.9英寸平板电脑 （16G WiFi版）','平板电脑','苹果','2788',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('IdeaCentre C340 20英寸一体电脑 ','台式机','联想','3499',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('Vostro 3800-R1206 台式电脑','台式机','戴尔','2899',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('iMac ME086CH/A 21.5英寸一体电脑','台式机','苹果','9188',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('AT7-7414LP 台式电脑 （i5-3450四核 4G 500G 2G独显 DVD 键鼠 Linux ）','台式机','宏碁','3699',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('Z220SFF F4F06PA工作站','服务器/工作站','惠普','4288',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('PowerEdge T110 II服务器','服务器/工作站','戴尔','5388',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('Mac Pro MD878CH/A 专业级台式电脑','服务器/工作站','苹果','28888',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES(' HMZ-T3W 头戴显示设备','笔记本配件','索尼','6999',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('商务双肩背包','笔记本配件','索尼','99',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('X3250 M4机架式服务器 2583i14','服务器/工作站','IBM','6888',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('玄龙精英版 笔记本散热器','笔记本配件','九州风神','',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES(' HMZ-T3W 头戴显示设备','笔记本配件','索尼','6999',DEFAULT,DEFAULT);
INSERT tdb_goods (goods_name,goods_cate,brand_name,goods_price,is_show,is_saleoff) VALUES('商务双肩背包','笔记本配件','索尼','99',DEFAULT,DEFAULT);
SELECT * FROM tdb_goods\G;
```

### MySQL 子查询简介
子查询是指在另一个查询语句中的SELECT子句。*子查询必须出现在圆括号之间。*

```sql
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
```
其中， `SELECT * FROM t1 ...` 称为 Outer Query[外查询](或者Outer Statement), `SELECT column1 FROM t2` 称为Sub Query[子查询]。
- 子查询可以包含多个关键字或条件。  
- 外层也可以是各种关键字或条件。
#### 1. 由比较运算符引发的子查询
使用比较运算符的子查询 `=`, `>`, `<`, `>=`, `<=`, `<>`, `!=`, `<=>`.  
语法结构

```sql
operand comparison_operator subquery
```
```sql
SELECT AVG(goods_price) FROM tdb_goods;
SELECT ROUND(AVG(goods_price), 2) FROM tdb_goods;
SELECT goods_id, goods_name, goods_price FROM tdb_goods WHERE goods_price >= 5636.36;
SELECT goods_id, goods_name, goods_price FROM tdb_goods WHERE goods_price >= (SELECT AVG(goods_price) FROM tdb_goods);
SELECT * FROM tdb_goods WHERE goods_cate = '超级本';
SELECT goods_price FROM tdb_goods WHERE goods_cate = '超级本';
SELECT goods_id, goods_name, goods_price FROM tdb_goods WHERE goods_price > (SELECT goods_price from tdb_goods WHERE goods_CATE = '超级本'); //子查询返回多于一行，因此报错
```
**用 `ANY`, `SOME` 或 `ALL` 修饰的比较运算数**

```sql
operand comparison_operator ANY (subquery)  // 符合其中一个
operand comparison_operator SOME (subquery)  // 符合其中一个
operand comparison_operator ALL (subquery)  // 符合其中全部
```
---|ANY|SOME|ALL
---|---|---|---
`>`, `>=`|最小值|最小值|最大值
`<`, `<=`|最大值|最大值|最小值
`=`|任意值|任意值||
`<>`, `!=`|||任意值

```sql
SELECT goods_id, goods_name, goods_price FROM tdb_goods WHERE goods_price > ANY (SELECT goods_price from tdb_goods WHERE goods_CATE = '超级本');
SELECT goods_id, goods_name, goods_price FROM tdb_goods WHERE goods_price > ALL (SELECT goods_price from tdb_goods WHERE goods_CATE = '超级本');
```
#### 2. 由 `[NOT] IN/EXISTS` 引发的子查询
##### 2.1. `IN`
```sql
SELECT goods_id, goods_name, goods_price FROM tdb_goods WHERE goods_price <> ALL (SELECT goods_price from tdb_goods WHERE goods_CATE = '超级本')\G;
```
##### 2.2. `EXISTS`
若子查询返回任意行，`EXISTS` 返回 `TRUE` , 否则返回 `FALSE`

### 使用 `INSERT...SELECT` 插入记录
因为品牌和类型为中文，占空间大。所以另外开辟一个表，做外键。

```sql
CREATE TABLE IF NOT EXISTS tdb_goods_cates(
    cate_id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    cate_name VARCHAR(40) NOT NULL
);
SELECT goods_cate FROM tdb_goods GROUP BY goods_cate;
```
将查询结果写入数据表

```sql
INSERT [INTO] tbl_name [(col_name, ...)] SELECT ...
```
```sql
INSERT tdb_goods_cates(cate_name) SELECT goods_cate FROM tdb_goods GROUP BY goods_cate;
SELECT * FROM tdb_goods_cates;
```
而需要根据 分类表 来更新 商品表，则需要下文中的: 多表更新

### 多表更新
```sql
UPDATE table_references SET col_name1={expr1 | DEFAULT} [, col_name2={expr2 | DEFAULT}]...[WHERE where_condition]
```
```sql
UPDATE tdb_goods INNER JOIN tdb_goods_cates ON goods_cate = cate_name SET goods_cate = cate_id;
SELECT * FROM tdb_goods\G;
```
#### 1. 多表更新之一步到位
之前是 3 步完成。现在希望一步完成。  
**CREATE ... SELECT**
创建数据表的同时，将查询结果写入数据表

```sql
CREATE TABLE [IF NOT EXISTS] tbl_name [(create_definition, ...)] select_statement
```
```sql
SELECT brand_name FROM tdb_goods GROUP BY brand_name;
CREATE TABLE tdb_goods_brands(
    brand_id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    brand_name VARCHAR(40) NOT NULL
)SELECT bRand_name FROM tdb_goods GROUP BY brand_name;
SELECT * FROM tdb_goods_bands;
UPDATE tdb_goods AS g INNER JOIN tdb_goods_brands AS b ON g.brand_name = b.brand_name SET g.brand_name = b.brand_id;
SELECT * FROM tdb_goods\G;
```
```sql
ALTER TABLE tdb_goods CHANGE goods_cate cate_id SMALLINT UNSIGNED NOT NULL, CHANGE brand_name brand_id SMALLINT UNSIGNED NOT NULL;
SHOW COLUMNS FROM tdb_goods;
```
```sql
INSERT tdb_goods_cates(cate_name) VALUES('路由器'),('交换机'),('网卡');
INSERT tdb_goods_brands(brand_name) VALUES('海尔'),('清华同方'),('神舟');
INSERT tdb_goods(goods_name,cate_id,brand_id,goods_price) VALUES(' LaserJet Pro P1606dn 黑白激光打印机','12','4','1849');
```

### 连接的语法结构
```sql
table_reference {[INNER | CROSS] JOIN | {LEFT | RIGHT} [OUTER] JOIN} table_reference ON conditional_expr
```
#### 1. 数据表参照 `table_reference`
```sql
tbl_name [[AS] alias] | table_subquery [AS] alias
```
- 数据表可以使用 `tbl_name AS alias_name` 或 `tbl_name alias_name` 赋予别名  
- `table_subquery` 可以作为子查询使用在 `FROM` 子句中，这样的子查询必须为其赋予别名
#### 2. 内连接 `INNER JOIN`  
显示左表及右表符合链接条件的记录  
> - 在 MySQL 中，`JOIN`, `CROSS JOIN` 和 `INNER JOIN` 是等价的。
> - `LEFT [OUTER] JOIN` 左外连接
> - `RIGHT [OUTER] JOIN` 右外连接  
> 
> **通常使用 `ON` 关键字来设定连接条件; 使用 `WHERE`  关键字来进行结果集记录的过滤。**  
> *ON 和 WHERE 可以等价*
```sql
SELECT goods_id, goods_name, cate_name FROM tdb_goods INNER JOIN tdb_goods_cates ON tdb_goods.cate_id = tdb_goods_cates.cate_id;
```
```sql
```
#### 3. 外连接 `outer join`
- `左外连接`:显示左表的全部记录及右表符合连接条件的记录
- `右外连接`:显示右表……

```sql
SELECT goods_id, goods_name, cate_name FROM tdb_goods LEFT JOIN tdb_goods_cates ON tdb_goods.cate_id = tdb_goods_cates.cate_id;
SELECT goods_id, goods_name, cate_name FROM tdb_goods RIGHT JOIN tdb_goods_cates ON tdb_goods.cate_id = tdb_goods_cates.cate_id;
```
#### 4. 多表链接
```sql
SELECT goods_id, goods_name, cate_name, brand_name, goods_price FROM tdb_goods AS g 
INNER JOIN tdb_goods_cates AS c ON g.cate_id = c.cate_id 
INNER JOIN tdb_goods_brands AS b ON g.brand_id = b.brand_id\G;
```

### 无限级分类表设计
类似购物网站的，图书下面还有子分类的情况
#### 1. 数据注入
```sql
SHOW COLUMNS FROM tdb_goods_cates;
SELECT * FROM tdb_goods_cates;
CREATE TABLE tdb_goods_types(
    type_id   SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    type_name VARCHAR(20) NOT NULL,
    parent_id SMALLINT UNSIGNED NOT NULL DEFAULT 0
); 
INSERT tdb_goods_types(type_name,parent_id) VALUES('家用电器',DEFAULT);
INSERT tdb_goods_types(type_name,parent_id) VALUES('电脑、办公',DEFAULT);
INSERT tdb_goods_types(type_name,parent_id) VALUES('大家电',1);
INSERT tdb_goods_types(type_name,parent_id) VALUES('生活电器',1);
INSERT tdb_goods_types(type_name,parent_id) VALUES('平板电视',3);
INSERT tdb_goods_types(type_name,parent_id) VALUES('空调',3);
INSERT tdb_goods_types(type_name,parent_id) VALUES('电风扇',4);
INSERT tdb_goods_types(type_name,parent_id) VALUES('饮水机',4);
INSERT tdb_goods_types(type_name,parent_id) VALUES('电脑整机',2);
INSERT tdb_goods_types(type_name,parent_id) VALUES('电脑配件',2);
INSERT tdb_goods_types(type_name,parent_id) VALUES('笔记本',9);
INSERT tdb_goods_types(type_name,parent_id) VALUES('超级本',9);
INSERT tdb_goods_types(type_name,parent_id) VALUES('游戏本',9);
INSERT tdb_goods_types(type_name,parent_id) VALUES('CPU',10);
INSERT tdb_goods_types(type_name,parent_id) VALUES('主机',10);
SHOW COLUMNS FROM tdb_goods_types;
SELECT * FROM tdb_goods_types;
```
#### 2. 自身连接
同一个数据表对其自身进行连接。

```sql
SELECT s.type_id, s.type_name, p.type_name FROM tdb_goods_types AS s LEFT JOIN tdb_goods_types AS p ON s.parent_id = p.type_id;
SELECT p.type_id, p.type_name, s.type_name FROM tdb_goods_types AS p LEFT JOIN tdb_goods_types AS p ON s.parent_id = p.type_id;
SELECT p.type_id,p.type_name,count(s.type_name) FROM tdb_goods_types AS p LEFT JOIN tdb_goods_types AS  s ON s.parent_id = p.type_id GROUP BY p.type_name ORDER BY p.type_id;
```

### 多表删除
```sql
DELTET tbl_name[.*] [, tbl_name[.*]] ... FROM table_references [WHERE where_condition]
```
```sql
SELECT goods_id, goods_name FROM tdb_goods GROUP BY goods_name HAVING count(goods_name) >= 2;
DELETE t1 FROM tdb_goods AS t1 LEFT JOIN (SELECT goods_id, goods_name FROM tdb_goods GROUP BY goods_name HAVING count(goods_name)>=2) AS t2 ON t1.goods_name = t2.goods_name WHERE t1.goods_id > t2.goods_id;
SELECT * FROM tdb_goods\G;
```

- - - - -

## 06 运算符和函数

### 字符函数

|   函数名称   |            描述            |
| ----------- | ------------------------- |
| CONCAQT()   | 字符连接                   |
| CONCAT_WS() | 使用指定的分隔符进行字符连接 |
| FORMAT()    | 数字格式化                 |
| LOWER()     | 转换成小写字母              |
| UPPER()     | 转换成大写字母              |
| LEFT()      | 获取左侧字符                |
| RIGHT()     | 获取右侧字符                |
| LENGTH()    | 获取字符串长度              |
| LTRIM()     | 删除前导空格                |
| RTRIM()     | 删除后续空格                |
| TRIM()      | 删除前导和后续空格          |
| SUBSTRING() | 字符串截取                 |
| [NOT] LIKE  | 模式匹配                   |
| REPLACE()   | 字符串替换                 |

```sql
USE imooc;
SELECT CONCAT('imooc', 'MySQL');
SELECT CONCAT('imooc','-','MySQL');
SELECT CONCAT(first_name, last_name) AS fullname FROM test;
SELECT CONCAT_WS('|','imooc','MySQL');
SELECT CONCAT_WS('-','imooc','MySQL','Functions');
SELECT FORMAT(12560.75, 2);
SELECT FORMAT(12560.75, 1);
SELECT FORMAT(12560.75, 0);
SELECT LOWER('MYSQL');
SELECT UPPER('mysql');
SELECT LEFT('MySQL', 2);
SELECT LOWER(LEFT("MySQL", 2));
SELECT RIGHT('MySQL', 3);
SELECT LENGTH('MySQL');
SELECT LTRIM('    MySQL   ');
SELECT LENGTH('    MySQL   ');
SELECT LENGTH(LTRIM('    MySQL   '));
SELECT LENGTH(RTRIM('    MySQL   '));
SELECT LENGTH(TRIM('    MySQL   '));
SELECT TRIM(LEADING '?' FROM '??MySQL???');
SELECT TRIM(TRAILING '?' FROM '??MySQL???');
SELECT TRIM(BOTH '?' FROM '??MySQL???');
SELECT TRIM(BOTH '?' FROM '??My????SQL???');
SELECT REPLACE('??MySQL???', '?', '');
SELECT REPLACE('??MySQL???', '?', '!*');
SELECT SUBSTRING('MySQL', 1, 2);
SELECT SUBSTRING('MySQL', 3);
SELECT SUBSTRING('MySQL', -1);
SELECT SUBSTRING('MySQL', -3, 1);
SELECT 'MySQL' LIKE 'M%';
SELECT * FROM test WHERE first_name LIKE '%O%';
SELECT * FROM test WHERE first_name LIKE '%\%%';
SELECT * FROM test WHERE first_name LIKE '%1%%' ESCAPE '1';
//  % 任意字符  |  _ 任意一个字符
```

### 数值运算符和函数

|    名称    |    描述     |
| ---------- | ----------- |
| CEIL()     | 进一取整     |
| DIV        | 整数除法     |
| FLOOR()    | 舍一取整     |
| MOD        | 取余数(取模) |
| POWER()    | 幂运算       |
| ROUND()    | 四舍五入     |
| TRUNCATE() | 数字截取     |

```sql
SELECT 3+4;
SELECT CEIL(3.01);
SELECT FLOOR(3.99);
SELECT 3/4;
SELECT 3 DIV 4;
SELECT 5 % 3;
SELECT 5 MOD 3;
SELECT POWER(3,3);
SELECT ROUND(3.652, 2);
SELECT ROUND(3.652, 0);
SELECT TRUNCATE(125.89, 1);
SELECT TRUNCATE(125.89, -1);
```

### 比较运算符和函数

|          名称          |       描述        |
| ---------------------- | ----------------- |
| [NOT] BETWEEN...AND... | [不]在范围之内     |
| [NOT] IN()             | [不]在列出值范围内 |
| IS [NOT] NULL          | [不]为空          |

```SQL
SELECT 15 BETWEEN 1 AND 22;
SELECT 35 BETWEEN 1 AND 22;
SELECT 35 NOT BETWEEN 1 AND 22;
SELECT 10 IN(5, 10, 15, 20);
SELECT 13 IN(5, 10, 15, 20);
SELECT NULL IS NULL;
SELECT '' IS NULL;
SELECT 0 IS NULL;
SELECT * FROM test WHERE first_name IS NULL;
SELECT * FROM test WHERE first_name IS NOT NULL;
```

### 日期时间函数

|     名称      |     描述      |
| ------------- | ------------- |
| NOW()         | 当前日期和时间 |
| CURDATE()     | 当前日期      |
| CURTIME()     | 当前时间      |
| DATE_ADD()    | 日期变化      |
| DATEDIFF()    | 日期差值      |
| DATE_FORMAT() | 日期格式化     |

```SQL
SELECT NOW();
SELECT CURDATE();
SELECT CURTIME();
SELECT DATE_ADD('2014-3-12', INTERVAL 365 DAY);
SELECT DATE_ADD('2014-3-12', INTERVAL -365 DAY);
SELECT DATE_ADD('2014-3-12', INTERVAL 1 YEAR
SELECT DATE_ADD('2014-3-12', INTERVAL 3 WEEK);
SELECT DATEDIFF('2013-3-12', '2014-3-12');
SELECT DATE_FORMAT('2014-3-12', '%m/%d/%Y');
```

### 信息函数

|       名称       |       描述        |
| ---------------- | ----------------- |
| CONNECTION_ID()  | 连接ID            |
| DATABASE()       | 当前数据库        |
| LAST_INSERT_ID() | 最后插入记录的ID号 |
| USER()           | 当前用户          |
| VERSION()        | 版本信息          |

```sql
SELECT CONNECTION_ID();
SELECT DATABASE();
SELECT * FROM test;
INSERT test(username) VALUES('liu');
SELECT * FROM test;
SELECT LAST_INSERT_ID();
INSERT test(username) VALUES('hu'),('gu'),('wang');
SELECT * FROM test;
SELECT LAST_INSERT_ID();
SELECT USER();
SELECT VERSION();
```

### 聚合函数

|  名称   |  描述  |
| ------- | ----- |
| AVG()   | 平均值 |
| COUNT() | 计数   |
| MAX()   | 最大值 |
| MIN()   | 最小值 |
| SUM()   | 求和   |

```SQL
SELECT AVG(id) FROM test;
SELECT * FROM tdb_goods LIMIT 1;
SELECT ROUND(AVG(goods_price), 2) AS avg_price FROM tdb_goods;
SELECT COUNT(goods_id) AS counts FROM tdb_goods;
SELECT MAX(goods_price) AS MAX FROM tdb_goods;
SELECT SUM(goods_price) AS SUM FROM tdb_goods;
```

### 加密函数

|    名称    |    描述     |
| ---------- | ----------- |
| MD5()      | 信息摘要算法 |
| PASSWORD() | 密码算法     |

```sql
SELECT MD5('admin');
SELECT PASSWORD('admin');
```
`PASSWORD()`用在更改MySQL用户密码上

```sql
SET PASSWORD=PASSWORD('111111');
EXIT;
mysql -uroot -p111111
```

- - - - -

## 07 自定义函数

### 自定义函数简介
用户自定义函数（user-defined function, UDF）是一种对MySQL扩展的途径，起用法与内置函数相同。  
> 两个条件
> 1. 参数 `< 1024个`
> 2. 返回值
#### 1. 创建自定义函数
```sql
CREATE FUNCTION function_name
RETURNS { STRING | INTEGER | REAL | DECIMAL }
routine_body
```
#### 2. 函数体
1. 函数体由合法的SQL语句构成
2. 函数体可以是简单的SELECT或INSERT语句
3. 函数体如果为符合结构则使用BEGIN...END语句
4. 符合结构可以包含声明，循环，控制结构。

### 创建不带参数的自定义函数
```sql
SELECT NOW();
SELECT DATE_FORMAT(NOW(), '%Y年%m月%d日 %H点:%i分:%s秒');
USE test;
CREATE FUNCTION f1()
RETURNS VARCHAR(30)
RETURN DATE_FORMAT(NOW(), '%Y年%m月%d日 %H点:%i分:%s秒');
SELECT f1();
```

### 创建带有参数的自定义函数
```SQL
CREATE FUNCTION f2(num1 SMALLINT UNSIGNED, num2 SMALLINT UNSIGNED)
RETURNS FLOAT(10, 2) UNSIGNED
RETURN (num1+num2)/2;
//删除函数的方式：DROP FUNCTION f2;
SELECT f2(10, 15);
```

### 创建具有复合结构函数体的自定义函数
```SQL
SHOW TABLES;
SELECT * FROM test;
DELIMITER //
CREATE FUNCTION adduser(username VARCHAR(20))
RETURNS INT UNSIGNED
BEGIN
INSERT test(username) VALUES(username);
RETURN LAST_INSERT_ID();
END
//
SELECT adduser('Rose');
DELIMITER ;
SELECT adduser('Tom');
SELECT * FROM test;
```

- - - - -

## 08 MySQL存储过程

### 存储过程简介
```graphLR
A[SQL命令] --> B(MySQL引擎)
B --> |分析|C{语法正确}
C -->D(可执行命令)
D -->|执行|E(执行结果)
E -->|返回|F[客户端]
```
存储过程是SQL语句和控制语句预编译集合，以一个名称存储并作为一个单元处理。  
效率要比单一的SQL执行效率更高。  
#### 1. 优点
- 增强SQL语句的功能和灵活性
- 实现较快的执行速度
- 减少网络流量

### 存储过程语法结构解析
```SQL
CREATE [DEFINER = {user | CURENT_USER}]
PROCEDURE sp_name ([proc_parameter[,...]])
[characteristic ...] routine_body
```
**proc_parameter**

```sql
[IN | OUT | INOUT] param_name type
```
- `IN` 表示该参数的值必须在调用存储过程时指定
- `OUT` 表示该参数的值可以被存储过程改变，并且可以返回
- `INOUT` 表示该参数的值在调用时指定，并且可以改变和返回
#### 1. 特性
```SQL
COMMENT 'string'
| { CONTAIN SQL | NO SQL | READS SQL DATE | MODIFIES SQL DATA }
| SQL SECURITY {DEFINER | INVOKER}
```
- `COMMENT` 注释
- `CONTAINS SQL` 包含SQL语句，但不包含读或写数据的语句
- `NO SQL` 不包含SQL语句
- `READS SQL DATA` 包含读数据的语句
- `MODIFIES SQL DATA` 包含写数据的语句
- `SQL SECURITY {DEFINER | INVOKER}` 指明谁有权限来执行 
#### 2. 过程体
- 过程体由合法的SQL语句构成
- 过程体可以是任意SQL语句
- 过程体如果为复合结构则使用BEGIN... END语句
- 符合结构可以包含声明，循环，控制结构

### 创建不带参数的存储过程
```sql
CREATE PROCEDURE sp1() SELECT VERSION();
```
#### 1. 调用存储过程
```sql
CALL sp_name([parameter[, ...]])
CALL sp_name[()]
```
```sql
CALL sp1;
CALL sp1();
```

### 创建带有IN类型参数的存储过程
参数名字不能和数据表中记录相同

```sql
DELIMITER //
CREATE PROCEDURE removeUserById(IN p_id INT UNSIGNED)
BEGIN
DELETE FROM users WHERE id = p_id;
END
//
DELIMITER ;
SELECT * FROM users;
CALL removeUserById(3);
SELECT * FROM users;
```
#### 1. 修改存储过程
只能修改注释，内容类型等，不能修改过程体。

```sql
ALTER PROCEDURE sp_name [characteristic ...]
COMMENT 'string'
| { CONTAIN SQL | NO SQL | READS SQL DATE | MODIFIES SQL DATA }
| SQL SECURITY {DEFINER | INVOKER}
```
**存储体出现问题时，只能删除后新建。**  
#### 2. 删除存储过程

```sql
DROP PROCEDURE removeUserById;
```

### 创建带有IN和OUT类型参数的存储过程
```sql
DELIMITER //
CREATE PROCEDURE removeUserAndReturnUserNums(IN p_id  INT UNSIGNED, OUT userNums INT UNSIGNED)
BEGIN
DELETE FROM users WHERE id = p_id;
SELECT COUNT(id) FROM users INTO userNums;
END
//
DELIMITER ;
CALL removeUserAndReturnUserNums(7, @nums);
SELECT @nums;
```
#### 1. 变量分类
1. 用户变量：以"@"开始，形式为"@变量名"
> 用户变量跟mysql客户端是绑定的，设置的变量，只对当前用户使用的客户端生效
> 
```sql
SET @i = 7;
SELECT @i;
```
2. 全局变量：定义时，以如下两种形式出现，`SET GLOBAL 变量名` 或者 `SET @@global.变量名` 
> 对所有客户端生效。只有具有super权限才可以设置全局变量
3. 会话变量：只对连接的客户端有效。
4. 局部变量：作用范围在`BEGIN ... END`语句块之间。在该语句块里设置的变量
> `DECLARE` 语句专门用于定义局部变量。`SET`语句是设置不同类型的变量，包括会话变量和全局变量

### 创建带有多个OUT类型参数的存储过程
```sql
SELECT ROW_COUNT();
INSERT test(username) VALUES('A'),('B'),('C');
SELECT ROW_COUNT();
UPDATE test SET username = CONCAT(username, '--imooc') WHERE id <= 2;
SELECT ROW_COUNT();
SELECT * FROM test;
```
```sql
DELIMITER //
CREATE PROCEDURE removeUserByAgeAndReturnInfos(IN p_age SMALLINT UNSIGNED, OUT deleteUsers SMALLINT UNSIGNED, OUT userCounts SMALLINT UNSIGNED)
BEGIN
DELETE FROM users WHERE age = p_age;
SELECT ROW_COUNT() INTO deleteUsers;
SELECT COUNT(id) FROM users INTO userCounts;
END
//
DELIMITER ;
SELECT * FROM users;
SELECT COUNT(id) FROM users;
SELECT COUNT(id) FROM users WHERE age = 20;
CALL removeUserByAgeAndReturnInfos(20, @a, @b);
SELECT @a, @b;
SELECT * FROM users;
```

### 存储过程与自定义函数的区别

|       存储过程        |                    自定义函数                    |
| -------------------- | ----------------------------------------------- |
| 实现的功能要更复杂一点 | 更具针对性                                       |
| 可以返回多个值        | 只能有一个返回值                                 |
| 一般独立执行          | 可以作为其他SQL语句的组成部分出现，经常对表进行操作 |

### 数据准备
```sql
DELETE FROM users WHERE 1;
INSERT users(username,password,age,sex) VALUES('A',md5('A'),20,0);
INSERT users(username,password,age,sex) VALUES('B',md5('B'),23,1);
INSERT users(username,password,age,sex) VALUES('C',md5('C'),23,1);
INSERT users(username,password,age,sex) VALUES('D',md5('D'),24,1);
INSERT users(username,password,age,sex) VALUES('E',md5('E'),24,0);
INSERT users(username,password,age,sex) VALUES('F',md5('F'),23,0);
INSERT users(username,password,age,sex) VALUES('G',md5('G'),22,0);
INSERT users(username,password,age,sex) VALUES('H',md5('H'),23,0);
INSERT users(username,password,age,sex) VALUES('I',md5('I'),23,0);
INSERT users(username,password,age,sex) VALUES('J',md5('J'),22,1);
INSERT users(username,password,age,sex) VALUES('K',md5('K'),22,1);
INSERT users(username,password,age,sex) VALUES('L',md5('L'),22,0);
INSERT users(username,password,age,sex) VALUES('M',md5('M'),24,1);
INSERT users(username,password,age,sex) VALUES('N',md5('N'),21,0);
INSERT users(username,password,age,sex) VALUES('O',md5('O'),20,0);
INSERT users(username,password,age,sex) VALUES('P',md5('P'),20,1);
INSERT users(username,password,age,sex) VALUES('Q',md5('Q'),24,1);
INSERT users(username,password,age,sex) VALUES('R',md5('R'),24,1);
```

- - - - -

## 09 MySQL存储引擎

### 存储引擎简介
查看数据表的创建命令

```sql
USE test;
SHOW TABLES;
SHOW CREATE TABLE test;
```
#### 1. 存储引擎
- MyISAM
- InnoDB
- Memory
- CSV
- Archive

### 相关知识点之并发处理
#### 1. 并发控制
当多个连接对记录进行修改时保证数据的一致性和完整性
#### 2. 锁
- **共享锁（读锁）**：在同一时间内，多个用户可以读取同一个资源，读取过程中数据不会发生任何变化
- **排它锁（写锁）**：在任何时候，只能有一个用户写入资源，当进行写锁时会阻塞其他的读锁或者写锁操作。
#### 3. 锁颗粒
- **表锁**：一种开销最小的锁策略
- **行锁**：一种开销最大的锁策略

### 相关知识点之事务处理
#### 1. 事务
事务用于保证数据库的完整性。将几个过程看作一个完整的事务
#### 2. 事务的特性
- 原子性
- 一致性
- 隔离性
- 持久性

### 相关知识点之外键和索引
#### 1. 索引
对数据表中一列或多列的值进行排序的一种结构

### 各个存储引擎特点

|   特点   | MyISAM | InnoDB | Memory | Archive |
| ------- | ------ | ------ | ------ | ------- |
| 存储限制 | 256TB  | 64TB   | 有     | 无      |
| 事务安全 | -      | 支持   | -      | -       |
| 支持索引 | 支持   | 支持   | 支持   | -       |
| 锁颗粒   | 表锁   | 行锁   | 表锁   | 行锁    |
| 数据压缩 | 支持   | -      | -      | 支持    |
| 支持外键 | -      | 支持   | -      | -       |

#### 1. 索引类型
- 普通索引
- 唯一索引
- 全文索引
- btree索引
- hash索引

### 设置存储引擎
1. 通过修改MySQL配置文件实现
```sql
default-storage-engine = engine
```
2. 通过创建数据表命令实现
```sql
CREATE TABLE tp1(
    s1 VARCHAR(10)
) ENGINE = MyISAM;
SHOW CREATE TABLE tp1;
```
3. 通过修改数据表命令实现
```sql
ALTER TABLE table_name ENGINE [=] engine_name;
```
```sql
ALTER TABLE tp1 ENGINE InnoDB;
SHOW CREATE TABLE tp1;
```

- - - - -

## 10 MySQL图形化管理工具

### phpMyAdmin
官网下载后解压到Apache目录即可

### Navicat for MySQL

### MySQL Workbench


- - - - -

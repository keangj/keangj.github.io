---
title: SQL
tags:
  - SQL
  - MYSQL
categories:
  - - SQL
  - - MYSQL
cover: 'https://unpkg.com/keangj-assets@1.0.7/img/sql.jpg'
abbrlink: 29900
date: 2024-06-28 22:17:14
updated: 2024-06-28 22:17:14
---

# SQL

---

## MYSQL

### 数据库基础

- **数据库（database）** 保存有组织的数据的容器（通常是一个文件或一组文件）

- **表（table）** 某种特定类型数据的结构化清单

- **列（column）** 表中的一个字段。所有表都是由一个或多个列组成的

- **行（row）** 表中的一个记录

- **字段（field）**  基本上与列（column）的意思相同，经常互换使用，不过数据库列一般称为列，而术语字段通常用在计算字段的连接上。

- **模式（schema）** 关于数据库和表的布局及特性的信息

- **数据类型（datatype）** 所容许的数据的类型。每个表列都有相应的数据类型，它限制（或容许）该列中存储的数据

- **主键（primary key）**一一列（或一组列），其值能够唯一区分表中每个行

  主键用来表示 一个特定的行。没有主键，更新或删除表中特定行很困难

  表中的任何列都可以作为主键，只要它满足以下条件：

  1. 任意两行都不具有相同的主键值
  2. 每个行都必须具有一个主键值（主键列不允许NULL值）

  主键的最好习惯

  1. 不更新主键列中的值
  2. 不重用主键列的值
  3. 不在主键列中使用可能会更改的值
  
- **外键（foreign key）** 外键为某个表中的一列，它包含另一个表 的主键值，定义了两个表之间的关系。

- **子句（clause）** SQL语句由子句构成，有些子句是必需的，而有的是可选的。一个子句通常由一个关键字和所提供的数据组成。子句的例子有 `SELECT` 语句的 `FROM` 子句。

- **查询（query）** 任何 SQL 语句都是查询。但此术语一般指 SELECT 语句。

- **操作符（operator）** 用来联结或改变 `WHERE` 子句中的子句的关键 字。也称为逻辑操作符（logical operator）。



**Mysql 登录**

``` sql
mysql -u <user> -p -h <host> -P <port>;
```

**Mysql 退出**

`quit` or `exit`

结束 SQL 语句，多条 SQL 语句必须以分号（`；`）分隔。

SQL 语句和大小写许多 SQL 开发人员喜欢关键字使用大写，而对所有列和表名使用小写，这样做使代码更易于阅读和调试。

使用空格在处理 SQL 语句时，其中所有空格都被忽略。SQL 语句可以在一行上给出，也可以分成许多行。



### 使用数据库

- 打开数据库 `USE <database>;`
- 查看可用数据库 `SHOW DATABASES;`
- 查看当前数据库表 `SHOW TABLES;`
- 查看表的列 `SHOW COLUMNS FROM <table>;`
- 显示广泛的服务器状态信息 `SHOW STATUS;`
- 显示创建特定数据库或表的MySQL语句 `SHOW CREATE DATABASE;` 和 `SHOW CREATE TABLE;`
- 显示授予用户的安全权限 `SHOW GRANTS;`
- 显示服务器错误或警告消息 `SHOW ERRORS;` 和 `SHOW WARNINGS;`

### 检索数据

- **检索单列** 

  ``` sql
  SELECT <column> FROM <table>;
  ```

- **检索多列**

  ``` sql
  SELECT <column1>, <column2>, <column3> FROM <table>;
  ```

- **检索所有列**  

  ``` sql
  SELECT * FROM <table>;
  ```

  一般，除非你确实需要表中的每个列，否则最 好别使用`*`通配符。

- **检索不同的行** 

  ``` sql
  SELECT DISTINCT <column> FROM <table>;
  ```

  `DISTINCT`关键字，表示只返回不同的值，`DISTINCT` 关键字应用于所有列而 不仅是前置它的列。

- **限制结果**

  ``` sql
  SELECT <column> FROM <table> LIMIT <row_start, row_count>;
  ```

  `LIMIT 3, 4` 指示MySQL返回从行3开始的4行。第一个数为开始位置，第二个数为要检索的行数。

  `LIMIT 4 OFFSET 3` 意为从行3开始取4行，就像 `LIMIT 3, 4` 一样。

  检索出来的第一行为行 0 而不是行 1。因此，`LIMIT 1, 1` 将检索出第二行而不是第一行。

  LIMIT 中指定要检索的行数为检索的最大行数。如果没有足够的行，只返回它能返回的那么多行。

- **使用完全限定的表名**

  ``` sql
  SELECT <table>.<column> FROM <database>.<table>;
  ```

### 排序检索数据

- **排序数据 **

  ``` sql
  SELECT <column> FROM <table> ORDER BY <column>;
  ```

  `ORDER BY` 子句取一个或多个列的名字，据此对输出进行排序。

- **多列排序**

  ``` sql
  SELECT <column1> <column2> <column3> FROM <table> ORDER BY <column1>, <column2>;
  ```

  多个列排序时，排序完全按所规定的顺序进行。

- **指定排序方向**

  ``` sql
  SELECT <column> FROM <table> ORDER BY <column> DESC;
  SELECT <column1> <column2> <column3> FROM <table> ORDER BY <column1> DESC, <column2>;
  ```

  `DESC` 关键字只应用到直接位于其前面的列名。

  如果想在多个列上进行降序排序，必须对每个列指定 `DESC` 关键字。

  `DESC` 降序，`ASC` 升序，默认排序为 `ASC`。

### 过滤数据

**`WHERE` 子句**

`WHERE` 子句的位置，在同时使用 `ORDER BY` 和 `WHERE` 子句时，应该让ORDER BY位于WHERE之后

**`WHERE` 子句操作符**

| 操作符  | 说明               |
| ------- | ------------------ |
| =       | 等于               |
| <>      | 不等于             |
| !=      | 不等于             |
| <       | 小于               |
| <=      | 小于等于           |
| >       | 大于               |
| >=      | 大于等于           |
| BETWEEN | 在指定的两个值之间 |



- **检查单个值**

  ``` sql
  SELECT <column1>, <column2> FROM <table> WHERE <column1> = 'demo';
  ```

  返回 `<column1>` 等于 `demo` 的记录

  ``` sql
  SELECT <column1>, <column2> FROM <table> WHERE <column2> < 10;
  ```

  返回 `<column2>` 小于 10 的记录

- **不匹配检查**

  ``` sql
  SELECT <column1>, <column2> FROM <table> WHERE <column1> <> 'demo';
  ```

  返回 `<column1>` 不等于 `demo` 的记录

  ``` sql
  SELECT <column1>, <column2> FROM <table> WHERE <column2> != 10;
  ```

  返回 `<column2>` 不等于 10 的记录

- **范围值检查**

  `BETWEEN` 操作符需要两个值，即范围的开始值和结束值。这两个值必须用 `AND` 关键字分隔。`BETWEEN` 匹配范围中所有的值，包括指定的开始值和结束值。

  ``` sql
  SELECT <column1>, <column2> FROM <table> WHERE <column2> <> BETWEEN 5 AND 10;
  ```

  返回 `<column2>` 在 5 和 10 之间的记录

- **空值检查**

  `IS NULL` 子句，可用来检查具有NULL值的列。

  `NULL` 无值（no value），它与字段包含0、空字符串或仅仅包含空格不同。

  ``` sql
  SELECT <column> FROM <table> WHERE <column> IS NULL;
  ```

  返回 `<column>` 为空值的记录



### 数据过滤

多个 `WHERE`子句，可以两种方式使用：以 `AND` 子句的方式或 `OR` 子句的方式使用。

**`AND` 用在 `WHERE` 子句中的关键字，用来指示检索满足所有给定条件的行。**

可以添加多个过滤条件，每添加一条就要使用一个 `AND`。

**`OR` WHERE 子句中使用的关键字，用来表示检索匹配任一给定条件的行。**

**`WHERE` 可包含任意数目的 `AND` 和 `OR` 操作符。**

- **多列过滤**，使用 `AND` 操作符给WHERE子句附加条件。

  ``` sql
  SELECT <column1>, <column2> FROM <table> WHERE <column1> = 'demo' AND <column2> < 10;
  ```

  返回 `<column1>` 等于 `demo` 且 `<column2>` 小于 10 的记录

- **匹配任一条件**

  ``` sql
  SELECT <column1>, <column2> FROM <table> WHERE <column1> = 'demo' OR <column1> = 'test';
  ```

  返回 `<column1>` 等于 `demo` 或者等于 `test` 的记录。

计算次序：

在处理 `OR` 操作符前，优先处理 `AND` 操作符。

使用圆括号 `()` 明确地分组相应的操作符，圆括号具有较 `AND` 或 `OR` 操作符高的计算次序。

任何时候使用具有 `AND` 和 `OR` 操作 符的 `WHERE` 子句，都应该使用圆括号明确地分组操作符。

``` sql
SELECT <column1>, <column2> FROM <table> WHERE (<column> = 'demo' OR <column> = 'demo') AND <column> < 10;
```



**`IN`  `WHERE` 子句中用来指定要匹配值的清单的关键字，功能与 `OR`  相当。**

`IN` 操作符用来指定条件范围，范围中的每个条件都可以进行匹配。`IN` 取合法值的由逗号分隔的清单，全都括在圆括号中。

``` sql
SELECT <column1>, <column2> FROM <table> WHERE <column1> IN ('demo', 'test');
```

返回 `<column1>` 包含 `'demo', 'test'` 的记录。

等同于

``` sql
SELECT <column1>, <column2> FROM <table> WHERE <column1> = 'demo' OR <column1> = 'test';
```



**`NOT` `WHERE` 子句中用来否定后跟条件的关键字。**

``` sql
SELECT <column1>, <column2> FROM <table> WHERE <column1> NOT IN ('demo', 'test');
```

返回 `<column1>` 不包含 `'demo', 'test'` 的记录

MySQL 支持使用 `NOT` 对 `IN`、`BETWEEN` 和 `EXISTS` 子句取反。



### 用通配符进行过滤

通配符（wildcard） 用来匹配值的一部分的特殊字符。

搜索模式（search pattern）由字面值、通配符或两者组合构成的搜索条件。

在搜索子句中使用通配符，必须使用 `LIKE` 操作符。从技术上说，LIKE 是谓词而不是操作符。

- **百分号（`%`）通配符** `%` 表示任何字符出现任意次数。

  ``` sql
  SELECT <column> FROM <table> WHERE <column> LIKE 'example%';
  ```

  检索任意以 example 起头的词。% 告诉 MySQL 接受 jet 之后的任意字符，不管它有多少字符。

  通配符可在搜索模式中任意位置使用，并且可以使用多个通配符。

  ``` sql
  SELECT <column> FROM <table> WHERE <column> LIKE '%example%';
  ```

  搜索模式 `'%example%'` 表示匹配任何位置包含文本 example 的值，而不论它之前或之后出现什么字符。

  通配符也可以出现在搜索模式的中间

  ``` sql
  SELECT <column> FROM <table> WHERE <column> LIKE 'e%e';
  ```

  匹配以 e 起头以 e 结尾的记录

  尾空格可能会干扰通配符匹配。解决这个问题的一个简单的办法是在搜索模式最后附加一个%。

- **下划线（`_`）通配符** 下划线只匹配单个字符。

  ``` sql
  SELECT <column> FROM <table> WHERE <column> LIKE 'example _';
  ```

  匹配 example 1 example 2 等字符

**使用通配符的技巧**

1. 不要过度使用通配符。如果其他操作符能达到相同的目的，应该使用其他操作符。通配符搜索的处理一般要比前面讨论的其他搜索所花时间更长。
2. 在确实需要使用通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处。把通配符置于搜索模式的开始处，搜索起来是最慢的。

3. 仔细注意通配符的位置。如果放错地方，可能不会返回想要的数据。



### 正则表达式

`LIKE` 和 `REGEXP` 之间有一个重要的差别。`LIKE` 匹配整个列。如果被匹配的文本在列值中出现，`LIKE` 将不会找到它，相应的行也不被返回（除非使用通配符）。而 `REGEXP` 在列值内进行匹配，如果被匹配的文本在列值中出现，`REGEXP` 将会找到它，相应的行将被返回。

如需要 `REGEXP` 匹配整个列值，使用 `^` 和 `$` 定位符（anchor）即可。

匹配不区分大小写。为区分大小写，可使用 BINARY 关键字， 如 `WHERE prod_name REGEXP BINARY 'Example .000'`。

- **基本字符匹配**

  ``` sql
  SELECT <column> FROM <table> WHERE <column> REGEXP '.000';
  ```

  `'.000'` 匹配 1000 2000 `.` 是正则表达式语言中一个特殊的字符。它表示匹配任意一个字符。

- **`OR` 匹配**

  ``` sql
  SELECT <column> FROM <table> WHERE <column> REGEXP '1000|2000';
  ```

  `'1000|2000'` 。`|` 为正则表达式的 `OR` 操作符。它表示匹配其中之一。

- **匹配几个字符之一**

  ``` sql
  SELECT <column> FROM <table> WHERE <column> REGEXP 'example[123]';
  ```

  `'example[123]'` 。`[123]` 定义一组字符，它的意思是匹配 1 或 2 或 3。

  `[]` 是另一种形式的 `OR` 语句。正则表达式 `[123]` 为 `[1|2|3]` 的缩写，也可以使用后者。

- **匹配范围**

  ``` sql
  SELECT <column> FROM <table> WHERE <column> REGEXP 'example[1-3]';
  ```

  使用 `-` 来定义一个范围。范围不限于完整的集合，`[1-3]` 和 `[6-9]` 也是合法的范围。此外，范围不一定只是数值的，`[a-z]` 匹配任意字母字符。

- **匹配特殊字符**

  ``` sql
  SELECT <column> FROM <table> WHERE <column> REGEXP '\\.';
  ```

  正则表达式语言由具有特定含义的特殊字符构成。匹配 `.`  `[]` `|` 和 `-` 等特殊字符，必须用 `\\`为前导。`\\-` 表示查找 `-`，`\\.` 表示查找 `.`。

  为了匹配反斜杠（`\`）字符本身，需要使用 `\\\`。

  `\\` 也用来引用元字符（具有特殊含义的字符）

  | 元字符 | 说明     |
  | ------ | -------- |
  | `\\f`  | 换页     |
  | `\\n`  | 换行     |
  | `\\r`  | 回车     |
  | `\\t`  | 制表     |
  | `\\v`  | 纵向制表 |

- **匹配多个实例**

  | 重复元字符 | 说明                            |
  | ---------- | ------------------------------- |
  | `*`        | 0 个或多个匹配                  |
  | `+`        | 1 个或多个匹配（等于 `{1,}`）   |
  | `?`        | 0 个或 1 个匹配（等于 `{0,1}`） |
  | `{n}`      | 指定数目的匹配                  |
  | `{n,}`     | 不少于指定数目的匹配            |
  | `{n,m}`    | 匹配数目的范围（m 不超过 255）  |

  ``` sql
  SELECT <column> FROM <table> WHERE <column> REGEXP '\\([0-9] xyz?\\)';
  ```

  `\\(` 匹配 `)`，`[0-9]` 匹配任意数字，`xyz?` 匹配 `xy` 和 `xyz`（`z` 后的 `?` 使 `z` 可选，因为 `?` 匹配它前面的任何字符的 0 次或 1 次出现），`\\)` 匹配 `)`。

  ``` sql
  SELECT <column> FROM <table> WHERE <column> REGEXP '[[:digit:]]{4}';
  ```

  `[:digit:]` 匹配任意数字，因而它为数字的一个集合。`{4}` 确切地要求它前面的字符（任意数字）出现 4 次，所以 `[[:digit:]]{4}` 匹配连在一起的任意 4 位数字。也可以是 `[0-9][0-9][0-9][0-9]`。

- **定位符** 为了匹配特定位置的文本需要使用定位符

  | 定位符    | 说明       |
  | --------- | ---------- |
  | `^`       | 文本的开始 |
  | `$`       | 文本的结尾 |
  | `[[:<:]]` | 词的开始   |
  | `[[:>:]]` | 词的结尾   |

  ``` sql
  SELECT <column> FROM <table> WHERE <column> REGEXP '^[0-9\\.]';
  ```

  匹配以数字（包括以小数点开始的数）开始的记录。

  `^` 有两种用法。在集合中（用 `[` 和 `]` 定义），用它来否定该集合，否则，用来指串的开始处。

**简单的正则表达式测试**

可以在不使用数据库表的情况下用 `SELECT` 来测试正则表达式。`REGEXP` 检查总是返回 0（没有匹配）或 1（匹配）。可以用带文字串的 `REGEXP` 来测试表达式，并试验它们。

``` sql
SELECT 'hello' REGEXP '[0-9]';
```

这个例子显然将返回 0（因为文本 hello 中没有数字）。



### 创建计算字段

- **拼接字段**

  **拼接（concatenate）** 将值联结到一起构成单个值。

  两个列拼接起来。在 `SELECT` 语句中，可使用 `Concat()` 函数来拼接两个列。

  `Concat()` 需要一个或多个指定的串，各个串之间用逗号分隔。

  ``` sql
  SELECT Concat(<column1>, '(', <column2>, ')') FROM <table> ORDER BY <column1>;
  ```

  返回 column1Field (column2Field)

- **使用别名** 

  **别名（alias）** 是一个字段或值的替换名。别名用 `AS` 关键字赋予。别名有时也称为**导出列（derived column）**

  常见的用途包括在实际的表列名包含不符合规定的字符（如空格）时重新命名它，在原来的名字含混或容易误解时扩充它，等等。

  ``` sql
  SELECT Concat(<column1>, '(', <column2>, ')') AS title FROM <table> ORDER BY <column1>;
  ```

  用 `AS` 为计算字段 `Concat(<column1>, '(', <column2>, ')')` 添加 `title` 的列名。

- **执行算术计算**

  ``` sql
  SELECT <column1>, <column2>, <column1>*<column2> AS result FROM <table> ORDER BY <column>;
  ```

  `result` 列为一个计算字段， 此计算为 `<column1>*<column2>`。

  | 算术操作符 | 说明 |
  | ---------- | ---- |
  | +          | 加   |
  | -          | 减   |
  | *          | 乘   |
  | /          | 除   |

  此外，圆括号可用来区分优先顺序。



### 使用数据处理函数

- **文本处理函数**

  ``` sql
  SELECT Concat(RTrim(<column1>), '(', Trim(<column2>), ')') FROM <table> ORDER BY <column1>;
  ```

  `RTrim()` 删除数据右边多余的空格，`LTrim()` 去掉串左边的空格，`Trim()` 去掉串左右两边的空格

  ``` sql
  SELECT <column1>, Upper(<column2>) AS result_upcase FROM <table> ORDER BY <column>;
  ```

  `Upper()` 将文本转换为大写。

  | 文本处理函数  | 说明                 |
  | ------------- | -------------------- |
  | `Left()`      | 返回串左边的字符     |
  | `Right()`     | 返回串右边的字符     |
  | `Upper()`     | 将串转换为大写       |
  | `Lower()`     | 将串转换为小写       |
  | `LTrim()`     | 去掉串左边的空格     |
  | `RTrim()`     | 去掉串左边的空格     |
  | `Trim()`      | 去掉串左右两边的空格 |
  | `Length()`    | 返回串的长度         |
  | `Locate()`    | 找出串的一个子串     |
  | `SubString()` | 返回子串的字符       |
  | `Soundex()`   | 返回串的 SOUNDEX 值  |

- **日期和时间处理函数**

  首选的日期格式为 `yyyy-mm-dd`。因此，1970 年 1 月 1 日，给出为 1970-01-01。

  | 日期和时间处理函数 | 说明                           |
  | ------------------ | ------------------------------ |
  | `AddDate()`        | 增加一个日期（天、周等）       |
  | `AddTime()`        | 增加一个时间（时、分等）       |
  | `CurDate()`        | 返回当前日期                   |
  | `CurTime()`        | 返回当前时间                   |
  | `Date()`           | 返回日期时间的日期部分         |
  | `DateDiff()`       | 计算两个日期之差               |
  | `Date_Add()`       | 高度灵活的日期运算函数         |
  | `Date_Format()`    | 返回一个格式化的日期或时间串   |
  | `Day()`            | 返回一个日期的天数部分         |
  | `DayOfWeek()`      | 对于一个日期，返回对应的星期几 |
  | `Hour()`           | 返回一个时间的小时部分         |
  | `Minute()`         | 返回一个时间的分钟部分         |
  | `Month()`          | 返回一个日期的月份部分         |
  | `Now()`            | 返回当前日期和时间             |
  | `Second()`         | 返回一个时间的秒部分           |
  | `Time()`           | 返回一个日期时间的时间部分     |
  | `Year()`           | 返回一个日期的年份部分         |

  ``` sql
  SELECT <column> FROM <table> WHERE Date(<column_date>) = '1970-01-01';
  ```

  检索日期为 1970-01-01 的记录

  ``` sql
  SELECT <column> FROM <table> WHERE Date(<column_date>) BETEEN '1970-01-01' AND '1970-01-31';
  SELECT <column> FROM <table> WHERE Year(<column_date>) = 1970 AND Month(<column_date>) = 1;
  ```

  检索 1970-01 月的记录

- **数值处理函数**

  | 数值处理函数 | 说明               |
  | ------------ | ------------------ |
  | `Abs()`      | 返回一个数的绝对值 |
  | `Cos()`      | 返回一个角度的余弦 |
  | `Exp()`      | 返回一个数的指数值 |
  | `Mod()`      | 返回除操作的余数   |
  | `Pi()`       | 返回圆周率         |
  | `Rand()`     | 返回一个随机数     |
  | `Sin()`      | 返回一个角度的正弦 |
  | `Sqrt()`     | 返回一个数的平方根 |
  | `Tan()`      | 返回一个角度的正切 |



### 汇总数据

- **聚集函数（aggregate function）** 运行在行组上，计算和返回单个值的函数。

  | 聚集函数  | 说明             |
  | --------- | ---------------- |
  | `AVG()`   | 返回某列的平均值 |
  | `COUNT()` | 返回某列的行数   |
  | `MAX()`   | 返回某列的最大值 |
  | `MIN()`   | 返回某列的最小值 |
  | `SUM()`   | 返回某列值之和   |

  - **`AVG()`** 通过对表中行数计数并计算特定列值之和，求得该列的平均值。`AVG()` 可用来返回所有列的平均值，也可以用来返回特定列或行的平均值。

    `AVG()` 只能用来确定特定数值列的平均值，而且列名必须作为函数参数给出。为了获得多个列的平均值， 必须使用多个 `AVG()` 函数。

    `AVG()` 函数忽略列值为 `NULL` 的行。

    ``` sql
    SELECT AVG(<column>) AS avg_value FROM <table>;
    ```

    获取表中 <column> 列的平均值，并为它添加别名 avg_value。

    ``` sql
    SELECT AVG(<column>) AS avg_value FROM <table> WHERE <column1> = 123;
    ```

    用来确定特定列(`<column1> = 123`)或行的平均值。

  - **`COUNT()`** 函数进行计数。可利用 `COUNT()` 确定表中行的数目或符合特定条件的行的数目。

    `COUNT()` 函数有两种使用方式

    1. 使用 `COUNT(*)` 对表中行的数目进行计数，不管表列中包含的是空值（`NULL`）还是非空值。
    2. 使用 `COUNT(<column>)` 对特定列中具有值的行进行计数， 忽略 `NULL` 值。

    如果指定列名，则指定列的值为空的行被 COUNT() 函数忽略，但如果 COUNT() 函数中用的是星号（*），则不忽略。

    ``` sql
    SELECT COUNT(*) AS num_value FROM <table>;
    ```

    对表中所有行计数。计数值在 num_value 中返回。

    ``` sql
    SELECT COUNT(<column>) AS num_value FROM <table>;
    ```

    对 <column> 列中有值的行进行计数。

  - **`MAX()`** 返回指定列中的最大值。`MAX()` 要求指定列名。

    虽然 `MAX()` 一般用来找出最大的数值或日期值，但MySQL允许将它用来返回任意列中的最大值，包括返回文本列中的最大值。在用于文本数据时，如果数据按相应的列排序，则 `MAX()` 返回最后一行。

    `MAX()` 函数忽略列值为 `NULL` 的行。

    ``` sql
    SELECT MAX(<column>) AS max_value FROM <table>;
    ```

    返回 <column> 列中的最大值。

  - **`MIN()`** 的功能正好与 `MAX()` 功能相反，它返回指定列的最小值。与 `MAX()` 一样，`MIN()` 要求指定列名。

    `MIN()` 函数与 `MAX()` 函数类似， MySQL允许将它用来返回任意列中的最小值，包括返回文本列中的最小值。在用于文本数据时，如果数据按相应的列排序， 则 `MIN()` 返回最前面的行。

    `MIN()` 函数忽略列值为 `NULL` 的行。

    ``` sql
    SELECT MAX(<column>) AS min_value FROM <table>;
    ```

    返回 <column> 列中的最小值。

  - **`SUM()`** 用来返回指定列值的和（总计）。

    **在多个列上进行计算**，利用标准的算术操作符，所有聚集函数都可用来执行多个列上的计算。

    `SUM()` 函数忽略列值为 `NULL` 的行。

    ``` sql
    SELECT SUM(<column1>) AS total_value FROM <table> WHERE <column> = 1;
    ```

    返回 <column> 等于 1 的 <column1> 之和。

    ``` sql
    SELECT SUM(<column1>*<column2>) AS total_value FROM <table> WHERE <column> = 1;
    ```

    返回 <column> 等于 1 的 <column1> 乘以 <column2> 之和。

- **聚集不同值**

  以上 5 个聚集函数都可以如下使用：

  1. 所有的行执行计算，指定 `ALL` 参数或不给参数（因为 `ALL` 是默认行为）。
  2. 包含不同的值，指定 `DISTINCT` 参数。

  ``` sql
  SELECT AVG(DISTINCT <column1>) AS avg_value FROM <table> WHERE <column> = 1;
  ```

  使用了 `DISTINCT` 参数，因此平均值只考虑各个不同的值。

  如果指定列名，则 `DISTINCT` 只能用于 `COUNT()`。`DISTINCT` 不能用于 `COUNT(*)`，因此不允许使用`COUNT(DISTINCT)`， 否则会产生错误。类似地，`DISTINCT` 必须使用列名，不能用于计算或表达式。

  虽然 `DISTINCT` 从技术上可 用于 `MIN()` 和 `MAX()`，但这样做实际上没有价值。一个列中的最小值和最大值不管是否包含不同值都是相同的。

- **组合聚集函数**

  `SELECT` 语句可根据需要包含多个聚集函数。

  ``` sql
  SELECT COUNT(*) AS count_value,
  			 MIN(<column>) AS min_value,
  			 MAX(<column>) AS max_value,
  			 AVG(<column>) AS avg_value,
  FROM <table>;
  ```

  在指定别名以包含某个聚集函数的结果时，不应该使用表中实际的列名。虽然这样做并非不合法，但使用唯一的名字会使你的 SQL 更易于理解和使用（以及将来容易排除故障）。


### 分组数据

分组允许把数据分为多个逻辑组，以便能对每个组进行聚集计算。

- **`GROUP BY` 子句建立分组**

  ``` sql
  SELECT <column>, COUNT(*) AS num_value FROM <table> GROUP BY <column>;
  ```

  `GROUP BY` 子句指示 MySQL 按 <column> 排序并分组数据。然后对每个组而不是整个结果集进行聚集。

  使用 **`WITH ROLLUP`** 关键字，可以得到每个分组以及每个分组汇总级别（针对每个分组）的值。

  ``` sql
  SELECT <column>, COUNT(*) AS num_value FROM <table> GROUP BY <column> WITH ROLLUP;
  ```

  在具体使用GROUP BY子句前，需要知道一些重要的规定

  1. `GROUP BY` 子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。
  2. 如果在 `GROUP BY` 子句中嵌套了分组，数据将在最后规定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）。
  3. `GROUP BY` 子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在 `SELECT` 中使用表达式， 则必须在 `GROUP BY` 子句中指定相同的表达式。不能使用别名。
  4. 除聚集计算语句外，`SELECT` 语句中的每个列都必须在 `GROUP BY` 子句中给出。
  5. 如果分组列中具有 `NULL` 值，则 `NULL` 将作为一个分组返回。如果列中有多行 `NULL` 值，它们将分为一组。
  6. `GROUP BY` 子句必须出现在 `WHERE` 子句之后，`ORDER BY` 子句之前。

- **`HAVING` 子句过滤分组**

  `HAVING` 非常类似于 `WHERE`，支持所有 `WHERE` 操作符。唯一的差别是 `WHERE` 过滤行，而 `HAVING` 过滤分组。

  `HAVING` 和 `WHERE` 的差别，这里有另一种理解方法，`WHERE` 在数据分组前进行过滤，`HAVING` 在数据分组后进行过滤。这是一个重要的区别，`WHERE` 排除的行不包括在分组中。这可能会改变计算值，从而影响 `HAVING` 子句中基于这些值过滤掉的分组。

  过滤是基于分组聚集值而不是特定行值的。

  ``` sql
  SELECT <column>, COUNT(*) AS num_value 
  FROM <table> 
  GROUP BY <column>
  HAVING COUNT(*) >= 2;
  ```

  过滤了 `COUNT(*) >= 2` 的那些分组。

  ``` sql
  SELECT <column>, COUNT(*) AS num_value 
  FROM <table>
  WHERE <column1> >= 10
  GROUP BY <column>
  HAVING COUNT(*) >= 2;
  ```

  `WHERE` 子句过滤所有 `<column1>` 至少为10的行。然后按 `<column>` 分组数据，`HAVING`子句过滤计数为 2 或 2 以上的分组。

- **分组和排序**

  `GROUP BY` 和 `ORDER BY` 的差别

  | GROUP BY                                                  | ORDER BY                                      |
  | --------------------------------------------------------- | --------------------------------------------- |
  | 分组行。但输出可能不是分组的顺序                          | 排序产生的输出                                |
  | 只可能使用选择列或表达式列，而且必须使用每个选择 列表达式 | 任意列都可以使用（甚至 非选择的列也可以使用） |
  | 如果与聚集函数一起使用列（或表达式），则必须使用          | 不一定需要                                    |

  **不要忘记 `ORDER BY`**，一般在使用 `GROUP BY` 子句时，应该也给出 `ORDER BY` 子句。这是保证数据正确排序的唯一方法。千万 不要仅依赖 `GROUP BY` 排序数据。

  ``` sql
  SELECT <column>, SUM(<column1>*<column2>) AS total_value
  FROM <table>
  GROUP BY <column>
  HAVING SUM(<column1>*<column2>) >= 50
  ORDER BY total_value;
  ```

  `GROUP BY` 子句用来按 `<column>` 分组数据，以便 `SUM(*)` 函数能够返回总计 `total_value`。`HAVING` 子句过滤数据，使得只返回总计 `total_value` 大于等于50的记录。最后，用 `ORDER BY` 子句排序输出。

- **SELECT子句顺序**

  在 `SELECT` 语句中 使用时必须遵循的次序

  | 子句       | 说明               | 是否必须使用           |
  | ---------- | ------------------ | ---------------------- |
  | `SELECT`   | 要返回的列或表达式 | 是                     |
  | `FROM`     | 从中检索数据的表   | 仅在从表选择数据时使用 |
  | `WHERE`    | 行级过滤           | 否                     |
  | `GROUP BY` | 分组说明           | 仅在按组计算聚集时使用 |
  | `HAVING`   | 组级过滤           | 否                     |
  | `ORDER BY` | 输出排序顺序       | 否                     |
  | `LIMIT`    | 要检索的行数       | 否                     |

### 子查询

- SQL 还允许创建**子查询（subquery）**，即嵌套在其他查询中的查询。

- **利用子查询进行过滤**

  现在，假如需要列出订购物品 TNT2 的所有客户，所需的具体步骤

  1. 检索包含物品 TNT2 的所有订单的编号。

     对于 prod_id 为 TNT2 的所有订单物品，它检索其 order_num 列。

     ``` sql
     SELECT order_num FROM orderitems WHERE prod_id = 'TNT2';
     ```

  2. 检索具有前一步骤列出的订单编号的所有客户的 ID（假设第一步查询结果为 20005 和 20007）。

     ``` sql
     SELECT cust_id FROM orders WHERE order_num IN (20005,20007);
     ```

  3. 检索前一步骤返回的所有客户ID的客户信息（假设第一步查询结果为 10001 和 10004）。

     ``` sql
     SELECT cust_name, cust_contact FROM customers WHERE cust_id IN (10001,10004);
     ```

  把上边三个步骤的 `WHERE` 子句转换为子查询

  ``` sql
  SELECT cust_name, cust_contact 
  FROM customers 
  WHERE cust_id IN (SELECT cust_id
                    FROM orders 
                    WHERE order_num IN (SELECT order_num
                                        FROM orderitems
                                        WHERE prod_id = 'TNT2'));
  ```

  在 `SELECT`  语句中，子查询总是从内向外处理。
  
  在 `WHERE` 子句中使用子查询（如这里所示），应该保证 `SELECT` 语句具有与 `WHERE` 子句中相同数目的列。通常，子查询将返回单个列并且与单个列匹配，但如果需要也可以使用多个列。
  
  虽然子查询一般与 `IN` 操作符结合使用，但也可以用于测试等于（`=`）、不等于（`<>`）等。
  
- **作为计算字段使用子查询**

  ``` sql
  SELECT cust_name,
  			 cust_state,
  			 (SELECT COUNT(*)
          FROM orders
          WHERE orders.cust_id = customers.cust_id) AS orders
  FROM customers
  ORDER BY cust_name;
  ```

  这条 `SELECT` 语句对 `customers` 表中每个客户返回 3 列：cust_name、cust_state 和 orders。orders是一个计算字段， 它是由圆括号中的子查询建立的。该子查询对检索出的每个客户执行一次。

  子查询中的 `WHERE` 子句与前面使用的 `WHERE` 子句稍有不同，因为它使用了完全限定列名

  **相关子查询（correlated subquery）** 涉及外部查询的子查询



### 联结

- 联结是一种机制，用来在一条 `SELECT` 语句中关联表，因此称之为联结。

  ``` sql
  SELECT <column1>, <column2>, <column3> 
  FROM <table1>, <table2>
  WHERE <table1>.<column_id> = <table2>.<column_id>
  ORDER BY <column1>, <column2>;
  ```

  `SELECT` 语句的 <column2>, <column3>  在一个表中，而另一个列 <column1> 在另一个表中。

  `FROM` 子句列出了两个表 <table1>, <table2>。它们就是这条 `SELECT` 语句联结的两个表的名字。这两个表用 `WHERE` 子句正确联结，`WHERE` 子句匹配 <table1> 表中的 <column_id> 和 <table2> 表中的 <column_id>。

  **完全限定列名** 在引用的列可能出现二义性时，必须使用完全限定列名（用一个点分隔的表名和列名）。

  在联结两个表时，你实际上做的是将第一个表中的每一行与第二个表中的每一行配对。`WHERE` 子句作为过滤条件，它只包含那些匹配给定条件（这里是联结条件）的行。没有 `WHERE` 子句，第一个表中的每个行将与第二个表中的每个行配对，而不管它们逻辑上是否可以配在一起。

  **笛卡儿积（cartesian product）** 由没有联结条件的表关系返回的结果为笛卡儿积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

  **不要忘了 `WHERE` 子句 **应该保证所有联结都有 `WHERE` 子句，否则将返回比想要的数据多得多的数据。同理，应该保证 `WHERE` 子句的正确性。不正确的过滤条件将导致返回的数据不正确。

- **内部联结**

  ``` sql
  SELECT <column1>, <column2>, <column3> 
  FROM <table1> INNER JOIN <table2>
  	ON <table1>.<column_id> = <table2>.<column_id>;
  ```

  此语句中的 `SELECT` 与前面的 `SELECT` 语句相同，但 `FROM` 子句不同。这里，两个表之间的关系是 `FROM` 子句的组成部分，以 `INNER JOIN` 指定。在使用这种语法时，联结条件用特定的 `ON` 子句而不是 `WHERE` 子句给出。传递给 `ON` 的实际条件与传递给 `WHERE` 的相同。

  ANSI SQL 规范首选 `INNER JOIN` 语法。此外，尽管使用 `WHERE` 子句定义联结的确比较简单，但是使用明确的联结语法能够确保不会忘记联结条件，有时候这样做也能影响性能。

- **联结多个表**

  SQL对一条SELECT语句中可以联结的表的数目没有限制。

  ``` sql
  SELECT <column1>, <column2>, <column3>, <column4>
  FROM <table1>, <table2>, <table3>
  WHERE <table1>.<column_id> = <table2>.<column_id>
  	AND <table1>.<column_id> = <table3>.<column_id>
  	AND <column> = 123;
  ```

  MySQL 在运行时关联指定的每个表以处理联结。这种处理可能是非常耗费资源的，因此应该仔细，不要联结不必要的表。联结的表越多，性能下降越厉害。

  子查询并不总是执行复杂 `SELECT` 操作的最有效的方法

  ``` sql
  SELECT cust_name, cust_contact
  FROM customers
  WHERE cust_id IN (SELECT cust_id
                    FROM orders 
                    WHERE order_num IN (SELECT order_num
                                        FROM orderitems
                                        WHERE prod_id = 'TNT2'));
  ```

  使用联结

  ``` sql
  SELECT cust_name, cust_contact
  FROM customers, orders, orderitems
  WHERE customers.cust_id = orders.cust_id
  	AND orderitems.order_num = orders.order_num
  	AND prod_id = 'TNT2';
  ```



### 高级联结

- **表别名**

  别名除了用于列名和计算字段外，SQL还允许给表名起别名。这样做有两个主要理由：

  1. 缩短SQL语句
  2. 允许在单条SELECT语句中多次使用相同的表

  ``` sql
  SELECT <column1>, <column2>
  FROM <table1> AS t1, <table2> AS t2, <table3> AS t3
  WHERE t1.id = t2.id
  	AND t1.num = t3.num
  	AND <column> = 123;
  ```

  表别名不仅能用于 `WHERE` 子句，它还可以用于 `SELECT` 的列表、`ORDER BY` 子句以及语句的其他部分。

  表别名只在查询执行中使用。与列别名不一样，表别名不返回到客户机。

- **自联结**

  使用子查询

  ``` sql
  SELECT prod_id, prod_name
  FROM products
  WHERE vend_id = ( SELECT vend_id
                   FROM products
                   WHERE prod_id = 'DTNTR');
  ```

  使用联结

  ``` sql
  SELECT p1.prod_id, p1.prod_name
  FROM products AS p1, products AS p2
  WHERE p1.vend_id = p2.vend_id
  	AND p2.prod_id = 'DTNTR';
  ```

  `SELECT` 语句使用 p1 前缀明确地给出所需列的全名。

  此查询中需要的两个表实际上是相同的表，因此 products 表在 `FROM` 子句中出现了两次。 

  `WHERE`（通过匹配 p1 中的 vend_id 和 p2 中的 vend_id ）首先联结两个表， 然后按第二个表中的 prod_id 过滤数据，返回所需的数据。

  **用自联结而不用子查询**自联结通常作为外部语句用来替代从相同表中检索数据时使用的子查询语句。虽然最终的结果是相同的，但有时候处理联结远比处理子查询快得多。

- **自然联结**

  无论何时对表进行联结，应该至少有一个列出现在不止一个表中（被联结的列）。标准的联结（内部联结）返回所有数据，甚至相同的列多次出现。自然联结排除多次出现，使每个列只返回一次。

  ``` sql
  SELECT c.* o.order_num, o.order_date, oi.prod_id, oi.quantity, oi.item_price
  FROM customers AS c, orders AS o, orderitems AS oi
  WHERE c.cust_id = o.cust_id
  	AND oi.order_num = o.order_num
  	AND prod_id = 'FB';
  ```

  通配符只对第一个表使用。所有其他列明确列出，所以没有重复的列被检索出来。

- **外部联结**

  





---



## PostgreSQL

### 基础

- **连接数据库**

  ``` sql
  psql -U <user name> -d <database name>
  ```

- **查看全部数据库** `\l`

- **连接数据库** `\c <database name>`

- **查看全部表** `\d`

- **查看表** `\d <table name>`

- **退出数据库** `\q` or `control + d`


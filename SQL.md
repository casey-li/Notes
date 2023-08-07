
[课程：Mosh_完全掌握SQL【笔记】](https://zhuanlan.zhihu.com/p/222865842)

别人的笔记, 自己复制过来的

- [第一部分：基础——增删查改](#第一部分基础增删查改)
  - [第二章、在单一表格中检索数据](#第二章在单一表格中检索数据)
  - [第三章、在多张表格中检索数据](#第三章在多张表格中检索数据)
  - [第四章、插入、更新和删除数据](#第四章插入更新和删除数据)
- [第二部分：基础进阶——汇总、复杂查询、内置函数](#第二部分基础进阶汇总复杂查询内置函数)
  - [第五章、汇总数据](#第五章汇总数据)
  - [第六章、编写复杂查询](#第六章编写复杂查询)
  - [第七章、MySQL的基本函数](#第七章mysql的基本函数)
- [第三部分：提高效率——视图、存储过程、函数](#第三部分提高效率视图存储过程函数)
  - [第八章、视图](#第八章视图)
  - [第九章、存储过程](#第九章存储过程)
  - [1. 什么是存储过程](#1-什么是存储过程)
  - [2. 创建一个存储过程](#2-创建一个存储过程)
  - [3. 使用MySQL工作台创建存储过程](#3-使用mysql工作台创建存储过程)
  - [4. 删除存储过程](#4-删除存储过程)
  - [5. 参数](#5-参数)
  - [6. 带默认值的参数](#6-带默认值的参数)
  - [7. 参数验证](#7-参数验证)
  - [8. 输出参数](#8-输出参数)
  - [9. 变量](#9-变量)
  - [10. 函数](#10-函数)
  - [11. 其他约定](#11-其他约定)
  - [A Quick Note](#a-quick-note)

# 第一部分：基础——增删查改

## 第二章、在单一表格中检索数据

- **1、选择语句**

**实例**

```sql
USE sql_store;

SELECT * / 1, 2  -- 纵向筛选列，甚至可以是常数
FROM customers  -- 选择表
WHERE customer_id < 4  -- 横向筛选行
ORDER BY first_name  -- 排序

-- 单行注释

/*
多行注释
*/
```

- **2、选择子句**

**小结**

SELECT 是**列/字段**选择语句，可选择列，列间数学表达式，特定值或文本，可用AS关键字设置列别名（AS可省略），注意 `DISTINCT` 关键字的使用。

**注意**

**SQL会完全无视大小写（绝大数情况下的大小写）、多余的空格（超过一个的空格）、缩进和换行，SQL语句间完全由分号 `;` 分割**，用缩进、换行等只是为了代码看着更美观结构更清晰，这些与Python很不同，要注意。

**实例**

```sql
USE sql_store;

SELECT
    DISTINCT last_name, 
    -- 这种选择的字段中部分取 DISTINCT 的是如何筛选的？
    first_name,
    points,
    (points + 70) % 100 AS discount_factor/'discount factor'
    -- % 取余（取模）
FROM customers
```

- **3、WHERE子句**

**小结**

WHERE 是行筛选条件，实际是一行一行/一条条记录依次验证是否符合条件，进行筛选

**导航**

3~9 节讲的都是写 WHERE 子句中表达筛选条件的不同方法，这一节（第3节）主要讲比较运算，第4节讲逻辑运算 AND、OR、NOT，5~9可看作都是在讲特殊的比较运算（是否符合某种条件）：IN、BETWEEN、LIKE、REGEXP、IS NULL。

所以总的来说WHERE条件就是 **数学→比较→逻辑运算**，逻辑层次和执行优先级也是按照这三个的顺序来的

```sql
USE sql_store;

SELECT *
FROM customers
WHERE points > 3000  
/WHERE state != 'va'  -- 'VA'/'va'一样
```

**比较运算符 > < = >= <= !=/<>** ，注意等于是一个等号而不是两个等号

也可对日期或文本进行比较运算，**注意 SQL 里日期的标准写法及其需要用引号包裹**这一点

```sql
WHERE birth_date > '1990-01-01'

-- 今年（2019） 的订单

USE sql_store;

select *
from orders
where order_date > '2019-01-01'
-- 有更一般的方法，不用每年改代码，之后教
```

- **4、AND, OR, NOT运算符**

**小结**

用逻辑运算符AND、OR、NOT对（数学和）比较运算进行组合实现多重条件筛选  
**执行优先级：数学→比较→逻辑**

```sql
USE sql_store;

SELECT *
FROM customers
WHERE birth_date > '1990-01-01' AND points > 1000
/WHERE birth_date > '1990-01-01' OR 
      points > 1000 AND state = 'VA'
```

**AND优先级高于OR，但最好加括号，更清晰**

> WHERE birth_date > '1990-01-01' OR (points > 1000 AND state = 'VA')

NOT的用法

> WHERE NOT (birth_date > '1990-01-01' OR points > 1000)

去括号等效转化为

> WHERE birth_date <= '1990-01-01' AND points <= 1000


- **5、IN运算符**

**小结**

用 IN 运算符将某一属性**与多个值（一系列值）进行比较**  
实质是多重相等比较运算条件的简化

```sql
--选出'va'、'fl'、'ga'三个州的顾客

USE sql_store;

SELECT * FROM customers
WHERE state = 'va' OR state = 'fl' OR state = 'ga'
```

不能 `state = 'va' OR 'fl' OR 'ga'` 因为数学和比较运算优先于逻辑运算，加括号 `state = ('va' OR 'fl' OR 'ga')` 也不行，逻辑运算符只能链接布林值。

用 IN 操作符简化该条件

> WHERE state IN ('va', 'fl', 'ga')

可加NOT

> WHERE state NOT IN ('va', 'fl', 'ga')

这里可用NOT的原因：可以这么看，IN语句 `IN ('va', 'fl', 'ga')` 是在进行一种是否符合条件的判断，可看作是一种特殊的比较运算，得到的是一个逻辑值，故可用NOT进行取反

- **6、BETWEEN运算符**

**小结**

用于表达**范围**型条件

**注意**

*   用AND而非括号
*   **闭区间，包含两端点**
*   也可用于日期，毕竟日期本质也是数值，日期也有大小（早晚），可比较运算
*   同 IN 一样，BETWEEN 本质也是一种特定的 多重比较运算条件 的简化

**案例**

```sql
-- 选出积分在1k到3k的顾客
USE sql_store;

select * from customers
where points >= 1000 and points <= 3000

-- 等效简化为：
WHERE points BETWEEN 1000 AND 3000

-- 选出90后的顾客
SELECT * FROM customers
WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01'
```

注意两端都是包含的 不能写作BETWEEN (1000, 3000)！别和IN的写法搞混

- **7、LIKE运算符**

**小结**

模糊查找，查找具有某种模式的字符串的记录/行

**注意**

*   注意和正则表达式一样都是用引号包裹表示字符串

```sql
USE sql_store;
SELECT * FROM customers
WHERE last_name like 'brush%' / 'b____y'
```

引号内描述想要的字符串模式，注意SQL（几乎）任何情况都是不区分大小写的

两种通配符：

*   `%` 任何个数（包括0个）的字符（**用的更多**）
*   `_` 单个字符

**练习**

分别选择满足如下条件

```sql
-- 地址包含 'TRAIL' 或 'AVENUE' 的顾客：

USE sql_store;

select * 
from customers
where address like '%Trail%' or 
      address like '%avenue%'
```

LIKE 执行优先级在逻辑运算符之前，毕竟 **IN BETWEEN LIKE 本质可看作是比较运算符的简化，应该和比较运算同级，数学→比较→逻辑，始终记住这个顺序**

LIKE 的判断结果也是个 TRUE / FASLE 的问题，任何逻辑值都可前置NOT来取反

- **8、REGEXP运算符**

**小结**

正则表达式，在搜索字符串方面更为强大，可搜索更复杂的模板

```sql
USE sql_store;

select * from customers
where last_name like '%field%'

-- 等效于：
where last_name regexp 'field'
```
regexp 是 regular expression（正则表达式） 的缩写

正则表达式可以组合来表达更复杂的字符串模式

```sql
where last_name regexp '^mac|field$|rose' 
where last_name regexp '[gi]e|e[fmq]' -- 查找含ge/ie或ef/em/eq的
where last_name regexp '[a-h]e|e[c-j]'
```

正则表达式总结：

| 符号    | 意义   |
| ------- | ------ |
| `^`     | 开头   |
| `$`     | 结尾   |
| `[abc]` | 含abc  |
| `[a-c]` | 含a到c |
| `       | `      | logical or |

正则表达式用法还有更多...

```sql
-- 分别选择满足如下条件的顾客：

select * 
from customers
where first_name regexp 'elka|ambur'    -- 1. first names 是 ELKA 或 AMBUR

where last_name regexp 'ey$|on$'        -- 2. last names 以 EY 或 ON 结束

where last_name regexp '^my|se'         -- 3. last names 以 MY 开头 或包含 SE

where last_name regexp 'b[ru]'/'br|bu'  -- 4. last names 包含 BR 或 BU
```

- **9、IS NULL运算符**

**小结**

找出空值，找出有某些属性缺失的记录

```sql
-- 找出电话号码缺失的顾客，也许发个邮件提醒他们之类

USE sql_store;

select * from customers
where phone is null/is not null
```

**回顾**

3~9 节全在讲WHERE子句中条件的具体写法 :

*   第3节：比较运算 `> < = >= <= !=`
*   第4节：逻辑运算 `AND`、`OR`、`NOT`
*   5~9节：特殊的比较运算（是否符合某种条件）：`IN` 和 `BETWEEN`、`LIKE` 和 `REGEXP`、`IS NULL`

所以总的来说WHERE条件就是

> 数学运算 → 比较运算（包括特殊的比较运算）→ 逻辑运算

逻辑层次和执行优先级也是按照这三个的顺序来的。

- **10、ORDER BY 子句**

**小结**

排序语句，和 `SELECT ……` 很像：

1.  可多列
2.  **可以是列间的数学表达式**
3.  **可包括任何列，包括没选择的列（MySQL特性，其它DBMS可能报错），**
4.  **可以是之前定义好的别名列（MySQL特性，甚至可以是用一个常数设置的列别名）**
5.  任何一个排序依据列后面都可选加 DESC

**注意**

最好别用 `ORDER BY 1, 2`（表示以 `SELECT ……` 选中列中的第1、2列为排序依据） 这种隐性依据，因为SELECT选择的列一变就容易出错，还是显性地写出列名作为排序依据比较好

**实例**
```sql
USE sql_store;

select name, unit_price * 1.1 + 10 as new_price 
from products
order by new_price desc, product_id
-- 这两个分别是 别名列 和 未选择列，都用到了 MySQL 特性

-- 订单2的商品按总价降序排列:

-- 法1. 可以以总价的数学表达式为排序依据
select * from order_items 
where order_id = 2
order by quantity * unit_price desc -- 列间数学表达式


-- 法2. 或先定义总价别名，在以别名为排序依据
select *, quantity * unit_price as total_price 
from order_items 
where order_id = 2
order by total_price desc -- 列别名
```

- **11、LIMIT子句**

**小结**

限制返回结果的记录数量，“前N个” 或 “跳过M个后的前N个”

**实例**

```sql
USE sql_store;

select * from customers
limit 3 
/limit 6, 3
```

6, 3 表示跳过前6个，取第7~9个，6是偏移量 
如：网页分页 每3条记录显示一页 第3页应该显示的记录就是 limit 6, 3

**回顾**

SELECT 语句完结了，里面的子句顺序固定要记牢，顺序乱会报错  
`select` `from` `where` + `order by` `limit` 

## 第三章、在多张表格中检索数据

常常需要在多张表中检索数据，这一章讲的就是这个，具体来说，主要讲如何横向连接表和纵向连接查询结果

- **1、内连接（Inner Joins）**

**小结**

各表分开存放是为了减少重复信息和方便修改，需要时可以根据相互之间的关系连接成相应的合并详情表以满足相应的查询。FROM JOIN ON 语句就是告诉sql： 将哪几张表以什么基础连接/合并起来。

这种有多表合并的查询语句可分两部分从后往前看：

1. 后面的 `from` 表A `join` 表B `on` AB 的关系，就是以某些相关联的列为依据（关系型数据库就是这么来的）进行多表合并得到所需的详情表

2. 前面的 select 就是在合并详情表中找到所需的列

**关于表别名**

之前在SELECT中给选定的列加别名主要是为了得到更有意义的列名，这里在 FROM JOIN 中给表加别名主要是为了简化

```sql
USE sql_store;

select 
    order_id, 
    o.customer_id, 
    ……
from orders as o
    ……
(inner) join customers c 
    on o.customer_id = c.customer_id
```

因为两个表都有这个 `customer_id` 列，只写 `customer_id` 的话会报错：ambiguous，必须指定一个表的 `customer_id`，这里指定任意一个表的都行，因为正是按相等的 `customer_id` 来链接两个表的。总之选择多张表里都有的同名列时，必须加上表名前缀来明确列的来源

用了别名后其他地方（包括前面select语句，这一点当时觉得挺奇怪的，后来知道了是SQL语句执行顺序的关系，FROM … JOIN … 语句最先执行）只能用别名，用全名会报错。另外就像在 select 里一样，这里as也是可省略的

因为别名需要在 WHERE JOIN 语句中确定等原因，最好先SELECT * FROM 选择全部，等写好了 FROM JOIN ON 等后面的语句，即确定了选哪些表以及怎么链接它们并取好了表别名后，再回头去 SELECT 里细化明确需要的列

**练习**
```sql
-- 通过 product\_id 链接 orders\_items 和 products:
USE sql_store;

select *
/select oi.*, p.name
/select 
    order_id, 
    oi.product_id, 
    name, 
    quantity, 
    oi.unit_price  
    ……
from order_items oi
join products p
    on oi.product_id = p.product_id  -- 链接的基础
```
两个表都有 `unit_price`，故要指明要的是哪一个，有两个单价是因为单价会变，订单项目表里的是下订单时的实际单价，产品表里的单价是目前的价格，若计算总价该用前者，别搞错了

- **2、跨数据库连接（合并）**

**小结**

有时需要选取不同库的表的列，其他都一样，就只是WHERE JOIN里对于非现在正在用的库的表要**加上库名前缀而已**。依然可用别名来简化。**只有非当前使用的库才要加库前缀**

```sql
use sql_store;

select * from order_items oi
join sql_inventory.products p
    on oi.product_id = p.product_id

-- 或
use sql_inventory;

select * from sql_store.order_items oi
join products p
    on oi.product_id = p.product_id
```

- **3、自连接**

**小结**

一个表和它自己合并。如下面的例子，员工的上级也是员工，所以也在员工表里，所以想得到的有员工和他的上级信息的合并表，就要员工表自己和自己合并，**用两个不同的表别名即可实现**。这个例子中只有两级，但也可用类似的方法构建多层级的组织结构。

```sql
USE sql_hr;

select 
    e.employee_id,
    e.first_name,
    m.first_name as manager
    ……
from employees e
join employees m
    on e.reports_to = m.employee_id
```
自合并必然每列都要加表前缀，因为每列都同时在两张表中出现。另外，两个 first_name 列有歧义，注意将最后一列改名为 manager 使得结果表更易于理解

- **4、多表连接**

`FROM` 一个核心表A，用多个 `JOIN …… ON ……` 分别通过不同的链接关系链接不同的表B、C、D……，通常是让表B、C、D……为表A提供更详细的信息从而合并为一张详情合并版A表，即：

```sql
FROM  A 
    JOIN B ON AB的关系 
    JOIN C ON AC的关系 
    JOIN D ON AD的关系 
    ……
```

将得到一个合并了BCD……等表详细信息的详情合并版A表

```sql
-- 订单表同时链接顾客表和订单状态表，合并为有顾客和状态信息的详细订单表
USE sql_store;

SELECT 
    o.order_id, 
    o.order_date,
    c.first_name,
    c.last_name,
    os.name AS status
FROM orders o
JOIN customers c
    ON o.customer_id = c.customer_id
JOIN order_statuses os
    ON o.status = os.order_status_id

-- 同理，支付记录表链接顾客表和支付方式表形成支付记录详情表

USE sql_invoicing;

SELECT 
    p.invoice_id,
    p.date,
    p.amount,
    c.name,
    pm.name AS payment_method
FROM payments p
JOIN clients c
    ON p.client_id = c.client_id
JOIN payment_methods pm
    ON p.payment_method = pm.payment_method_id
```

**5、复合连接条件**

**小结**

像订单项目（order_items）这种表，**订单id和产品id合在一起才能唯一表示一条记录**，这叫**复合主键**，设计模式下也可以看到两个字段都有PK标识，订单项目备注表（order_item_notes）也是这两个复合主键，因此他们两合并时要用复合条件：`FROM` 表1 `JOIN` 表2 `ON` 条件1 `AND` 条件2

```sql
-- 将订单项目表和订单项目备注表合并
USE sql_store;

SELECT * 
FROM order_items oi
JOIN order_item_notes oin
    ON oi.order_Id = oin.order_Id
    AND oi.product_id = oin.product_id
```

- **6、隐式连接语法**

**小结**

就是用 FROM WHERE 取代 FROM JOIN ON

**注意**

尽量别用，因为若忘记 WHERE 条件筛选语句，不会报错但会得到交叉合并（cross join）结果：即10条 order 会分别与10个 customer 结合，得到100条记录。最好使用显性合并语法，因为会强制要求你写合并条件ON语句，不至于漏掉。

```sql
-- 合并顾客表和订单表，显性合并：
USE sql_store;

SELECT * 
FROM orders o
JOIN customers c
    ON o.customer_id = c.customer_id

-- 隐式合并语法：
SELECT * 
FROM orders o, customers c  
WHERE o.customer_id = c.customer_id
```

注意 FROM 子句里的逗号，就像 SELECT 多条列用逗号隔开一样，FROM 多个表也用逗号隔开，此时若忘记WHERE条件筛选语句则得到这几张表的交叉合并结果

这里也可以看得出来，ON/USING 和 WHERE 以及后面会学的 HAVING 的作用是类似的，本质上都是**对行进行筛选的条件语句**，只不过使用的位置不一样而已

**7、外连接**

**小结**

*   `(INNER) JOIN` 结果只包含两表的交集，**另外注意“广播（broadcast）”效应**
*   `LEFT/RIGHT (OUTER) JOIN` 结果里除了交集，还包含只出现在左/右表中的记录

```sql
-- 合并顾客表和订单表，用 INNER JOIN：

USE sql_store;

SELECT 
    c.customer_id,
    c.first_name,
    o.order_id
FROM customers c
JOIN orders o
    ON o.customer_id = c.customer_id
ORDER BY customer_id
```

这样是INNER JOIN，只展示有订单的顾客（及其订单），也就是两张表的交集，但注意这里因为一个顾客可能有多个订单，所以INNER JOIN以后顾客信息其实是是广播了的，即一条顾客信息被多条订单记录共用，当然 这叫广播（broadcast）效应，是另一个问题，这里关注的重点是 INNER JOIN 的结果确实是两表的交集，是那些同时有顾客信息和订单信息的记录。

若要展示全部顾客（及其订单，如果有的话），要改用LEFT (OUTER) JOIN，结果相较于 INNER JOIN 多了没有订单的那些顾客，即只有顾客信息没有订单信息的记录

当然，也可以调换左右表的顺序（即调换FROM和JOIN的对象）再 RIGHT JOIN，即：

```sql
FROM orders o
    RIGHT [OUTER] JOIN customers c
    -- 中括号 [] 表示是可选项、可省略 
    ON o.customer_id = c.customer_id
```

若要展示全部订单（及其顾客），就应该是 customers RIGHT JOIN orders，结果相较于 `INNER JOIN` 多了没有顾客的那些订单，即只有订单信息没有顾客信息的记录。（注：因为这里所有订单都有顾客，所以这里 RIGHT JOIN 结果和 INNER JOIN 一样）

```sql
-- 展示各产品在订单项目中出现的记录和销量，也要包括没有订单的产品

SELECT 
    p.product_id,
    p.name, -- 或者直接name
    oi.quantity -- 或者直接quantity
FROM products p
LEFT JOIN order_items oi
    ON p.product_id = oi.product_id
```

**8、多表外连接**

**小结**

与内连接类似，我们可以对多个表（3个及以上）进行外连接，最好只用 JOIN 和 LEFT JOIN

```sql
-- 查询顾客、订单和发货商记录，要包括所有顾客（包括无订单的顾客），也要包括所有订单（包括未发出的）
USE sql_store;

SELECT 
    c.customer_id,
    c.first_name,
    o.order_id,
    sh.name AS shipper
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
LEFT JOIN shippers sh
    ON o.shipper_id = sh.shipper_id
ORDER BY customer_id
```

虽然可以调换顺序并用 RIGHT JOIN，但作为最佳实践，最好调整顺序并统一只用 `[INNER] JOIN` 和 `LEFT [OUTER] JOIN`（总是左表全包含），这样，当要合并的表比较多时才方便书写和理解而不易混乱

```sql
-- 查询 订单 + 顾客 + 发货商 + 订单状态，包括所有的订单（包括未发货的），其实就只是前两个优先级变了一下，是要看全部订单而非全部顾客了
USE sql_store;

SELECT 
    o.order_id,
    o.order_date,
    c.first_name AS customer,
    sh.name AS shipper,
    os.name AS status
FROM orders o
JOIN customers c
    ON o.customer_id = c.customer_id
LEFT JOIN shippers sh
    ON o.shipper_id = sh.shipper_id
JOIN order_statuses os
    ON o.status = os.order_status_id
```

订单必有顾客和状态，所以这第1个和第3个 JOIN 加不加 LEFT 效果一样 但订单不一定发货了，即不一定有发货商，所以第2个 JOIN 必须是 LEFT JOIN，否者会筛掉没发货的订单

- **9、自我外部连接**

**小结**

就用前面那个员工表的例子来说，就是用LEFT JOIN让得到的 员工-上级 合并表也包括老板本人（老板没有上级，即 reports_to 字段为空，如果用 JOIN 会被筛掉，用 LEFT JOIN 才能保留）

```sql
USE sql_hr;

SELECT 
    e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
LEFT JOIN employees m  -- 包含所有雇员（包括没有report_to的老板本人）
    ON e.reports_to = m.employee_id
```

**10、USING子句**

**小结**

当作为合并条件（join condition）的列在两个表中有相同的列名时，可用 `USING (……, ……)` 取代 `ON …… AND ……` 予以简化，内/外链接均可如此简化。

**一定注意 USING 后接的是括号，特容易搞忘**

```sql
SELECT
    o.order_id,
    c.first_name,
    sh.name AS shipper
FROM orders o
JOIN customers c
    USING (customer_id)
LEFT JOIN shippers sh
    USING (shipper_id)
ORDER BY order_id
```

**复合主键**表间复合连接条件的合并也可用 USING，中间逗号隔开就行：

```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
ON oi.order_id = oin.order_Id AND
    oi.product_id = oin.product_id
/USING (order_id, product_id)

```

USING对复合主键的简化效果更加明显

**练习**

sql_invoicing库里，将payments、clients、payment_methods三张表合并起来，以知道什么日期哪个顾客用什么方式付了多少钱

```sql
USE sql_invoicing;

SELECT 
    p.date,
    c.name AS client,
    pm.name AS payment_method,
    p.amount
FROM payments p
JOIN clients c USING (client_id)
JOIN payment_methods pm
    ON p.payment_method = pm.payment_method_id
```

**注意**

列名不同就必须用 ON …… 了  
实际中同一个字段在不同表列名不同的情况也很常见（如上面的 payment_method 和payment_method_id），不能想当然的用USING

**11、自然连接**

**小结**

`NATURAL JOIN` 就是让MySQL自动检索同名列作为合并条件。

**注意**

最好别用，因为不确定合并条件是否找对了，有时会造成无法预料的问题，编程时保持对结果的**控制**是非常重要的

但也要知道有这个东西，混个脸熟，不要别人用了看不懂。

```sql
USE sql_store;

SELECT 
    o.order_id,
    c.first_name
FROM orders o
NATURAL JOIN customers c
```

**12、交叉连接**

**小结**

得到名字和产品的**所有组合**，因此**不需要合并条件**。 实际运用如：要得到尺寸和颜色的全部组合

```sql
-- 得到顾客和产品的全部组合（毫无意义，纯粹为了展示交叉连接）
USE sql_store;

SELECT 
    c.first_name AS customer,
    p.name AS product
FROM customers c
CROSS JOIN products p
ORDER BY c.first_name

```

上面是显性语法，还有隐式语法，之前讲过，其实就是隐式内合并忽略WHERE子句（即合并条件）的情况，也就是把 `CROSS JOIN` 改为逗号，即 `FROM A CROSS JOIN B` 等效于 `FROM A, B`，Mosh更推荐显式语法，因为更清晰

```sql
USE sql_store;

SELECT 
    c.first_name,
    p.name
FROM customers c, products p
ORDER BY c.first_name
```

```sql
-- 交叉合并shippers和products，分别用显式和隐式语法

USE sql_store;

SELECT 
    sh.name AS shippers,
    p.name AS product
FROM shippers sh
CROSS JOIN products p
ORDER BY sh.name

-- 或

SELECT 
    sh.name AS shippers,
    p.name AS product
FROM shippers sh, products p
ORDER BY sh.name
```

**13、联合**

**小结**

`FROM …… JOIN ……` 可对多张表进行横向列合并，而 `…… UNION ……` 可用来按行纵向合并多个查询结果，这些查询结果可能来自相同或不同的表

*   同一张表可通过UNION添加新的分类字段，即先通过分类查询并添加新的分类字段再UNION合并为带分类字段的新表。  
    
*   不同表通过UNION合并的情况如：将一张18年的订单表和19年的订单表纵向合并起来在一张表里展示  
    

**注意**

*   合并的查询结果必须列数相等，否则会报错  
    
*   合并表里的列名由排在 UNION 前面的决定  
    
```sql
-- 给订单表增加一个新字段——status，用以区分今年的订单和今年以前的订单

USE sql_store;

    SELECT 
        order_id,
        order_date,
        'Active' AS status
    FROM orders
    WHERE order_date >= '2019-01-01'

UNION

    SELECT 
        order_id,
        order_date,
        'Archived' AS status  -- Archived 归档
    FROM orders
    WHERE order_date < '2019-01-01';

-- 合并不同表的例子——在同一列里显示所有顾客名以及所有商品名

USE sql_store;

    SELECT first_name AS name_of_all
    -- 新列名由排UNION前面的决定
    FROM customers

UNION

    SELECT name
    FROM products

-- 给顾客按积分大小分类，添加新字段type，并按顾客id排序 (青铜，白银，黄金)

    SELECT 
        customer_id,
        first_name,
        points,
        'Bronze' AS type
    FROM customers 
    WHERE points < 2000

UNION

    SELECT 
        customer_id,
        first_name,
        points,
        'Silver' AS type
    FROM customers 
    WHERE points BETWEEN 2000 and 3000

UNION

    SELECT 
        customer_id,
        first_name,
        points,
        'Gold' AS type
    FROM customers 
    WHERE points > 3000

ORDER BY customer_id
```

可以看出ORDER BY的优先级在UNION之后，应该是排序和限制语句的执行优先级比较靠后。另外，这里如果没有 ORDER BY 的话就会按3个 query 的先后来排序。

**总结**

感觉本质上可以将查询语句的任何一步和任何一个层次，包括：

1.  横纵筛选 `SELECT ……` `WHERE ……`
2.  选表 `FROM ……`
3.  横纵连接 `…… JOIN ……` `…… UNION ……`
4.  排序、限制`ORDER BY ……` `LIMIT ……`

都看作暂时生成了一张新表（虚拟表），将后续步骤都看作是在对这些新表进行进一步的操作， 这样，层次步骤就能理清，就好理解了，也才真的能从本质上掌握并灵活运用

## 第四章、插入、更新和删除数据


第二、三章讲如何 “查询”，其中第二章讲单个表里如何“查询”，第三章讲如何使用多张表“查询”（通过横纵向连接）

这一章讲如何 “增、改、删”

前四章构成了**SQL的基础 “增删改查”**

- **1、列属性**

点击表的扳手按钮：打开设计模式，介绍了一些表中字段/列的属性。

- **2、插入单行**

**小结**

> INSERT INTO 目标表 （目标列，可选，逗号隔开）
> VALUES (目标值，逗号隔开)

**案例**: 在顾客表里插入一个新顾客的信息

法1. 若不指明列名，则插入的值必须按所有字段的顺序完整插入

```sql
USE sql_store;

INSERT INTO customers -- 目标表
VALUES (
    DEFAULT,
    'Michael',
    'Jackson',
    '1958-08-29',  -- DEFAULT/NULL/'1958-08-29'
    DEFAULT,
    '5225 Figueroa Mountain Rd', 
    'Los Olivos',
    'CA',
    DEFAULT
    );
```

法2. 指明列名，可跳过取默认值的列且可更改顺序，一般用这种，更清晰

```sql
INSERT INTO customers (
    address,
    city,
    state,
    last_name,
    first_name,
    birth_date,
    )
VALUES (
    '5225 Figueroa Mountain Rd',
    'Los Olivos',
    'CA',
    'Jackson',
    'Michael',    
    '1958-08-29',  
    )
```

- **3、插入多行**

**小结**

`VALUES ……` 里一行内数据用**括号内逗号**隔开，而多行数据用**括号间逗号**隔开

**案例**
```sql
-- 插入多条产品信息

USE sql_store;

INSERT INTO products 
VALUES (DEFAULT, 'product1', 1, 10),
       (DEFAULT, 'product2', 2, 20),
       (DEFAULT, 'product3', 3, 30)

-- 或
INSERT INTO products (name, quantity_in_stock, unit_price)
VALUES ('product1', 1, 10),
       ('product2', 2, 20),
       ('product3', 3, 30)
```

还是感觉后面这种指明列名的要清晰一点

**注意**

对于AI (Auto Incremental 自动递增) 的id字段，MySQL会记住删除的/用过的id，并在此基础上递增

- **4、插入分级行**

**小结**

订单表（orders表）里的一条记录对应订单项目表（order_items表）里的多条记录，一对多，是相互关联的父子表。通过添加一条订单记录和对应的多条订单项目记录，学习如何向父子表插入分级（层）/耦合数据（insert hierarchical data）：

*   关键：在插入子表记录时，需要用内建函数 `LAST_INSERT_ID()` 获取相关父表记录的自增ID (这个例子中就是 `order_id` )
*   内建函数：MySQL里有很多可用的内置函数，也就是可复用的代码块，各有不同的功能，注意函数名的单词之间用下划线连接
*   `LAST_INSERT_ID()`：获取最新的成功的 `INSERT 语句` 中的自增id，在这个例子中就是父表里新增的 `order_id`

**案例**

新增一个订单（order），里面包含两个订单项目/两种商品（`order_items`），请同时更新订单表和订单项目表

```sql
USE sql_store;

INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-01', 1);

-- 可以先试一下用 SELECT last_insert_id() 看能否成功获取到的最新的 order_id

INSERT INTO order_items  -- 全是必须字段，就不用指定了
VALUES 
    (last_insert_id(), 1, 2, 2.5),
    (last_insert_id(), 2, 5, 1.5)
```

- **5、创建表的副本**

**小结**

`DROP TABLE 要删的表名`、`CREATE TABLE 新表名 AS 子查询`

`TRUCATE '要清空的表名'`、`INSERT INTO 表名 子查询`

子查询里当然也可以用WHERE语句进行筛选

**案例 1**

运用 `CREAT TABLE 新表名 AS 子查询` 快速创建表 orders 的副本表 `orders_archived`

```sql
USE sql_store;

CREATE TABLE orders_archived AS
    SELECT * FROM orders  -- 子查询
```

`SELECT * FROM orders` 选择了 oders 中所有数据，作为AS的内容，是一个子查询

*   子查询： 任何一个充当另一个SQL语句的一部分的 `SELECT……` 查询语句都是子查询，子查询是一个很有用的技巧。

**注意**

创建已有的表或删除不存在的表的话都会报错，所以建表和删表语句都最好加上条件语句（后面会讲）

**案例 2**

不再用全部数据，而选用原表中部分数据创建副本表，如，用今年以前的 orders 创建一个副本表 `orders_archived`，其实就是在子查询里增加了一个WHERE语句进行筛选。注意要先 drop 删掉 或 truncate 清空掉之前建的 `orders_archived` 表再重建或重新插入数据。

法1. `DROP TABLE 要删的表名`、`CREATE TABLE 新表名 AS 子查询`

```sql
USE sql_store;

DROP TABLE orders_archived;  -- 也可右键该表点击 drop
CREATE TABLE orders_archived AS
    SELECT * FROM orders
    WHERE order_date < '2019-01-01'
```

法2. `TRUCATE '要清空的表名'`、`INSERT INTO 表名 子查询`

`INSERT INTO 表名 子查询` 很常用，子查询替代原先插入语句中 `VALUES(……,……),(……,……),……` 的部分

```sql
TRUNCATE 'orders_archived'; -- 也可右键该表点击 truncate  
INSERT INTO orders_archived  
-- 不用指明列名，会直接用子查询表里的列名
    SELECT * FROM orders  
    -- 子查询，替代原先插入语句中VALUES(……,……),(……,……),…… 的部分
    WHERE order_date < '2019-01-01'
```

**练习**

创建一个存档发票表，只包含有过支付记录的发票并将顾客id换成顾客名字

构建的思路顺序：

1、先创建子查询，确定新表内容：

- A. 合并发票表和顾客表

- B. 筛选支付记录不为空的行/记录

- C. 筛选（并重命名）需要的列

2、第1步得到的查询内容，可以先运行看一下，确保准确无误后，再作为子查询内容存入新创建的副本订单存档表 `CREATE TABLE 新表名 AS 子查询`

```sql
USE sql_invoicing;

DROP TABLE invoices_archived;  

CREATE TABLE invoices_archived AS
    SELECT i.invoice_id, c.name AS client, i.payment_date  
    -- 为了简化，就选这三列
    FROM invoices i
    JOIN clients c
        USING (client_id)
    WHERE i.payment_date IS NOT NULL
    -- 或者 i.payment_total > 0
```

- **6、更新单行**

**小结**

用 `UPDATE ……` 语句 来修改表中的一条或多条记录，具体语法结构：

    UPDATE 表 
    SET 要修改的字段 = 具体值/NULL/DEFAULT/列间数学表达式 （修改多个字段用逗号分隔）
    WHERE 行筛选

```sql
USE sql_invoicing;

UPDATE invoices
SET 
    payment_total = 100 / 0 / DEFAULT / NULL / 0.5 * invoice_total, 
    /*注意 0.5 * invoice_total 的结果小数部分会被舍弃，
    之后讲数据类型会讲到这个问题*/
    payment_date = '2019-01-01' / DEFAULT / NULL / due_date
WHERE invoice_id = 3
```

- **7、更新多行**

**小结**

语法一样的，就是让 `WHERE……` 的条件包含更多记录，就会同时更改多条记录了

```sql
USE sql_invoicing;

UPDATE invoices
SET payment_total = 233, payment_date = due_date
WHERE client_id = 3  
-- 该客户的发票记录不止一条，将同时更改
/WHERE client_id IN (3, 4) 
-- 第二章 4~9 讲的那些写 WHERE 条件的方法均可用
-- 甚至可以直接省略 WHERE 语句，会直接更改整个表的全部记录


-- 让所有非90后顾客的积分增加50点

USE sql_store;

UPDATE customers
SET points = points + 50
WHERE birth_date < '1990-01-01'
```

- **8、在Updates中用子查询**

**小结**

非常有用，其实本质上是将子查询用在 `WHERE……` 行筛选条件中

**注意**

1.  括号的使用
2.  `IN ……` 后除了可接 `（……, ……）` 也可接由子查询得到的多个数据（一列多条数据），感觉和前面 `VALUES` 后可接子查询道理是相通的

```sql
-- 更改发票记录表中名字叫 Yadel 的记录，但该表只有 client_id，故先要从另一个顾客表中查询叫 Yadel 人的 client_id

USE sql_invoicing;

UPDATE invoices
SET payment_total = 567, payment_date = due_date

WHERE client_id = 
    (SELECT client_id 
    FROM clients
    WHERE name = 'Yadel');
    -- 放入括号，确保先执行

-- 若子查询返回多个数据（一列多条数据）时就不能用等号而要用 IN 了：
WHERE client_id IN 
    (SELECT client_id 
    FROM clients
    WHERE state IN ('CA', 'NY'))
```


**最佳实践**

Update 前，最好先验证看一看子查询以及WHERE行筛选条件是不是准确的，筛选出的是不是我们的修改目标， 确保不会改错记录，再套入 UPDATE SET 语句更新，如上面那个就可以先验证子查询：

    SELECT client_id 
    FROM clients
    WHERE state IN ('CA', 'NY')

以及验证WHERE行筛选条件（即先不UPDATE，先SELECT，改之前，先看一看要改的目标选对了没）

    SELECT *
    FROM invoices
    WHERE client_id IN (
        SELECT client_id 
        FROM clients
        WHERE state IN ('CA', 'NY')
    )

确保WHERE行筛选条件准确准确无误后，再放到修改语句后执行修改：

    UPDATE invoices
    SET payment_total = 567, payment_date = due_date
    WHERE client_id IN (
        SELECT client_id 
        FROM clients
        WHERE state IN ('CA', 'NY')

**练习**

将 orders 表里那些 分数>3k 的用户的订单 comments 改为 'gold customer'

思考步骤：

1.  WHERE 行筛选出要求的顾客
2.  SELECT 列筛选他们的id
3.  将前两步 作为子查询 用在修改语句中的 WHERE 条件中，执行修改

```sql
USE sql_store;

UPDATE orders
SET comments = 'gold customer'
WHERE customer_id IN
    (SELECT customer_id
    FROM customers
    WHERE points > 3000)
```

- **9、删除行**

**小结**

语法结构：

    DELETE FROM 表 
    WHERE 行筛选条件
    （当然也可用子查询）
    （若省略 WHERE 条件语句会删除表中所有记录（和 TRUNCATE 等效？））

```sql
-- 选出顾客id为3/顾客名字叫'Myworks'的发票记录

USE sql_invoicing;

DELETE FROM invoices
WHERE client_id = 3
-- WHERE可选，省略就是会删除整个表的所有行/记录
/WHERE client_id = 
    (SELECT client_id  
    /*Mosh 错写成了 SELECT *，将报错：
    Error Code: 1241. Operand should contain 1 column(s) 
    Operand n. [计] 操作数；[计] 运算对象；运算元 */
    FROM clients
    WHERE name = 'Myworks')
```

# 第二部分：基础进阶——汇总、复杂查询、内置函数

## 第五章、汇总数据

汇总统计型查询非常有用，甚至可能常常是你的主要工作内容

- **1、聚合函数**

**小结**

聚合函数：**输入一系列值并聚合为一个结果**的函数

```sql
USE sql_invoicing;

SELECT 
    MAX(invoice_date) AS latest_date,  
    -- SELECT选择的不仅可以是列，也可以是数字、列间表达式、列的聚合函数
    MIN(invoice_total) lowest,
    AVG(invoice_total) average,
    SUM(invoice_total * 1.1) total,
    COUNT(*) total_records,
    COUNT(invoice_total) number_of_invoices, 
    -- 和上一个相等
    COUNT(payment_date) number_of_payments,  
    -- 【聚合函数会忽略空值】，得到的支付数少于发票数
    COUNT(DISTINCT client_id) number_of_distinct_clients
    -- DISTINCT client_id 筛掉了该列的重复值，再COUNT计数，会得到不同顾客数
FROM invoices
WHERE invoice_date > '2019-07-01'  -- 想只统计下半年的结果
```


**练习**

目标：

| date\_range         | total\_sales | total\_payments | what\_we\_expect (the difference) |
| ------------------- | ------------ | --------------- | --------------------------------- |
| 1st\_half\_of\_2019 |              |                 |                                   |
| 2nd\_half\_of\_2019 |              |                 |                                   |
| Total               |              |                 |                                   |

思路：很明显要 分类子查询+聚合函数+UNION

```sql
USE sql_invoicing;

    SELECT 
        '1st_half_of_2019' AS date_range,
        SUM(invoice_total) AS total_sales,
        SUM(payment_total) AS total_payments,
        SUM(invoice_total - payment_total) AS what_we_expect
    FROM invoices
    WHERE invoice_date BETWEEN '2019-01-01' AND '2019-06-30'

UNION

    SELECT 
        '2st_half_of_2019' AS date_range,
        SUM(invoice_total) AS total_sales,
        SUM(payment_total) AS total_payments,
        SUM(invoice_total - payment_total) AS what_we_expect
    FROM invoices
    WHERE invoice_date BETWEEN '2019-07-01' AND '2019-12-31'

UNION

    SELECT 
        'Total' AS date_range,
        SUM(invoice_total) AS total_sales,
        SUM(payment_total) AS total_payments,
        SUM(invoice_total - payment_total) AS what_we_expect
    FROM invoices
    WHERE invoice_date BETWEEN '2019-01-01' AND '2019-12-31'
```

- **2、GROUP BY子句**

**小结**

按一列或多列分组，注意语句的位置。

**案例1：按一个字段分组**

```sql
在发票记录表中**按不同顾客分组统计**下半年总销售额并降序排列

USE sql_invoicing;

SELECT 
    client_id,  
    SUM(invoice_total) AS total_sales
    ……
FROM invoices
WHERE invoice_date >= '2019-07-01'  -- 筛选，过滤器
GROUP BY client_id  -- 分组
ORDER BY invoice_total DESC
```

只有聚合函数是按 client\_id 分组时，这里选择 client\_id 列才有意义（**分组统计语句里SELECT通常都是选择分组依据列+目标统计列的聚合函数，选别的列没意义**）。若未分类，结果会是一条总 total\_sales 和一条 client\_id（该client\_id无意义），即 client\_id 会被压缩为只显示一条而非 SUM 广播为多条，可以理解为聚合函数比较强势吧。

若省略排序语句就会默认按分组依据排序

记住语句顺序很重要 WHERE GROUP BY ORDER BY，分组语句在排序语句之前，调换顺序会报错

**案例2：按多个字段分组**

```sql
-- 算各州各城市的总销售额
USE sql_invoicing;

SELECT 
    state,
    city,
    SUM(invoice_total) AS total_sales
FROM invoices
JOIN clients USING (client_id) -- 别忘了USING之后是括号，太容易忘了
GROUP BY state, city  
-- 逗号分隔就行
-- 这个例子里 GROUP BY 里去掉 state 结果一样
ORDER BY state
```

其实上面的例子里一个城市只能属于一个州中，所有归根结底还是算的各城市的销售额，`GROUP BY ……` 里去掉 state 只写 city （但 SELECT 和 ORDER BY 里保留 state）结果是完全一样的（包括结果里的 state 列），下面这个例子更能说明以多个字段为分组依据进行分组统计的意义

```sql
-- 在 payments 表中，按日期和支付方式分组统计总付款额
USE sql_invoicing;

SELECT 
    date, 
    pm.name AS payment_method,
    SUM(amount) AS total_payments
FROM payments p
JOIN payment_methods pm
    ON p.payment_method = pm.payment_method_id
GROUP BY date, payment_method
-- 用的是 SELECT 里的列别名
ORDER BY date
```

每个分组显示一个日期和支付方式的独立组合，可以看到某特定日期特定支付方式的总付款额。这个例子里每一种支付方式可以在不同日子里出现，每一天也可以出现多种支付方式，这种情况，才叫真·多字段分组。不过上一个例子里那种假·多字段分组，把 state 加在分组依据里也没坏处还能落个心安，也还是加上别省比较好

**3、HAVING子句**

**小结**

HAVING 和 WHERE 都是是条件筛选语句，条件的写法相通，数学、比较（包括特殊比较）、逻辑运算都可以用（如 AND、REGEXP 等等）

```sql
-- 筛选出总发票金额大于500且总发票数量大于5的顾客
USE sql_invoicing;

SELECT 
    client_id,
    SUM(invoice_total) AS total_sales,
    COUNT(*/invoice_total/invoice_date) AS number_of_invoices
FROM invoices
GROUP BY client_id
HAVING total_sales > 500 AND number_of_invoices > 5
-- 均为 SELECT 里的列别名
```

若写：`WHERE total_sales > 500 AND number_of_invoices > 5`，会报错：Error Code: 1054. Unknown column 'total\_sales' in 'where clause'

**练习**

在 sql\_store 数据库（有顾客表、订单表、订单项目表等）中，找出在 'VA' 州且消费总额超过100美元的顾客（这是一个面试级的问题，还很常见）

思路：  
1\. 需要的信息在顾客表、订单表、订单项目表三张表中，先将三张表合并  
2\. WHERE 事前筛选 'VA' 州的  
3\. 按顾客分组，并选取所需的列并聚合得到每位顾客的付款总额  
4\. HAVING 事后筛选超过 100美元 的

```sql
USE sql_store;

SELECT 
    c.customer_id,
    c.first_name,
    c.last_name,
    SUM(oi.quantity * oi.unit_price) AS total_sales
FROM customers c
JOIN orders o USING (customer_id)  -- 别忘了括号，特容易忘
JOIN order_items oi USING (order_id)
WHERE state = 'VA'
GROUP BY 
    c.customer_id, 
    c.first_name, 
    c.last_name
HAVING total_sales > 100
```

**补充**

**学第六章第6节时发现，当 HAVING 筛选的是聚合函数时，该聚合函数可以不在SELECT里显性出现。（作为一种需要记住的特殊情况）**如：下面这两种写法都能筛选出总点数大于3k的州，如果不要求显示总点数，应该用后一种

```sql
SELECT state, SUM(points)
FROM customers
GROUP BY state
HAVING SUM(points) > 3000

-- 或
SELECT state
FROM customers
GROUP BY state
HAVING SUM(points) > 3000
```

**4、ROLLUP运算符**

`GROUP BY …… WITH ROLL UP` 自动汇总型分组，若是多字段分组的话汇总也会是多层次的，注意这是**MySQL扩展语法**，不是SQL标准语法。

MySQL的ROLLUP运算符是用于在GROUP BY查询中生成汇总行的特殊操作符。它可以在查询结果中添加一行或多行，用于显示每个分组的总计或部分总计。使用ROLLUP运算符时，需要在GROUP BY子句中指定要分组的字段，并在SELECT子句中使用ROLLUP()函数指定需要进行汇总的字段。汇总行的标识是NULL

```sql
-- 例如，假设有一个表格sales包含以下字段：product，region，month，和amount。我们希望按照product、region和month进行分组，并对amount字段进行求和，并在最后显示总计和部分总计
SELECT product, region, month, SUM(amount) AS total_amount
FROM sales
GROUP BY product, region, month WITH ROLLUP;

-- 上述查询将生成一个包含总计和部分总计的结果集。例如：

product    | region    | month      | total_amount
----------------------------------------------
product_1  | region_A  | Jan        | 100
product_1  | region_A  | Feb        | 150
product_1  | region_A  | NULL       | 250        (region_A总计)
product_1  | region_B  | Mar        | 200
product_1  | region_B  | NULL       | 200        (region_B总计)
product_1  | NULL      | NULL       | 450        (product_1总计)
product_2  | region_C  | Jan        | 50
product_2  | region_C  | NULL       | 50         (region_C总计)
product_2  | NULL      | NULL       | 50         (product_2总计)
NULL       | NULL      | NULL       | 500        (所有产品总计)
```

```sql
-- 分组查询各客户的发票总额以及所有人的总发票额
USE sql_invoicing;

SELECT 
    client_id,
    SUM(invoice_total)
FROM invoices
GROUP BY client_id WITH ROLLUP

-- 多字段分组 例1：分组查询各州、市的总销售额（发票总额）以及州层次和全国层次的两个层次的汇总额
SELECT 
    state,
    city,
    SUM(invoice_total) AS total_sales
FROM invoices
JOIN clients USING (client_id) 
GROUP BY state, city WITH ROLLUP

-- 多字段分组 例2：分组查询特定日期特定付款方式的总支付额以及单日汇总和整体汇总
SELECT 
    date, 
    pm.name AS payment_method,
    SUM(amount) AS total_payments
FROM payments p
JOIN payment_methods pm
    ON p.payment_method = pm.payment_method_id
GROUP BY date, pm.name WITH ROLLUP
```

**★总结**

![](https://img-blog.csdnimg.cn/20190909144440851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3ODg4NjQ4NA==,size_16,color_FFFFFF,t_70)


"SELECT 是在大部分语句执行了之后才执行的，严格的说是在 FROM、WHERE 和 GROUP BY （以及 HAVING）之后执行的。理解这一点是非常重要的，**这就是你不能在 WHERE 中使用在 SELECT 中设定别名的字段作为判断条件的原因。**"

这个顺序可以由下面这个例子的缩进表现出来（出右往左）（注意 DISTINCT 放不进去了只有以注释的形式展示出来，另外 SELECT 还是选择放在了 HAVING 之前）

```sql
                    SELECT name, SUM(invoice_total) AS total_sales
         -- DISTINCT
                                FROM invoices JOIN clients USING (client_id) 
                            WHERE due_date < '2019-07-01'
                        GROUP BY name  
                HAVING total_sales > 150

        UNION

                    SELECT name, SUM(invoice_total) AS total_sales
         -- DISTINCT
                                FROM invoices JOIN clients USING (client_id) 
                            WHERE due_date > '2019-07-01'
                        GROUP BY name  
                HAVING total_sales > 150

    ORDER BY total_sales
LIMIT 2
```

**关于 SELECT 的位置**

1.  如后面几篇参考文章所说，**按标准 SQL 的执行顺序， SELECT 是在 HAVING 之后**
2.  但根据前面的内容，**似乎在 MySQL 里，SELECT 的执行顺序是在 WHERE GROUP BY 之后，而在 HAVING 之前 —— 因而 WHERE GROUP BY 要用原列名（后来发现只有 WHERE 里必须用原列名，GROUP BY 是原列名或列别名都可用（甚至可以用1，2来指代 SELECT 中的列，不过 Mosh 不建议这样做））而 HAVING 必须用 SELECT 里的列别名（聚合函数除外）**

**按实践经验来看，就按 2 来记忆和理解是可行的**，但之后最好还是要去看书看资料把这个执行顺序的疑惑彻底搞清楚，这个还挺重要的。

## 第六章、编写复杂查询

- **1、介绍**

主要是**子查询**（或者说 **嵌套查询**），有些前面已经讲过

- **2、子查询**

**回顾**

子查询： 任何一个充当另一个SQL语句的一部分的 SELECT…… 查询语句都是子查询，子查询是一个很有用的技巧。子查询的层级用括号实现。

**注意**

另外发现各种语言，各种语句，各种逻辑结构，各种情形下一般好像多加括号都不会有问题，只有少加括号才会出问题，所以不确定执行顺序时最好加上括号确保万无一失。


```sql
-- 在 products 中，找到所有比生菜（id = 3）价格高的
USE sql_store;

SELECT *
FROM products
WHERE unit_price > (
    SELECT unit_price
    FROM products
    WHERE product_id = 3
)
```
关键：**用子查询找到生菜价格**。MySQL执行时会先执行括号内的子查询（内查询），将获得的生菜价格作为结果返回给外查询

子查询不仅可用在 WHERE …… 中，也可用在 SELECT …… 或 FROM …… 等子句中

```sql
-- 在 sql_hr 库 employees 表里，选择所有工资超过平均工资的雇员
USE sql_hr;

SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
)
```

- **3、IN运算符**

**案例**

在 sql\_store 库 products 表中找出那些从未被订购过的产品

思路：  
1\. order\_items 表里有所有产品被订购的记录，用 DISTINCT 去重，得到所有被订购过的产品列表  
2\. 不在这列表里（NOT IN 的使用）的产品即为从未被订购过的产品

```sql
USE sql_store;

SELECT *
FROM products
WHERE product_id NOT IN (
    SELECT DISTINCT product_id
    FROM order_items
)
```

上一节是子查询返回一个值（平均工资），这一节是返回一列数据（被订购过的产品id列表），之后还会用子查询返回一个多列的表

- **4、子查询 vs 连接**

**小结**

子查询（Subquery）是将一张表的查询结果作为另一张表的查询依据并层层嵌套，其实也可以先将这些表链接（Join）合并成一个包含所需全部信息的详情表再直接在详情表里筛选查询。两种方法一般是可互换的，具体用哪一种取决于 **效率/性能（Performance） 和 可读性（readability）**，之后会学习 执行计划，到时候就知道怎样编写并更快速地执行查询，现在主要考虑可读性

```sql
-- 找出从未订购（没有invoices）的顾客：

-- 法1. 子查询。先用子查询查出有过发票记录的顾客名单，作为筛选依据
USE sql_invoicing;

SELECT *
FROM clients
WHERE client_id NOT IN (
    SELECT DISTINCT client_id
    /*其实这里加不加DISTINCT对子查询返回的结果有影响
    但对最后的结果其实没有影响*/
    FROM invoices
)

-- 法2. 链接表。用顾客表 LEFT JOIN 发票记录表，再直接在这个合并详情表中筛选出没有发票记录的顾客
USE sql_invoicing;

SELECT DISTINCT client_id, name …… 
-- 不能SELECT DISTINCT *
FROM clients
LEFT JOIN invoices USING (client_id)
-- 注意不能用内链接，否则没有发票记录的顾客（我们的目标）直接就被筛掉了
WHERE invoice_id IS NULL
```

就上面这个案例而言，子查询可读性更好，但有时子查询会过于复杂（嵌套层数过多），用链接表更好。总之在选择方法时，可读性是很重要的考虑因素

```sql
-- 在 sql_store 中，选出买过生菜（id = 3）的顾客的id、姓和名

-- 法1. 完全子查询
USE sql_store;

SELECT customer_id, first_name, last_name
FROM customers
WHERE customer_id IN (  
    -- 子查询2：从订单表中找出哪些顾客买过生菜
    SELECT customer_id
    FROM orders
    WHERE order_id IN (  
        -- 子查询1：从订单项目表中找出哪些订单包含生菜
        SELECT DISTINCT order_id
        FROM order_items
        WHERE product_id = 3
    )
)

-- 法2. 混合：子查询 + 表连接
USE sql_store;

SELECT customer_id, first_name, last_name
FROM customers
WHERE customer_id IN (  
    -- 子查询：哪些顾客买过生菜
    SELECT customer_id
    FROM orders
    JOIN order_items USING (order_id)  
    -- 表连接：合并订单和订单项目表得到 订单详情表
    WHERE product_id = 3
)


-- 法3. 完全表连接

-- 直接链接合并3张表（顾客表、订单表和订单项目表）得到 带顾客信息的订单详情表，该合并表包含我们所需的所有信息，可直接在合并表中用WHERE筛选买过生菜的顾客（注意 DISTINCT 关键字的运用）。

USE sql_store;

SELECT DISTINCT customer_id, first_name, last_name
FROM customers
LEFT JOIN orders USING (customer_id)
LEFT JOIN order_items USING (order_id)
WHERE product_id = 3
```
这个案例中，先将所需信息所在的几张表全部连接合并成一张大表再来查询筛选明显比层层嵌套的多重子查询更加清晰明了


- **5、ALL关键字**

**小结**

`> (MAX (……))` 和 `> ALL(……)` 等效可互换

“比这里面最大的还大” = “比这里面的所有的都大”

```sql
-- sql_invoicing 库中，选出金额大于3号顾客所有发票金额（或3号顾客最大发票金额） 的发票

-- 法1. 用MAX关键字
USE sql_invoicing;

SELECT *
FROM invoices
WHERE invoice_total > (
    SELECT MAX(invoice_total)
    FROM invoices
    WHERE client_id = 3
)

-- 法2. 用ALL关键字
USE sql_invoicing;

SELECT *
FROM invoices
WHERE invoice_total > ALL (
    SELECT invoice_total
    FROM invoices
    WHERE client_id = 3
)
```

其实就是把内层括号的MAX拿到了外层括号变成ALL：  
MAX法是用MAX()返回一个顾客3的最大订单金额，再判断哪些发票的金额比这个值大；  
ALL法是先返回顾客3的所有订单金额，是一列值，再用ALL()判断比所有这些金额都大的发票有哪些。  
两种方法是完全等效的

- **6、ANY关键字**

**小结**

`> ANY/SOME (……)` 与 `> (MIN (……))` 等效

`= ANY/SOME (……)` 与 `IN (……)` 等效

```sql
-- `> ANY (……)` 与 `> (MIN (……))` 等效的例子：  
-- sql_invoicing 库中，选出金额大于3号顾客任何发票金额（或最小发票金额） 的发票

USE sql_invoicing;

SELECT *
FROM invoices

WHERE invoice_total > ANY (
    SELECT invoice_total
    FROM invoices
    WHERE client_id = 3
)

-- 或

WHERE invoice_total > (
    SELECT MIN(invoice_total)
    FROM invoices
    WHERE client_id = 3
)

-- `= ANY (……)` 与 `IN (……)` 等效的例子:  
-- 选出至少有两次发票记录的顾客

USE sql_invoicing;

SELECT *
FROM clients
WHERE client_id IN (  -- 或 = ANY ( 
    -- 子查询：有2次以上发票记录的顾客
    SELECT client_id
    FROM invoices
    GROUP BY client_id
    HAVING COUNT(*) >= 2
)

```

- **7、相关子查询**

**小结**

之前都是非关联主/子（外/内）查询，比如子查询先查出整体的某平均值或满足某些条件的一列id，作为主查询的筛选依据，这种子查询与主查询无关，会先一次性得出查询结果再返回给主查询供其使用。

而下面这种相关联子查询例子里，子查询要查询的是某员工所在办公室的平均值，子查询是依赖主查询的，**注意这种关联查询是在主查询的每一行/每一条记录层面上依次进行的**，这一点可以为我们写关联子查询提供线索（注意表别名的使用），另外也正因为这一点，**相关子查询会比非关联查询执行起来慢一些**

**案例**

选出 sql\_hr.employees 里那些工资超过**他所在**办公室平均工资（而不是整体平均工资）的员工  
关键：如何查询目前主查询员工的所在办公室的平均工资而不是整体的平均工资？  
思路：给主查询 employees表 设置别名 e，这样在子查询查询平均工资时加上 `WHERE office_id = e.office_id` 筛选条件即可相关联地查询到目前员工所在地办公室的平均工资

```sql
USE sql_hr;

SELECT *
FROM employees e  -- 关键 1
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE office_id = e.office_id  -- 关键 2
    -- 【子查询表字段不用加前缀，主查询表的字段要加前缀，以此区分】
)


-- 在 sql_invoicing 库 invoices 表中，找出高于每位顾客平均发票金额的发票

USE sql_invoicing;

SELECT *
FROM invoices i
WHERE  invoice_total > (
    -- 子查询：目前客户的平均发票额
    SELECT AVG(invoice_total)
    FROM invoices
    WHERE client_id = i.client_id
)
```

- **8、EXISTS运算符**

**小结**

`IN + 子查询` 等效于 `EXIST + 相关子查询`，如果**前者子查询的结果集过大占用内存，用后者逐条验证更有效率**。另外 EXIST() 本质上是根据是否为空返回 TRUE 和 FALSE，所以也可以加 NOT 取反

**案例**

```sql
-- 找出有过发票记录的客户，第4节学过用子查询或表连接来实现

-- 法1. 子查询
USE sql_invoicing;

SELECT *
FROM clients
WHERE client_id IN (
    SELECT DISTINCT client_id
    FROM invoices
)

-- 法2. 链接表。内链接，只留下有过发票记录的客户
USE sql_invoicing;

SELECT DISTINCT client_id, name …… 
FROM clients
JOIN invoices USING (client_id)

-- 第3种方法是用EXISTS运算符实现
USE sql_invoicing;

SELECT *
FROM clients c
WHERE EXISTS (
    SELECT */client_id  
    /* 就这个子查询的目的来说，SELECT的选择不影响结果，
    因为EXISTS()函数只根据是否为空返回 TRUE 和 FALSE */
    FROM invoices
    WHERE client_id = c.client_id
)
```

这还是个相关子查询，因为在其中引用了主查询的 clients 表。这同样是按照主查询的记录一条条验证执行的。具体说来，对于 clients 表（设置别名为 c）里的每一个顾客，子查询在 invoices 表查找这个人的发票记录（ 即 client\_id = c.client\_id 的发票记录），有就返回相关记录否者返回空，然后 EXISTS() 根据是否为空得到 TRUE 和 FALSE（表示此人有无发票记录），然后主查询凭此确定是否保留此条记录。

对比一下，法1是用子查询返回一个有发票记录的顾客id列表，如（1，3，8 ……），然后用IN运算符来判断，**如果子查询表太大，可能返回一个上百万千万甚至上亿的id列表，这个id列表就会很占内存非常影响性能**，对于这种子查询会返回一个很大的结果集的情况，用这里的**EXIST+相关子查询逐条筛选会更有效率**

另外，因为 SELECT() 返回的是 TRUE/FALSE，所以自然也可以加上NOT取反

```sql
-- 在sql_store中，找出从来没有被订购过的产品。

USE sql_store;

SELECT *
FROM products 
WHERE product_id NOT IN (
    SELECT product_id 
    -- 加不加DISTINCT对最终结果无影响
    FROM order_items
)

-- 或

SELECT *
FROM products p
WHERE NOT EXISTS (
    SELECT *
    FROM order_items
    WHERE product_id = p.product_id
)
```

对于亚马逊这样的大电商来说，如果用IN+子查询法，子查询可能会返回一个百万量级的产品id列表，这种情况还是用EXIST+相关子查询逐条验证法更有效率

- **9、SELECT子句的子查询**

**小结**

不仅 WHERE 筛选条件里可以用子查询，SELECT 选择子句和 FROM 来源表子句也能用子查询，这节课讲 SELECT 子句里的子查询

简单讲就是，SELECT选择语句是用来确定查询结果选择包含哪些字段，每个字段都可以是一个表达式，而每个字段表达式里的元素除了可以是原始的列，具体的数值，也同样可以是其它各种花里胡哨的子查询的结果

任何子查询都是简单查询的嵌套，没什么新东西，只是多了一个层级而已，由内向外地一层层梳理就很清楚

要特别注意记住以子查询方式实现在SELECT中使用同级列别名的方法

**案例**

得到一个有如下列的表格：invoice\_id, invoice\_total, avarege（总平均发票额）, difference（前两个值的差）

```sql
USE sql_invoicing;

SELECT 
    invoice_id,
    invoice_total,
    (SELECT AVG(invoice_total) FROM invoices) AS invoice_average,
    /*不能直接用聚合函数，因为“比较强势”，会压缩聚合结果为一条
    用括号+子查询(SELECT AVG(invoice_total) FROM invoices) 
    将其作为一个数值结果 152.388235 加入主查询语句*/
    invoice_total - (SELECT invoice_average) AS difference
    /*SELECT表达式里要用原列名，不能直接用别名invoice_average
    要用列别名的话用子查询（SELECT 同级的列别名）即可
    说真的，感觉这个子查询有点难以理解，但记住会用就行*/
FROM invoices
```

**练习**

得到一个有如下列的表格：client\_id, name, total\_sales（各个客户的发票总额）, average（总平均发票额）, difference（前两个值的差）

```sql
USE sql_invoicing;

SELECT 
    client_id,
    name,
    (SELECT SUM(invoice_total) FROM invoices WHERE client_id = c.client_id) AS total_sales,
    -- 要得到【相关】客户的发票总额，要用相关子查询 WHERE client_id = c.client_id
    (SELECT AVG(invoice_total) FROM invoices) AS average,
    (SELECT total_sales - average) AS difference   
    /* 如前所述，引用同级的列别名，要加括号和 SELECT，
    和前两行子查询的区别是，引用同级的列别名不需要说明来源，
    所以没有 FROM …… */
FROM clients c
```

注意第四个客户的 total\_sales 和 difference 都是空值 null

- **10、FROM子句的子查询**

**小结**

子查询的结果同样可以充当一个“虚拟表”作为FROM语句中的来源表，即将筛选查询结果作为来源再进行进一步的筛选查询。但注意只有在子查询不太复杂时进行这样的嵌套，否则最好用后面讲的视图先把子查询结果储存起来再使用。

**案例**

将上一节练习里的查询结果当作来源表，查询其中 total\_sales 非空的记录

```sql
USE sql_invoicing;

SELECT * 
FROM (
    SELECT 
        client_id,
        name,
        (SELECT SUM(invoice_total) FROM invoices WHERE client_id = c.client_id) AS total_sales,
        (SELECT AVG(invoice_total) FROM invoices) AS average,
        (SELECT total_sales - average) AS difference   
    FROM clients c
) AS sales_summury
/* 在FROM中使用子查询，即使用 “派生表” 时，
必须给派生表取个别名（不管用不用），这是硬性要求，不写会报错：
Error Code: 1248. Every derived table（派生表、导出表）
must have its own alias */
WHERE total_sales IS NOT NULL
```

复杂的子查询再嵌套进 FROM 里会让整个查询看起来过于复杂，上面这个最好是将子查询结果储存为叫 sales\_summury 的视图，然后再直接使用该视图作为来源表，之后会讲。

## 第七章、MySQL的基本函数

内置的用来处理数值、文本、日期等的函数

- **1、数值函数**

**小结**

主要介绍最常用的几个数值函数：ROUND、TRUNCATE、CEILING、FLOOR、ABS、RAND

查看MySQL全部数值函数可**谷歌 'mysql numeric function'**，第一个就是官方文档。

```sql
SELECT ROUND(5.7365, 2)  -- 四舍五入
SELECT TRUNCATE(5.7365, 2)  -- 截断
SELECT CEILING(5.2)  -- 天花板函数，大于等于此数的最小整数
SELECT FLOOR(5.6)  -- 地板函数，小于等于此数的最大整数
SELECT ABS(-5.2)  -- 绝对值
SELECT RAND()  -- 随机函数，0到1的随机值
```

- **2、字符串函数**

**小结**

依然介绍最常用的字符串函数：  
1. LENGTH, UPPER, LOWER  
2. TRIM, LTRIM, RTRIM  
3. LEFT, RIGHT, SUBSTRING  
4. LOCATE, REPLACE, 【CONCAT】

查看全部搜索关键词 **'mysql string functions'**

- 长度、转大小写：

```sql
SELECT LENGTH('sky')  -- 字符串字符个数/长度（LENGTH）
SELECT UPPER('sky')  -- 转大写
SELECT LOWER('Sky')  -- 转小写
```

- 用户输入时时常多打空格，下面三个函数用于处理/修剪（trim）字符串前后的空格，L、R 表示 LEFT、RIGHT：

```sql
SELECT LTRIM('  Sky')
SELECT RTRIM('Sky  ')
SELECT TRIM(' Sky ')
```

- 切片：

```sql
-- 取左边，取右边，取中间
SELECT LEFT('Kindergarden', 4)  -- 取左边（LEFT）4个字符
SELECT RIGHT('Kindergarden', 6)  -- 取右边（RIGHT）6个字符
SELECT SUBSTRING('Kindergarden', 7, 6)  
-- 取中间从第7个开始的长度为6的子串（SUBSTRING）
-- 注意是从第1个（而非第0个）开始计数的
-- 省略第3参数（子串长度）则一直截取到最后
```

- 定位：

```sql
SELECT LOCATE('gar', 'Kindergarden')  -- 定位（LOCATE）首次出现的位置
-- 没有的话返回0（其他编程语言大多返回-1，可能因为索引是从0开始的）
-- 这个定位/查找函数依然是不区分大小写的
```

- 替换：

```sql
SELECT REPLACE('Kindergarten', 'garten', 'garden')
```

- 连接：

```sql
USE sql_store;

SELECT CONCAT(first_name, ' ', last_name) AS full_name
-- concatenate v. 连接
FROM customers
```

- **3、MySQL中的日期函数**

**小结**

本节学基本的处理时间日期的函数，下节课学日期时间的格式化

1.  `NOW, CURDATE, CURTIME`
2.  `YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, DAYNAME, MONTHNAME`
3.  `EXTRACT`(单位 FROM 日期时间对象)， 如 `EXTRACT(YEAR FROM NOW())`

**实例**

- 当前时间

```sql
SELECT NOW()  -- 2020-09-12 08:50:46
SELECT CURDATE()  -- current date, 2020-09-12
SELECT CURTIME()  -- current time, 08:50:46
```

以上函数将返回时间日期对象

- 提取时间日期对象中的元素：

```sql
SELECT YEAR(NOW())  -- 2020
-- 还有MONTH, DAY, HOUR, MINUTE, SECOND
```

以上函数均返回整数，还有另外两个返回字符串的：

```sql
SELECT DAYNAME(NOW())  -- Saturday
SELECT MONTHNAME(NOW())  -- September
```

**标准SQL语句**有一个类似的函数 EXTRACT()，若需要在不同DBMS中录入代码，最好用EXTRACT()：

> SELECT EXTRACT(YEAR FROM NOW())

当然第一参数也可以是MONTH, DAY, HOUR ……  
总之就是：`EXTRACT(单位 FROM 日期时间对象)`

```sql
-- 返回【今年】的订单
-- 用时间日期函数而非手动输入年份，代码更可靠，不会随着时间的改变而失效

USE sql_store;

SELECT * 
FROM orders
WHERE YEAR(order_date) = YEAR(now())
```

- **4、格式化日期和时间**

**小结**

- `DATE_FORMAT(date, format)` 将 date 根据 format 字符串进行格式化。

- `TIME_FORMAT(time, format)` 类似于 DATE\_FORMAT 函数，但这里 format 字符串只能包含用于小时，分钟，秒和微秒的格式说明符。其他说明符产生一个 NULL 值或0。

**方法**

很多像这种完全不需要记也不可能记得完，重要的是知道有这么个可以实现这个功能的函数，具体的**格式说明符（Specifiers）**可以需要的时候去查，至少有两种方法：  
1\. 直接谷歌关键词 如 **mysql date format functions**, 其实是在官方文档的 12.7 Date and Time Functions 小结里，有两个函数的说明和 specifiers 表  
2\. 用软件里的帮助功能，如 workbench 里的 HELP INDEX 打开官方文档查询或者右侧栏的 automatic comtext help (其是也是查官方文档，不过是自动的)

```sql
SELECT DATE_FORMAT(NOW(), '%M %d, %Y')  -- September 12, 2020
-- 格式说明符里，大小写是不同的，这是目前SQL里第一次出现大小写不同的情况
SELECT TIME_FORMAT(NOW(), '%H:%i %p')  -- 11:07 AM
```

- **5、计算日期和时间**

**小结**

有时需要对日期事件对象进行运算，如增加一天或算两个时间的差值之类，介绍一些最有用的日期时间计算函数：

1.  `DATE_ADD, DATE_SUB`
2.  `DATEDIFF`
3.  `TIME_TO_SEC`

- 增加或减少一定的天数、月数、年数、小时数等等
```sql
SELECT DATE_ADD(NOW(), INTERVAL -1 DAY)
SELECT DATE_SUB(NOW(), INTERVAL 1 YEAR)

-- 但其实不用函数，直接加减更简洁：
NOW() - INTERVAL 1 DAY
NOW() - INTERVAL 1 YEAR 
```

- 计算日期差异

```sql
SELECT DATEDIFF('2019-01-01 09:00', '2019-01-05')  -- -4
-- 会忽略时间部分，只算日期差异

-- 借助 TIME_TO_SEC 函数计算时间差异
-- TIME_TO_SEC：计算从 00:00 到某时间经历的秒数

SELECT TIME_TO_SEC('09:00')  -- 32400
SELECT TIME_TO_SEC('09:00') - TIME_TO_SEC('09:02')  -- -120
```

- **6、IFNULL和COALESCE函数**

之前讲了基本的处理**数值、文本、日期时间**的函数，再介绍几个其它的有用的MySQL函数

**小结**

两个用来替换空值的函数：`IFNULL, COALESCE`. 后者更灵活

**案例**

将 orders 里 shipper.id 中的空值替换为 'Not Assigned'（未分配）
```sql
USE sql_store;

SELECT 
    order_id,
    IFNULL(shipper_id, 'Not Assigned') AS shipper
    /* If expr1 is not NULL, IFNULL() returns expr1; 
    otherwise it returns expr2. */
FROM orders
```

将 orders 里 shipper.id 中的空值替换为 comments，若 comments 也为空则替换为 'Not Assigned'（未分配）
```sql
USE sql_store;

SELECT 
    order_id,
    COALESCE(shipper_id, comments, 'Not Assigned') AS shipper
    /* Returns the first non-NULL value in the list, 
    or NULL if there are no non-NULLvalues. */
FROM orders
```

COALESCE 函数是返回一系列值中的首个非空值，更灵活

**练习**

返回一个有如下两列的查询结果：  
1\. customer (顾客的全名)  
2\. phone (没有的话，显示'Unknown')

```sql
USE sql_store;

SELECT 
    CONCAT(first_name, ' ', last_name) AS customer,
    IFNULL/COALESCE(phone, 'Unknown') AS phone   
FROM customers
```

- **7、IF函数**

**小结**

根据是否满足条件返回不同的值:

`IF(条件表达式, 返回值1, 返回值2)` 返回值可以是任何东西，数值 文本 日期时间 空值null 均可

**案例**

将订单表中订单按是否是今年的订单分类为active（活跃）和archived（存档），之前讲过用UNION法，即用两次查询分别得到今年的和今年以前的订单，添加上分类列再用UNION合并，这里直接在SELECT里运用IF函数可以更容易地得到相同的结果

```sql
USE sql_store;

SELECT 
    *,
    IF(YEAR(order_date) = YEAR(NOW()),
       'Active',
       'Archived') AS category
FROM orders
```

**练习**

得到包含如下字段的表：  
1\. product\_id  
2\. name (产品名称)  
3\. orders (该产品出现在订单中的次数)  
4\. frequency (根据是否多于一次而分类为'Once'或'Many times')
```sql
USE sql_store;

SELECT 
    product_id,
    name,
    COUNT(*) AS orders,
    IF(COUNT(*) = 1, 'Once', 'Many times') AS frequency
    /* 因为之后的内连接筛选掉了无订单的商品，
    所以这里不变考虑次数为0的情况 */
FROM products
JOIN order_items USING(product_id)
GROUP BY product_id
```

另外，发现如果想用同级列别名orders怎么都不行：

若写成 `IF(orders = 1, 'Once', 'Many times') AS frequency`  
会报错：Error Code: 1054. Unknown column 'orders' in 'field list'

若写成 `IF((SELECT orders) = 1, 'Once', 'Many times') AS frequency`  
会报错：Error Code: 1247. Reference 'orders' not supported (reference to group function)

- **8、CASE运算符**

**小结**

当分类多余两种时，可以用IF嵌套，也可以用CASE语句，后者可读性更好

CASE语句结构：

    CASE 
        WHEN …… THEN ……
        WHEN …… THEN ……
        WHEN …… THEN ……
        ……
        [ELSE ……] （ELSE子句是可选的）
    END

**案例**

不是将订单分两类，而是分为三类：今年的是 'Active', 去年的是 'Last Year', 比去年更早的是 'Achived'：
```sql
USE sql_store;

SELECT
    order_id,
    CASE
        WHEN YEAR(order_date) = YEAR(NOW()) THEN 'Active'
        WHEN YEAR(order_date) = YEAR(NOW()) - 1 THEN 'Last Year'
        WHEN YEAR(order_date) < YEAR(NOW()) - 1 THEN 'Achived'
        ELSE 'Future'  
    END AS 'category'
FROM orders
```

ELSE 'Future' 是可选的，实验发现若分类不完整，比如只写了今年和去年的两个分类条件，则不在这两个分类的记录的 category 字段会是 null.

**练习**

得到包含如下字段的表：customer, points, category（根据积分 <2k、2k~3k（包含两端）、>3k 分为青铜、白银和黄金用户）

之前也是用过 UNION 法，分别查询增加分类字段再合并，很麻烦。

```sql
USE sql_store;

SELECT
    CONCAT(first_name, ' ', last_name) AS customer,
    points,
    CASE
        WHEN points < 2000 THEN 'Bronze'
        WHEN points BETWEEN 2000 AND 3000 THEN 'Silver'
        WHEN points > 3000 THEN 'Gold'
        -- ELSE null
    END AS category
FROM customers
ORDER BY points DESC

-- 其实也可以用IF嵌套，甚至代码还少些，但感觉没有CASE语句结构清晰、可读性好

SELECT
    CONCAT(first_name, ' ', last_name) AS customer,
    points,
    IF(points < 2000, 'Bronze', 
        IF(points BETWEEN 2000 AND 3000, 'Silver', 
        -- 第二层的条件表达式也可以简化为 <= 3000
            IF(points > 3000, 'Gold', null))) AS category
FROM customers
ORDER BY points DESC
```

# 第三部分：提高效率——视图、存储过程、函数

## 第八章、视图

**1、创建视图**

**小结**

就是创建虚拟表，自动化一些重复性的查询模块，简化各种复杂操作（包括复杂的子查询和连接等）

注意视图虽然可以像一张表一样进行各种操作，但**并没有真正储存数据**，数据仍然储存在原始表中，**视图只是储存起来的模块化的查询结果**，是为了方便和简化后续进一步操作而储存起来的虚拟表

**案例**

创建 sales\_by\_client 视图

```sql
USE sql_invoicing;

CREATE VIEW sales_by_client AS
    SELECT 
        client_id,
        name,
        SUM(invoice_total) AS total_sales
    FROM clients c
    JOIN invoices i USING(client_id)
    GROUP BY client_id, name;
    -- 虽然实际上这里加不加上name都一样
```

若要删掉该视图用 `DROP VIEW sales_by_client` 或通过右键菜单

创建视图后可就当作 sql\_invoicing 库下一张表一样进行各种操作
```sql
USE sql_invoicing;

SELECT 
    s.name,
    s.total_sales,
    phone
FROM sales_by_client s
JOIN clients c USING(client_id)
WHERE s.total_sales > 500
```

**练习**

创建一个客户差额表视图，可以看到客户的id，名字以及**差额（发票总额-支付总额）**

```sql
USE sql_invoicing;

CREATE VIEW clients_balance AS
    SELECT 
        client_id,
        c.name,
        SUM(invoice_total - payment_total) AS balance
    FROM clients c
    JOIN invoices USING(client_id)
    GROUP BY client_id
```

**2、更新或删除视图**

**小结**

修改视图可以先DROP在CREATE（也可以用CREATE OR REPLACE）

视图的查询语句可以在编辑模式下查看和修改，但最好是保存为sql文件并放在源码控制妥善管理

**案例**

想在上一节的顾客差额视图的查询语句最后加上按差额降序排列
```sql
-- 法1. 先删除再重建
USE sql_invoicing;

DROP VIEW IF EXISTS clients_balance;
-- 若不存在这个视图，直接 DROP 会报错，所以要加上 IF EXISTS 先检测有没有这个视图

CREATE VIEW clients_balance AS 
    ……
    ORDER BY balance DESC


-- 法2. 用REPLACE关键字，即用 `CREATE OR REPLACE VIEW clients_balance AS`，和上面等效，不过上面那种分成两个语句的方式要用的多一点
USE sql_invoicing;

CREATE OR REPLACE VIEW clients_balance AS
    ……
    ORDER BY balance DESC
```

**方法**

如何保存视图的原始查询语句？

法1.

（推荐方法） 将原始查询语句保存为 views 文件夹下的和与视图同名的 clients\_balance.sql 文件，然后将这个文件夹放在源码控制下（put these files under source control）, 通常放在 git repository（仓库）里与其它人共享，团队其他人因此能在自己的电脑上重建这个数据库

法2.

若丢失了原始查询语句，要修改的话可点击视图的扳手按钮打开编辑模式，可看到如下被MySQL处理了的查询语句

MySQL在前面加了些莫名其妙的东西并且在所有库名表名字段名外套上反引号防止名称冲突（当对象名和MySQL里的关键字相同时确保被当作对象名而不是关键字），但这都不影响

直接做我们需要的修改，如加上`ORDER BY balance DESC` 然后点apply就行了。法2是没有办法的办法，当然最好还是将 views 保存为 sql 文件并放入源码控制

**3、可更新视图**

**小结**

视图作为虚拟表/衍生表，除了可用在查询语句SELECT中，也可以用在增删改（INSERT DELETE UPDATE）语句中，但后者有一定的前提条件。

如果一个视图的原始查询语句中没有如下元素：  
1. `DISTINCT` 去重  
2. `GROUP BY/HAVING/聚合函数` (后两个通常是伴随着 GROUP BY 分组出现的)  
3. `UNION` 纵向连接

则该视图是可更新视图（Updatable Views），**可以增删改，否则只能查**

另外，**增还要满足附加条件：视图必须包含底层原表的所有必须字段**

总之，一般通过原表修改数据，但当出于安全考虑或其他原因没有某表的直接权限时，可以**通过视图来修改底层数据 (因为视图不实际存储数据，当你尝试修改视图的数据时，实际上是在尝试修改基本表中的数据，前提是视图被定义为可更新)**

之后会将关于安全和权限的内容

**案例**

创建视图（新虚拟表）invoices\_with\_balance（带差额的发票记录表）

```sql
USE sql_invoicing;

CREATE OR REPLACE VIEW invoices_with_balance AS
SELECT 
    /* 这里有个小技巧，要插入表中的多列列名时，
    可从左侧栏中连选并拖入相关列 */
    invoice_id, 
    number, 
    client_id, 
    invoice_total, 
    payment_total, 
    invoice_date,
    invoice_total - payment_total AS balance,  -- 新增列
    due_date, 
    payment_date
FROM invoices
WHERE (invoice_total - payment_total) > 0
/* 这里不能用列别名balance，会报错说不存在，
必须用原列名的表达式，这还是执行顺序的问题
之前讲WHERE和HAVING作为事前筛选和事后筛选的区别时提到过 */
```

该视图满足条件，是可更新视图，故可以增删改：

- 删：删掉id为1的发票记录

```sql
DELETE FROM invoices_with_balance
WHERE invoice_id = 1
```

- 改：将2号发票记录的期限延后两天

```sql
UPDATE invoices_with_balance
SET due_date = DATE_ADD(due_date, INTERVAL 2 DAY)
WHERE invoice_id = 2
```

- 增：在视图中用INSERT新增记录的话还有另一个前提，即视图必须包含其底层所有原始表的所有必须字段  

例如，若这个 invoices\_with\_balance 视图里没有 invoice\_date 字段（invoices 中的必须字段），那就无法通过该视图向 invoices 表新增记录，因为 invoices 表不会接受 invoice\_date 字段为空的记录

- **4、WITH CHECK OPTION 子句**

**小结**

在视图的原始查询语句最后加上 `WITH CHECK OPTION` 可以防止执行那些会让视图中某些行（记录）消失的修改语句。

**案例**

接前面的 invoices\_with\_balance 视图的例子，该视图与原始的 orders 表相比增加了balance(invouce\_total - payment\_total) 列，且只显示 balance 大于0的行（记录），若将某记录（如2号订单）的 payment\_total 改为和 invouce\_total 相等，则 balance 为0，该记录会从视图中消失：

```sql
UPDATE invoices_with_balance
SET payment_total = invoice_total
WHERE invoice_id = 2
```

更新后会发现invoices\_with\_balance视图里2号订单消失。

但在视图原始查询语句最后加入 `WITH CHECK OPTION` 后，对3号订单执行类似上面的语句后会报错：

```sql
UPDATE invoices_with_balance
SET payment_total = invoice_total
WHERE invoice_id = 3

-- Error Code: 1369. CHECK OPTION failed 'sql_invoicing.invoices_with_balance'
```

- **5、视图的其他优点**

**小结**

三大优点：

- 简化查询
- 增加抽象层和减少变化的影响
- 数据安全性

具体来讲：

1.  （首要优点）简化查询 simplify queries  
    
2.  增加抽象层，减少变化的影响：视图给表增加了一个抽象层（模块化），这样如果数据库设计改变了（如一个字段从一个表转移到了另一个表），只需修改视图的查询语句使其能保持原有查询结果即可，不需要修改使用这个视图的那几十个查询。相反，如果没有视图这一层的话，所有查询将直接使用指向原表的原始查询语句，这样一旦更改原表设计，就要相应地更改所有的这些查询。  
    
3.  限制对原数据的访问权限 Restrict access to the data：在视图中可以对原表的行和列进行筛选，这样如果你禁止了对原始表的访问权限，用户只能通过视图来修改数据，他们就无法修改视图中未返回的那些字段和记录。但注意这通常并不简单，需要良好的规划，否则最后可能搞得一团乱。  
    

了解这些优点，但不要盲目将他们运用在所有的情形中。

## 第九章、存储过程

1\. 什么是存储过程
-----------

What are Stored Procedures (2:18)

**小结**

存储过程三大作用：

1.  储存和管理SQL代码 Store and organize SQL  
    
2.  性能优化 Faster execution  
    
3.  数据安全 Data security  
    

**导航**

之前学了增删改查，包括复杂查询以及如何运用视图来简化查询。

假设你要开发一个使用数据库的应用程序，你应该将SQL语句写在哪里呢？

如果将SQL语句内嵌在应用程序的代码里，将使其混乱且难以维护，所以应该将SQL代码和应用程序代码分开，将SQL代码储存在所属的数据库中，具体来说，是放在储存过程（stored procedure）和函数中。

储存过程是一个包含SQL代码模块的数据库对象，在应用程序代码中，我们调用储存过程来获取和保存数据（get and save the data）。也就是说，我们使用储存过程来储存和管理SQL代码。

使用储存程序还有另外两个好处。首先，大部分DBMS会对储存过程中的代码进行一些优化，因此有时储存过中的SQL代码执行起来会更快。

此外，就像视图一样，储存过程能加强数据安全。比如，我们可以移除对所有原始表的访问权限，让各种增删改的操作都通过储存过程来完成，然后就可以决定谁可以执行何种储存过程，用以限制用户对我们数据的操作范围，例如，防止特定的用户删除数据。

所以，储存过程很有用，本章将学习如何创建和使用它。

2\. 创建一个存储过程
------------

Creating a Stored Procedure (5:34)

**小结**

    DELIMITER $$
    -- delimiter n. 分隔符
    
        CREATE PROCEDURE 过程名()  
            BEGIN
                ……;
                ……;
                ……;
            END$$
    
    DELIMITER ;

**实例**

创造一个get\_clients()过程

    CREATE PROCEDURE get_clients()  
    -- 括号内可传入参数，之后会讲
    -- 过程名用小写单词和下划线表示，这是约定熟成的做法
        BEGIN
            SELECT * FROM clients;
        END

BEGIN 和 END 之间包裹的是此过程（PROCEDURE）的内容（body），内容里可以有多个语句，但每个语句都要以 `;` 结束，包括最后一个。

为了将过程内容内部的语句分隔符与SQL本身执行层面的语句分隔符 `;` 区别开，要先用 DELIMITER(分隔符) 关键字暂时将SQL语句的默认分隔符改为其他符号，一般是改成双美元符号 `$$` ，创建过程结束后再改回来。注意创建过程本身也是一个完整SQL语句，所以别忘了在END后要加一个暂时语句分隔符 `$$`

**注意**

过程内容中所有语句都要以 `;` 结尾并且因此要暂时修改SQL本身的默认分隔符，这些都是MySQL地特性，在SQL Server等就不需要这样

    DELIMITER $$
    
        CREATE PROCEDURE get_clients()  
            BEGIN
                SELECT * FROM clients;
            END$$
    
    DELIMITER ;

调用此程序：

法1. 点击闪电按钮

法2. 用CALL关键字

    USE sql_invoicing;
    CALL get_clients()
    
    或
    
    CALL sql_invoicing.get_clients()

**注意**

上面讲的是如何在SQL中调用储存过程，但更多的时候其实是要在应用程序代码（可能是 C#、JAVA 或 Python 编写的）中调用。

**练习**

创造一个储存过程 get\_invoices\_with\_balance（取得有差额（差额不为0）的发票记录）

    DROP PROCEDURE get_invoices_with_balance;
    -- 注意DROP语句里这个过程没有带括号
    
    DELIMITER $$
    
        CREATE PROCEDURE get_invoices_with_balance()
            BEGIN
                SELECT *
                FROM invoices_with_balance 
                -- 这是之前创造的视图
                -- 用视图好些，因为有现成的balance列
                WHERE balance > 0;
            END$$
    
    DELIMITER ;
    
    CALL get_invoices_with_balance();

3\. 使用MySQL工作台创建存储过程
--------------------

Creating Procedures Using MySQLWorkbench (1:21)

也可以用点击的方式创造过程，右键选择 Create Stored Procedure，填空，Apply。这种方式 Workbench 会帮你处理暂时修改分隔符的问题

这种方式一样可以储存SQL文件

事实证明，mosh很喜欢用这种方式，后面基本都是用这种方式创建过程（毕竟不用管改分隔符的问题，能偷懒又何必自找麻烦呢？谁还不是条懒狗呢？）

4\. 删除存储过程
----------

Dropping Stored Procedures (2:09)

这一节学习删除储存过程，这在发现储存过程里有错误需要重建时很有用。

**实例**

一个创建过程（get\_clients）的标准模板

    USE sql_invoicing;
    
    DROP PROCEDURE IF EXISTS get_clients;
    -- 注意加上【IF EXISTS】，以免因为此过程不存在而报错
    
    DELIMITER $$
    
        CREATE PROCEDURE get_clients()
            BEGIN
                SELECT * FROM clients;
            END$$
    
    DELIMITER ;
    
    CALL get_clients();

**最佳实践**

同视图一样，最好把删除和创建每一个过程的代码也储存在不同的SQL文件中，并把这样的文件放在 Git 这样的源码控制下，这样就能与其它团队成员共享 Git 储存库。他们就能在自己的机器上重建数据库以及该数据库下的所有的视图和储存过程

如上面那个实例，可储存在 stored\_procedures 文件夹（之前已有 views 文件夹）下的 get\_clients.sql 文件。当你把所有这些脚本放进源码控制，你能随时回来查看你对数据库对象所做的改动。

5\. 参数
------

Parameters (5:26)

**小结**

    CREATE PROCEDURE 过程名
    (
        参数1 数据类型,
        参数2 数据类型,
        ……
    )
    BEGIN
    ……
    END

**导航**

学完了如何创建和删除过程，这一节学习如何给过程添加参数

通常我们使用参数来给储存过程传值，但我们也可以用参数获取调用程序的结果值，第二个我们之后再讲

**案例**

创建过程 get\_clients\_by\_state，可返回特定州的顾客

    USE sql_invoicing;
    
    DROP PROCEDURE IF EXISTS get_clients_by_state;
    
    DELIMITER $$
    
    CREATE PROCEDURE get_clients_by_state
    (
        state CHAR(2)  -- 参数的数据类型
    )
    BEGIN
        SELECT * FROM clients c
        WHERE c.state = state;
    END$$
    
    DELIMITER ;

参数类型一般设定为 VARCHAR，除非能确定参数的字符数

多个参数可用逗号隔开

`WHERE state = state` 是没有意义的，有两种方法可以区分参数和列名：一种是取不一样的参数名如 p\_state 或 state\_param，第二种是像上面一样给表起别名，然后用带表前缀的列名以同参数名区分开。

    CALL get_clients_by_state('CA')

不传入'CA'会报错，因为MySQL里所有参数都是必须参数

**练习**

创建过程 get\_invoices\_by\_client，通过 client\_id 来获得发票记录

client\_id 的数据类型设置可以参考原表中该字段的数据类型

    USE sql_invoicing;
    
    DROP PROCEDURE IF EXISTS get_invoices_by_client ;
    
    DELIMITER $$
    
    CREATE PROCEDURE get_invoices_by_client
    (
        client_id INT  -- 为何不写INT(11)？
    )
    BEGIN
    SELECT * 
    FROM invoices i
    WHERE i.client_id = client_id;
    END$$
    
    DELIMITER ;
    
    CALL get_invoices_by_client(1);

Mosh 创建和调用都直接用的点击法

6\. 带默认值的参数
-----------

Parameters with Default Value (8:18)

**小结**

给参数设置默认值，主要是运用条件语句块和替换空值函数

**回顾**

SQL中的条件类语句：

1.  替换空值 `IFNULL(值1，值2)`  
    
2.  条件函数 `IF(条件表达式, 返回值1, 返回值2)`  
    
3.  条件语句块  
    

    IF 条件表达式 THEN
        语句1;
        语句2;
        ……;
    [ELSE]（可选）
        语句1;
        语句2;
        ……;
    END IF;
    -- 别忘了【END IF】

**案例1**

把 get\_clients\_by\_state 过程的默认参数设为'CA'，即默认查询加州的客户

    USE sql_invoicing;
    
    DROP PROCEDURE IF EXISTS get_clients_by_state;
    
    DELIMITER $$
    
    CREATE PROCEDURE get_clients_by_state
    (
        state CHAR(2)  
    )
    BEGIN
        IF state IS NULL THEN 
            SET state = 'CA';  
            /* 注意别忽略SET，
            SQL 里单个等号 '=' 是比较操作符而非赋值操作符
            '=' 与 SET 配合才是赋值 */
        END IF;
        SELECT * FROM clients c
        WHERE c.state = state;
    END$$
    
    DELIMITER ;

调用

    CALL get_clients_by_state(NULL)

注意要调用过程并使用其默认值时时要传入参数 `NULL` ，MySQL不允许不传参数。

**案例2**

将 get\_clients\_by\_state 过程设置为默认选取所有顾客

法1. 用IF条件语句块实现

    ……
    BEGIN
        IF state IS NULL THEN 
            SELECT * FROM clients c;
        ELSE
            SELECT * FROM clients c
            WHERE c.state = state;
        END IF;    
    END$$
    ……

法2. 用IFNULL替换空值函数实现

    ……
    BEGIN
        SELECT * FROM clients c
        WHERE c.state = IFNULL(state, c.state)
    END$$
    ……

若参数为NULL，则返回c.state，利用 c.state = c.state 永远成立来返回所有顾客，思路很巧妙。

**练习**

创建一个叫 get\_payments 的过程，包含 client\_id 和 payment\_method\_id 两个参数，数据类型分别为 INT(4) 和 TINYINT(1) (1个字节，能存0~255，之后会讲数据类型，好奇可以谷歌 'mysql int size')，默认参数设置为返回所有记录

这是一个为你的工作预热的练习

    USE sql_invoicing;
    
    DROP PROCEDURE IF EXISTS get_payments;
    
    DELIMITER $$
    
    CREATE PROCEDURE get_payments
    (
        client_id INT,  -- 不用写成INT(4)
        payment_method_id TINYINT
    )
    BEGIN
        SELECT * FROM payments p
        WHERE 
            p.client_id = IFNULL(client_id, p.client_id) AND
            p.payment_method = IFNULL(payment_method_id, p.payment_method);
            -- 再次小心这种实际工作中各表相同字段名称不同的情况
    END$$
    
    DELIMITER ;

调用：

所有支付记录

    CALL get_payments(NULL, NULL)

1号顾客的所有记录

    CALL get_payments(1, NULL)

3号支付方式的所有记录

    CALL get_payments(NULL, 3)

5号顾客用2号支付方式的所有记录

    CALL get_payments(5, 2)

**注意**

注意一个区别：

1.  Parameter 形参（形式参数）：创建过程中用的占位符，如 client\_id、payment\_method\_id  
    
2.  Argument 实参（实际参数）：调用时实际传入的值，如 1、3、5、NULL  
    

7\. 参数验证
--------

Parameter Validation (6:40)

**小结**

过程除了可以查，也可以增删改，但修改数据前最好先进行参数验证以防止不合理的修改

主要利用条件语句块和 `SIGNAL` `SQLSTATE` `MESSAGE_TEXT` 关键字

具体来说是在过程的内容开头加上这样的语句：

    IF 错误参数条件表达式 THEN
        SIGNAL SQLSTATE '错误类型'
            [SET MESSAGE_TEXT = '关于错误的补充信息']（可选）

**案例**

创建一个 make\_payment 过程，含 invoice\_id, payment\_amount, payment\_date 三个参数

（Mosh还是喜欢通过右键 Create Stored Procedure 地方式创建，不必考虑暂时改分隔符的问题，更简便）

    CREATE DEFINER=`root`@`localhost` PROCEDURE `make_payment`(
        invoice_id INT,
        payment_amount DECIMAL(9, 2),
        /*
        9是精度， 2是小数位数。
        精度表示值存储的有效位数，
        小数位数表示小数点后可以存储的位数
        见：
        https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html 
        */
        payment_date DATE    
    )
    BEGIN   
        UPDATE invoices i
        SET 
            i.payment_total = payment_amount,
            i.payment_date = payment_date
        WHERE i.invoice_id = invoice_id;
    END

为了防止传入像 -100 的 payment\_total 这样不合理的参数，要在增加一段参数验证语句，利用的是条件语句块加SIGNAL关键字，和其他编程语言中的抛出异常等类似

具体的错误类型可通过谷歌 "sqlstate error" 查阅（推荐使用IBM的那个表），这里是 '22 Data Exception' 大类中的 '22003 A numeric value is out of range.' 类型

注意还添加了 MESSAGE\_TEXT 以提供给用户参数错误的更具体信息。现在传入 负数的 payment\_amount 就会报错 'Error Code: 1644. Invalid payment amount '

    ……
    BEGIN
        IF payment_amount <= 0 THEN
            SIGNAL SQLSTATE '22003' 
                SET MESSAGE_TEXT = 'Invalid payment amount';    
        END IF;
    
        UPDATE invoices i
        ……
    END

**注意**

过犹不及（"Too much of a good thing is a bad thing"），加入过多的参数验证会让代码过于复杂难以维护，像 payment\_amount 非空这样的验证就不需要添加因为 payment\_amount 字段本身就不允许空值因此MySQL会自动报错，。

参数验证工作更多的应该在应用程序端接受用户输入数据时就检测和报告，那样更快也更有效。储存过程里的参数验证只是在有人越过应用程序直接访问储存过程时作为最后的防线。这里只应该写那些最关键和必要的参数验证。

8\. 输出参数
--------

Output Parameters (3:55)

**小结**

输入参数是用来给过程传入值的，我们也可以用输出参数来获取过程的结果值

具体是在参数的前面加上 OUT 关键字，然后再 SELECT 后加上 INTO……

调用麻烦，如无需要，不要多此一举

**案例**

创造 get\_unpaid\_invoices\_for\_client 过程，获取特定顾客所有未支付过的发票记录（即 payment\_total = 0 的发票记录）

    CREATE DEFINER=`root`@`localhost` PROCEDURE `get_unpaid_invoices_for_client`(
            client_id INT
    )
    BEGIN
        SELECT COUNT(*), SUM(invoice_total)
        FROM invoices i
        WHERE 
            i.client_id = client_id AND
            payment_total = 0;
    END

调用

    call sql_invoicing.get_unpaid_invoices_for_client(3);

得到3号顾客的 COUNT(\*) 和 SUM(invoice\_total) （未支付过的发票数量和总金额）分别为2和286

我们也可以通过输出参数（变量）来获取这两个结果值，修改过程，添加两个输出参数 invoice\_count 和 invoice\_total：

    CREATE DEFINER=`root`@`localhost` PROCEDURE `get_unpaid_invoices_for_client`(
            client_id INT,
            OUT invoice_count INT,
            OUT invoice_total DECIMAL(9, 2)
            -- 默认是输入参数，输出参数要加【OUT】前缀
    )
    BEGIN
        SELECT COUNT(*), SUM(invoice_total)
        INTO invoice_count, invoice_total
        -- SELECT后跟上INTO语句将SELECT选出的值传入输出参数（输出变量）中
        FROM invoices i
        WHERE 
            i.client_id = client_id AND
            payment_total = 0;
    END

调用：单击闪电按钮调用，只用输入client\_id，得到如下语句结果

    set @invoice_count = 0;
    set @invoice_total = 0;
    call sql_invoicing.get_unpaid_invoices_for_client(3, @invoice_count, @invoice_total);
    select @invoice_count, @invoice_total;

先定义以@前缀表示用户变量，将初始值设为0。（变量（variable）简单讲就是储存单一值的对象）再调用过程，将过程结果赋值给这两个输出参数，最后再用SELECT查看。

很明显，通过输出参数获取并读取数据有些麻烦，若无充足的原因，不要多此一举。

9\. 变量
------

Variables (4:33)

**小结**

两种变量:

1.  用户或会话变量 `SET @变量名 = ……`  
    
2.  本地变量 `DECLARE 变量名 数据类型 [DEFAULT 默认值]`  
    

**用户或会话变量（User or session variable）：**

上节课讲过，用 SET 语句并在变量名前加 @ 前缀来定义，将在整个用户会话期间存续，在会话结束断开MySQL链接时才被清空，这种变量主要在调用带输出的储存过程时，作为输出参数来获取结果值。

**实例**

    set @invoice_count = 0;
    set @invoice_total = 0;
    call sql_invoicing.get_unpaid_invoices_for_client(3, @invoice_count, @invoice_total);
    select @invoice_count, @invoice_total;

**本地变量（Local variable）**

在储存过程或函数中通过 DECLARE 声明并使用，在函数或储存过程执行结束时就被清空，常用来执行过程（或函数）中的计算

**案例**

创造一个 get\_risk\_factor 过程，使用公式 risk\_factor = invoices\_total / invoices\_count \* 5

    CREATE DEFINER=`root`@`localhost` PROCEDURE `get_risk_factor`()
    BEGIN
        -- 声明三个本地变量，可设默认值
        DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
        DECLARE invoices_total DECIMAL(9, 2);
        DECLARE invoices_count INT;
    
        -- 用SELECT得到需要的值并用INTO传入invoices_total和invoices_count
        SELECT SUM(invoice_total), COUNT(*)
        INTO invoices_total, invoices_count
        FROM invoices;
    
        -- 用SET语句给risk_factor计算赋值
        SET risk_factor = invoices_total / invoices_count * 5;
    
        -- 展示最终结果risk_factor
        SELECT risk_factor;        
    END

10\. 函数
-------

Functions (6:28)

**小结**

创建函数和创建过程的两点不同

    1. 参数设置和body内容之间，有一段确定返回值类型以及函数属性的语句段
    RETURNS INTEGER
    DETERMINISTIC
    READS SQL DATA
    MODIFIES SQL DATA
    ……
    2. 最后是返回（RETURN）值而不是查询（SELECT）值
    RETURN IFNULL(risk_factor, 0);

删除

    DROP FUNCTION [IF EXISTS] 函数名

**导航**

现在已经学了很多内置函数，包括聚合函数和处理数值、文本、日期时间的函数，这一节学习如何创建函数

函数和储存过程的作用非常相似，唯一区别是函数只能返回单一值而不能返回多行多列的结果集，当你只需要返回一个值时就可以创建函数。

**案例**

在上一节的储存过程 get\_risk\_factor 的基础上，创建函数 get\_risk\_factor\_for\_client，计算特定顾客的 risk\_factor

还是用右键 Create Function 来简化创建

创建函数的语法和创建过程的语法极其相似，区别只在两点：

1.  参数设置和body内容之间，有一段确定返回值类型以及函数属性的语句段  
    
2.  最后是返回（RETURN）值而不是查询（SELECT）值  
    

另外，关于函数属性的说明：

1.  DETERMINISTIC 决定性的：唯一输入决定唯一输出，和数据的改动更新无关，比如税收是订单总额的10%，则以订单总额为输入税收为输出的函数就是决定性的（？），但这里每个顾客的 risk\_factor 会随着其发票记录的增加更新而改变，所以不是DETERMINISTIC的  
    
2.  READS SQL DATA：需要用到 SELECT 语句进行数据读取的函数，几乎所有函数都满足  
    
3.  MODIFIES SQL DATA：函数中有 增删改 或者说有 INSERT DELETE UPDATE 语句，这个例子不满足  
    

    CREATE DEFINER=`root`@`localhost` FUNCTION `get_risk_factor_for_client`
    (
        client_id INT
    ) 
    RETURNS INTEGER
    -- DETERMINISTIC
    READS SQL DATA
    -- MODIFIES SQL DATA
    BEGIN
        DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
        DECLARE invoices_total DECIMAL(9, 2);
        DECLARE invoices_count INT;
    
        SELECT SUM(invoice_total), COUNT(*)
        INTO invoices_total, invoices_count
        FROM invoices i
        WHERE i.client_id = client_id;
        -- 注意不再是整体risk_factor而是特定顾客的risk_factor
    
        SET risk_factor = invoices_total / invoices_count * 5;
        RETURN IFNULL(risk_factor, 0);       
    END

有些顾客没有发票记录，NULL乘除结果还是NULL，所以最后用 IFNULL 函数将这些人的 risk\_factor 替换为 0

调用案例：

    SELECT 
        client_id,
        name,
        get_risk_factor_for_client(client_id) AS risk_factor
        -- 函数当然是可以处理整列的，我第一时间竟只想到传入具体值
        -- 不过这里更像是一行一行的处理，所以应该每次也是传入1个client_id值
    FROM clients

删除，还是用DROP

    DROP FUNCTION [IF EXISTS] get_risk_factor_for_client

**注意**

和视图和过程一样，也最好存入SQL文件并加入源码控制，老生常谈了。

11\. 其他约定
---------

Other Conventions (1:51)

有各种各样的命名习惯（包括对函数过程的命名习惯以及对更改分隔符的习惯），没有明显的好坏之分，重要的是在一个项目或团队中保持恒定不变，学会入乡随俗

A Quick Note
------------

……

* * *

![](https://pic2.zhimg.com/v2-caeb318bb0f895792b2e03df9c242f69_r.jpg)

  

上一级目录：[Mosh\_完全掌握SQL课程\_学习笔记](https://zhuanlan.zhihu.com/p/222865842)

其它相关：[数据概要](https://zhuanlan.zhihu.com/p/222899373)

编辑于 2020-12-06 17:05

[

MySQL

](https://www.zhihu.com/topic/19554128)

[

SQL

](https://www.zhihu.com/topic/19553557)

[

数据库

](https://www.zhihu.com/topic/19552067)

​赞同 40​​13 条评论

​分享

​喜欢​收藏​申请转载

​

赞同 40

​

分享
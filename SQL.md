
[课程：Mosh_完全掌握SQL【笔记】](https://zhuanlan.zhihu.com/p/222865842)

别人的笔记, 自己复制过来的

- [第一部分：基础——增删查改](#第一部分基础增删查改)
  - [第二章、在单一表格中检索数据](#第二章在单一表格中检索数据)
  - [第三章、在多张表格中检索数据](#第三章在多张表格中检索数据)
  - [第四章、插入、更新和删除数据](#第四章插入更新和删除数据)
  - [3. 插入多行](#3-插入多行)
  - [4. 插入分级行](#4-插入分级行)
  - [5. 创建表的副本](#5-创建表的副本)
  - [6. 更新单行](#6-更新单行)
  - [7. 更新多行](#7-更新多行)
  - [8. 在Updates中用子查询](#8-在updates中用子查询)
  - [9. 删除行](#9-删除行)
  - [10. 恢复数据库](#10-恢复数据库)

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

| 符号 | 意义| 
| --- | --- | 
| `^`| 开头| 
| `$`| 结尾
| `[abc]` | 含abc| 
| `[a-c]` | 含a到c| 
| `|` | logical or| 

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

**1、列属性**

点击表的扳手按钮：打开设计模式，介绍了一些表中字段/列的属性。


**2、插入单行**

**小结**

> INSERT INTO 目标表 （目标列，可选，逗号隔开）
> VALUES (目标值，逗号隔开)

**案例**

在顾客表里插入一个新顾客的信息

法1. 若不指明列名，则插入的值必须按所有字段的顺序完整插入

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

法2. 指明列名，可跳过取默认值的列且可更改顺序，一般用这种，更清晰

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

3\. 插入多行
--------

Inserting Multiple Rows (3:18)

**小结**

`VALUES ……` 里一行内数据用**括号内逗号**隔开，而多行数据用**括号间逗号**隔开

**案例**

插入多条运货商信息

    USE sql_store
    
    INSERT INTO shippers (name)
    VALUES ('shipper1'),
           ('shipper2'),
           ('shipper3');

**练习**

插入多条产品信息

    USE sql_store;
    
    INSERT INTO products 
    VALUES (DEFAULT, 'product1', 1, 10),
           (DEFAULT, 'product2', 2, 20),
           (DEFAULT, 'product3', 3, 30)

或

    INSERT INTO products (name, quantity_in_stock, unit_price)
    VALUES ('product1', 1, 10),
           ('product2', 2, 20),
           ('product3', 3, 30)

还是感觉后面这种指明列名的要清晰一点

**注意**

对于AI (Auto Incremental 自动递增) 的id字段，MySQL会记住删除的/用过的id，并在此基础上递增

4\. 插入分级行
---------

Inserting Hierarchical Rows (5:53)

**小结**

订单表（orders表）里的一条记录对应订单项目表（order\_items表）里的多条记录，一对多，是相互关联的父子表。通过添加一条订单记录和对应的多条订单项目记录，学习如何向父子表插入分级（层）/耦合数据（insert hierarchical data）：

*   关键：在插入子表记录时，需要用内建函数 `LAST_INSERT_ID()` 获取相关父表记录的自增ID（这个例子中就是 order\_id)
*   内建函数：MySQL里有很多可用的内置函数，也就是可复用的代码块，各有不同的功能，注意函数名的单词之间用下划线连接
*   `LAST_INSERT_ID()`：获取最新的成功的 `INSERT 语句` 中的自增id，在这个例子中就是父表里新增的 order\_id.

**案例**

新增一个订单（order），里面包含两个订单项目/两种商品（order\_items），请同时更新订单表和订单项目表

    USE sql_store;
    
    INSERT INTO orders (customer_id, order_date, status)
    VALUES (1, '2019-01-01', 1);
    
    -- 可以先试一下用 SELECT last_insert_id() 看能否成功获取到的最新的 order_id
    
    INSERT INTO order_items  -- 全是必须字段，就不用指定了
    VALUES 
        (last_insert_id(), 1, 2, 2.5),
        (last_insert_id(), 2, 5, 1.5)

5\. 创建表的副本
----------

Creating a Copy of a Table (8:47)

**小结**

`DROP TABLE 要删的表名`、`CREATE TABLE 新表名 AS 子查询`

`TRUCATE '要清空的表名'`、`INSERT INTO 表名 子查询`

子查询里当然也可以用WHERE语句进行筛选

**案例 1**

运用 `CREAT TABLE 新表名 AS 子查询` 快速创建表 orders 的副本表 orders\_archived

    USE sql_store;
    
    CREATE TABLE orders_archived AS
        SELECT * FROM orders  -- 子查询

`SELECT * FROM orders` 选择了 oders 中所有数据，作为AS的内容，是一个子查询

*   子查询： 任何一个充当另一个SQL语句的一部分的 `SELECT……` 查询语句都是子查询，子查询是一个很有用的技巧。

**注意**

创建已有的表或删除不存在的表的话都会报错，所以建表和删表语句都最好加上条件语句（后面会讲）

**案例 2**

不再用全部数据，而选用原表中部分数据创建副本表，如，用今年以前的 orders 创建一个副本表 orders\_archived，其实就是在子查询里增加了一个WHERE语句进行筛选。注意要先 drop 删掉 或 truncate 清空掉之前建的 orders\_archived 表再重建或重新插入数据。

法1. `DROP TABLE 要删的表名`、`CREATE TABLE 新表名 AS 子查询`

    USE sql_store;
    
    DROP TABLE orders_archived;  -- 也可右键该表点击 drop
    CREATE TABLE orders_archived AS
        SELECT * FROM orders
        WHERE order_date < '2019-01-01'

法2. `TRUCATE '要清空的表名'`、`INSERT INTO 表名 子查询`

`INSERT INTO 表名 子查询` 很常用，子查询替代原先插入语句中 `VALUES(……,……),(……,……),……` 的部分

    TRUNCATE 'orders_archived';
    -- 也可右键该表点击 truncate  
    /*新的 8.0版 MySQL 的语法好像变为了 TRUNCATE TABLE orders_archived？
    那样就与 DROP TABLE orders_archived 一致了*/
    INSERT INTO orders_archived  
    -- 不用指明列名，会直接用子查询表里的列名
        SELECT * FROM orders  
        -- 子查询，替代原先插入语句中VALUES(……,……),(……,……),…… 的部分
        WHERE order_date < '2019-01-01'

**练习**

创建一个存档发票表，只包含有过支付记录的发票并将顾客id换成顾客名字

构建的思路顺序：

1.  先创建子查询，确定新表内容：

A. 合并发票表和顾客表

B. 筛选支付记录不为空的行/记录

C. 筛选（并重命名）需要的列

2\. 第1步得到的查询内容，可以先运行看一下，确保准确无误后，再作为子查询内容存入新创建的副本订单存档表 `CREATE TABLE 新表名 AS 子查询`

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

6\. 更新单行
--------

**小结**

用 `UPDATE ……` 语句 来修改表中的一条或多条记录，具体语法结构：

    UPDATE 表 
    SET 要修改的字段 = 具体值/NULL/DEFAULT/列间数学表达式 （修改多个字段用逗号分隔）
    WHERE 行筛选

**实例**

    USE sql_invoicing;
    
    UPDATE invoices
    SET 
        payment_total = 100 / 0 / DEFAULT / NULL / 0.5 * invoice_total, 
        /*注意 0.5 * invoice_total 的结果小数部分会被舍弃，
        之后讲数据类型会讲到这个问题*/
        payment_date = '2019-01-01' / DEFAULT / NULL / due_date
    WHERE invoice_id = 3

7\. 更新多行
--------

Updating Multiple Rows (3:14)

**小结**

语法一样的，就是让 `WHERE……` 的条件包含更多记录，就会同时更改多条记录了

**注意**

Workbench默认开启了Safe Updates功能，不允许同时更改多条记录，要先关闭该功能（在 Edit-Preferences-SQL Editor-Safe Updates）

    USE sql_invoicing;
    
    UPDATE invoices
    SET payment_total = 233, payment_date = due_date
    WHERE client_id = 3  
    -- 该客户的发票记录不止一条，将同时更改
    /WHERE client_id IN (3, 4) 
    -- 第二章 4~9 讲的那些写 WHERE 条件的方法均可用
    -- 甚至可以直接省略 WHERE 语句，会直接更改整个表的全部记录

**练习**

让所有非90后顾客的积分增加50点

    USE sql_store;
    
    UPDATE customers
    SET points = points + 50
    WHERE birth_date < '1990-01-01'

8\. 在Updates中用子查询
-----------------

Using Subqueries in Updates (5:36)

**小结**

非常有用，其实本质上是将子查询用在 `WHERE……` 行筛选条件中

**注意**

1.  括号的使用
2.  `IN ……` 后除了可接 `（……, ……）` 也可接由子查询得到的多个数据（一列多条数据），感觉和前面 `VALUES` 后可接子查询道理是相通的

**案例**

更改发票记录表中名字叫 Yadel 的记录，但该表只有 client\_id，故先要从另一个顾客表中查询叫 Yadel 人的 client\_id

实际中这是很可能的情形，比如一个App是通过搜索名字来更改发票记录的

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

    USE sql_store;
    
    UPDATE orders
    SET comments = 'gold customer'
    WHERE customer_id IN
        (SELECT customer_id
        FROM customers
        WHERE points > 3000)

9\. 删除行
-------

Deleting Rows (1:24)

**小结**

语法结构：

    DELETE FROM 表 
    WHERE 行筛选条件
    （当然也可用子查询）
    （若省略 WHERE 条件语句会删除表中所有记录（和 TRUNCATE 等效？））

**案例**

选出顾客id为3/顾客名字叫'Myworks'的发票记录

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

10\. 恢复数据库
----------

Restoring the Databases (1:06)

就是重新运行那个 create-databases.sql 文件以重置数据库

* * *

![](https://pic2.zhimg.com/v2-caeb318bb0f895792b2e03df9c242f69_r.jpg)

  

上一级目录：[Mosh\_完全掌握SQL课程\_学习笔记](https://zhuanlan.zhihu.com/p/222865842)

其它相关：[数据概要](https://zhuanlan.zhihu.com/p/222899373)

编辑于 2020-12-06 17:03

[

SQL

](https://www.zhihu.com/topic/19553557)

[

MySQL

](https://www.zhihu.com/topic/19554128)

​赞同 59​​8 条评论

​分享

​喜欢​收藏​申请转载

​

赞同 59

​

分享
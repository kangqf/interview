## SQL 常规操作
以下操作均以 《SQL必知必会》 为蓝本

1. distinct
`SELECT DISTINCT vend_id FROM Products;`
2. limit
`SELECT prod_name FROM Products LIMIT 5;`
3. offset
`SELECT prod_name FROM Products LIMIT 5 OFFSET 5;`

等价于

`SELECT prod_name FROM Products LIMIT 5,5;`
4. 注释
单行注释：`#`  行内注释：`--`  多行注释：`/**/`

5. order by  该字句应该是最后一条子句  并且  可以使用非选择的列进行排序
`SELECT prod_name FROM Products ORDER BY prod_name,prod_price;`

使用列位置

`SELECT prod_id,prod_price,prod_name FROM Products ORDER BY 2,3;`

使用升序和降序 默认是升序 ASC 可以指定为降序 DESC

`SELECT prod_name FROM Products ORDER BY prod_name DESC,prod_price;`

6. where
支持的操作符有：`= != <= < >= > !< !> <> BETWEEN`

`SELECT prod_name,prod_price FROM Products WHERE prod_price BETWEEN 5 AND 10;`

`SELECT prod_name FROM Products WHERE prod_price IS NULL;` 空值检查

7. AND  OR  IN NOT   AND 优先级高于 OR 在使用级联的情况下建议使用括号
`SELECT prod_name,prod_price FROM Products WHERE vend_id = 'DLL01' OR vend_id = 'BRS01'`

`SELECT prod_name,prod_price FROM Products WHERE vend_id = 'DLL01' AND prod_price <= 4`

`SELECT prod_name,prod_price FROM Products WHERE vend_id IN ('DLL01','BRS01')`

8. 使用通配符 谓词like
`SELECT prod_id,prod_name FROM Products WHERE prod_name LIKE '%bean bag%'` ** % 匹配0或多个字符** 另外还有** _ 只匹配单个字符**

9. 拼接与别名
`SELECT Contact(vend_name,'(',RTRM(vend_country),')') FROM Vendors ORDER BY vend_name` 拼接两个字段，并加上括号 RTRM用于去除右边的空格

`SELECT Contact(vend_name,'(',RTRM(vend_country),')') AS vend_title FROM Vendors ORDER BY vend_name` 使用别名

`SELECT quantity*item_price AS expanded_price FROM Orderitems WHERE order_num = 20008`

10. 函数
字符处理函数
```
LEFT() 返回字符串左边字符
LENGTH() 返回字符串长度
LOWER() 将字符串转为小写
LTRIM() 去除字符串左侧的空格
RTGHT() 返回字符串右边字符
RTIM() 去除右边的空格
UPPER() 将字符串转换为大写
```

数值处理函数
```
ABS()
COS()
EXP()
PI()
SIN()
SQRT()
TAN()
```

聚集函数 经常与AS 连用
```
AVG()
COUNT() 可以内嵌 DISTINCT 例如 AVG(DISTINCT prod_price)
MAX()
MIN()
SUM()
```
11. 分组数据  group by   having
`SELECT cust_id,COUNT(*) AS orders From Orders GROUP BY cust_id HAVING COUNT(*) >= 2`

这里 **先通过 where 过滤，然后进行 group by 最后再通过having 筛选**

SELECT 子句的顺序**SETECT FROM WHERE GROUP BY HAVING ORDER BY**


sql 操作符优先级


左外联 右外联  在内链接的基础上保存哪边的不符合的数据就是哪个

union 并集
expect  差集
intersect  交集
内联  普通 的两个一样的

交叉连接
自身连接
完全连接
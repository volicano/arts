## ARTS

### Tip

#### mysql中查询重复数据记录

在实际的电商网站中，当订单量比较大的时候会偶尔出现相同的订单数据，这条订单的数据特征一般是除了订单id不同外，其他信息后一样，包含订单的创建时间都一样。
例如我们有一张订单表：

![图片](https://images-cdn.shimo.im/aQKH0f93DAk9m8qf/image.png!thumbnail)

我们需要查出有重复信息的订单数据：
```
SELECT
 id,
 payment_id,
 delivery_id,
 user_id,
 cart_id,
 type,
 total_pay,
 total_quantity,
 created_at,
 COUNT(*) AS num
FROM
 oms_orders
WHERE
 type = 0
GROUP BY
 payment_id,
 delivery_id,
 user_id,
 cart_id,
 type,
 total_pay,
 total_quantity,
 created_at
HAVING
 num > 1;
```
显示结果：

![图片](https://images-cdn.shimo.im/LiuC5gko7OQsoCBh/image.png!thumbnail)

在根据id去查询存在相同数据，因为是同时写入数据库的所以我们的where条件写他的相邻id，例如第一条的数据结果：

![图片](https://images-cdn.shimo.im/qHKR7hvpZiIagH9W/image.png!thumbnail)

**总结知识点：**

以下sql语句可以实现查找出一个表中的所有重复的记录.

select user_name,count(*) as count from user_table group by user_name having count>1;

参数说明:

user_name为要查找的重复字段，可以是多个字段，如果是多个字段相当于按多个字段进行了分组.

count用来判断大于一的才是重复的.

user_table为要查找的表名.

group by用来分组

having用来过滤group by分组后的数据.





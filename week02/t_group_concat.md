## ARTS

### Share


####mysql中group_concat()的用法

例如我们有如下表信息：

![图片](https://images-cdn.shimo.im/kBp4ckmqvUcGN2Qm/u_2135681378_882006691_fm_173_app_25_f_JPEG.jpg!thumbnail)

在有group by的查询语句中，select指定的字段要么就包含在group by语句的后面，作为分组的依据，要么就包含在聚合函数中。

![图片](https://images-cdn.shimo.im/tgF0WCUtWHkrFOOV/2.jpg!thumbnail)

上图可以查询name相同的的人中最小的id。.

如果我们要查询name相同的人的所有的id呢？当然我们可以这样查询：

![图片](https://images-cdn.shimo.im/HF1abS6U5iMixiLV/3.jpg!thumbnail)

但是这样同一个名字出现多次，看上去非常不直观。有没有更直观的方法，既让每个名字都只出现一次，又能够显示所有的名字相同的人的id呢？

当然，就是使用group_concat()函数。

1、功能：将group by产生的同一个分组中的值连接起来，返回一个字符串结果。

2、语法：group_concat( [distinct] 要连接的字段 [order by 排序字段 asc/desc ] [separator '分隔符'] )

说明：通过使用distinct可以排除重复值；如果希望对结果中的值进行排序，可以使用order by子句；separator是一个字符串值，缺省为一个逗号。

3、举例：

例1：使用group_concat()和group by显示相同名字的人的id号：

![图片](https://images-cdn.shimo.im/E4LazhnIlYoQZaKJ/4.jpg!thumbnail)

例2：将上面的id号从大到小排序，且用'_'作为分隔符：

![图片](https://images-cdn.shimo.im/WixSmY3rJdMdqFqY/5.jpg!thumbnail)

例如3：我们要查询以name分组的所有组的id和score：

![图片](https://images-cdn.shimo.im/WK6wgGJDzF85a4sx/6.jpg!thumbnail)


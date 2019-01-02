## ARTS

### Tip

## mysql中if()函数使用
在mysql中if()函数的用法类似于java中的三元表达式，其用处也比较多，具体语法如下：

if(expr1,expr2,expr3)，如果expr1的值为true，则返回expr2的值，如果expr1的值为false，

则返回expr3的值。

其经常判断查询出来的值，示例：

```
mysql> select name,if(sex=0,'女','男') as sex from student;
+-------+-----+
| name  | sex |
+-------+-----+
| name1 | 女 |
| name2 | 女 |
| name3 | 男 |
| name4 | 女 |
+-------+-----+
4 rows in set (0.00 sec)
```




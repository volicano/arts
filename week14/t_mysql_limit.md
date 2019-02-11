## ARTS

### Tip

## MySQL的Limit详解

**MySQL的Limit子句**

  Limit子句可以被用于强制 SELECT 语句返回指定的记录数。Limit接受一个或两个数字参数。参数必须是一个整数常量。如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目。
  
  //初始记录行的偏移量是 0(而不是 1)：
  
  mysql> SELECT * FROM table LIMIT 5,10; //检索记录行6-15
  
  //为了检索从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为 -1：
  
  mysql> SELECT * FROM table LIMIT 95,-1; // 检索记录行 96-last
  
  //如果只给定一个参数，它表示返回最大的记录行数目。换句话说，LIMIT n 等价于 LIMIT 0,n：
  
  mysql> SELECT * FROM table LIMIT 5;     //检索前 5 个记录行
  
**Limit的效率高？**

  常说的Limit的执行效率高，是对于一种特定条件下来说的：即数据库的数量很大，但是只需要查询一部分数据的情况。
  
  高效率的原理是：避免全表扫描，提高查询效率。
  
  比如：每个用户的email是唯一的，如果用户使用email作为用户名登陆的话，就需要查询出email对应的一条记录。
  
  SELECT * FROM t_user WHERE email=?;
  
  上面的语句实现了查询email对应的一条用户信息，但是由于email这一列没有加索引，会导致全表扫描，效率会很低。
  
  SELECT * FROM t_user WHERE email=? LIMIT 1;
  
  加上LIMIT 1，只要找到了对应的一条记录，就不会继续向下扫描了，效率会大大提高。
  
**Limit的效率低？**

  在一种情况下，使用limit效率低，那就是：只使用limit来查询语句，并且偏移量特别大的情况做以下实验：
 语句1：

           select * from table limit 150000,1000;

  语句2:
  
           select * from table while id>=150000 limit 1000;

  语句1为0.2077秒；语句2为0.0063秒
  
  两条语句的时间比是：语句1/语句2＝32.968
  
  
  比较以上的数据时，我们可以发现采用where...limit....性能基本稳定，受偏移量和行数的影响不大，而单纯采用limit的话，受偏移量的影响很大，当偏移量大到一定后性能开始大幅下降。不过在数据量不大的情况下，两者的区别不大。
  
  所以应当先使用where等查询语句，配合limit使用，效率才高。
    
**附录：OFFSET**

  为了与 PostgreSQL 兼容，MySQL 也支持句法： LIMIT # OFFSET #。
  
  经常用到在数据库中查询中间几条数据的需求
  
  比如下面的sql语句：
    
    ① selete * from testtable limit 2,1;
    ② selete * from testtable limit 2 offset 1;
    
  注意：
  
    1.数据库数据计算是从0开始的
    2.offset X是跳过X个数据，limit Y是选取Y个数据
    3.limit  X,Y  中X表示跳过X个数据，读取Y个数据
    
  这两个都是能完成需要，但是他们之间是有区别的：
  
    ①是从数据库中第三条开始查询，取一条数据，即第三条数据读取，一二条跳过
    ②是从数据库中的第二条数据开始查询两条数据，即第二条和第三条。
    


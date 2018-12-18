## ARTS

### Tip

## 三种修改数据库引擎的方式

1、直接alter table

ALTER TABLE youTable ENGINE=InnoDB; 

这种方式最简单，但是对于大数据的表会消耗很长时间，因为MySQL要执行旧表到新表的逐行复制。而且alter table操作不管哪种引擎，MySQL都会锁整个表。

2、利用dump和source

首先dump需要的表，然后修改dump文件，去掉DROP TABLE修改CREATE TABLE代码，执行source。

这种方式不能在线修改引擎，需要让数据库下线；或者在线修改后进行同步。

3、利用CREATE和SELECT

CREATE TABLE myTableCopy LIKE myTable; 

ALTER TABLE myTableCopy ENGINE=InnoDB; 

INSERT INTO myTableCopy SELECT * FROM myTable WHERE id BETWEEN x AND y;  

这种方式时候表的数据量比较大的情况，可以分批根据范围倒入，不会锁myTable。



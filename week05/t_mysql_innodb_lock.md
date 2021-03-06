## ARTS

### Tip

### InnoDB引擎的锁机制

 之所以以InnoDB为主介绍锁，是因为InnoDB支持事务，支持行锁和表锁；Myisam不支持事务，只支持表锁
  
 共享锁（S）：允许一个事务去读一行，阻止其他事务获得相同数据集的排他锁。
 
 排他锁（X)：允许获得排他锁的事务更新数据，阻止其他事务取得相同数据集的共享读锁和排他写锁。
 
 意向共享锁（IS）：事务打算给数据行加行共享锁，事务在给一个数据行加共享锁前必须先取得该表的IS锁。
 
 意向排他锁（IX）：事务打算给数据行加行排他锁，事务在给一个数据行加排他锁前必须先取得该表的IX锁。
  
 说明：
 
 1）共享锁和排他锁都是行锁，意向锁都是表锁，应用中我们只会使用到共享锁和排他锁，意向锁是mysql内部使用的，不需要用户干预。
 
 2）对于UPDATE、DELETE和INSERT语句，InnoDB会自动给涉及数据集加排他锁（X)；
    对于普通SELECT语句，InnoDB不会加任何锁，事务可以通过以下语句显示给记录集加共享锁或排他锁。
 
 共享锁（S）：SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE。
 
 排他锁（X)：SELECT * FROM table_name WHERE ... FOR UPDATE。
 
 3）InnoDB行锁是通过给索引上的索引项加锁来实现的，因此InnoDB这种行锁实现特点意味着：只有通过索引条件检索数据，
    InnoDB才使用行级锁，否则，InnoDB将使用表锁！
 
 

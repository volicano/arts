## ARTS

##### Review

### MySQL Database Horizontal Scaling Solution


The constant development of cloud technologies, mature storage resources and computing resources, and gradually decreasing costs make it easier for enterprises to develop, deploy, and provide services. This benefits rapidly developing small and medium-sized enterprises that can respond to the increasing traffic by choosing to continuously add new machines to increase the application cluster.

However, with the development of enterprises, there is still a bottleneck which we cannot remove by simply piling up machines; this is the performance ceiling caused by a growing database. One of the effective measures to solve this performance ceiling is to shard the database, and to make sure the data volume of a single table is smaller than five million data entries.

Today, we will discuss this problem further. When an enterprise starts speeding up its growth, it might reach performance ceiling, even after sharding the database, in terms of both database capacity and the data volume of single tables. In this case, how can we expand our database performance?

###### Basic Concepts of Sharding

Sharding is an easily comprehensible concept; it refers to a certain set of rules that govern how to evenly distribute data from the original data table into multiple new data tables that have the same structure as that of the original table.

Let's assume that we have a user table (t_user) with 20 million data entries; this is 4 times the threshold value of 5 million data entries. How can we adjust this table by sharding?

As we have mentioned above, by using an averaging algorithm on one or more fields of the data table which return the identical results for each calculation, we can evenly distribute and separately store our data. There are many algorithms for this purpose. For example, we could conduct mod calculation on the ID, or split the table into multiple sub-tables by month based on the creation time.


As in the above figure, we have shown how to distribute data in the original table into different sub-tables of different sub-databases by using the f(x) function. Here, we will simply describe a sharding algorithm: id modulo

As we have shown in the above figure, we have three sub-databases and each sub-database has two sub-tables. Then we will uniformly distribute data with id % 3 == 0 to sub-database 1, and data with id % 2 == 0 to the first sub-table; we will continue this distribution for the remaining data in the similar manner.

###### Horizontal Scaling

What are the technical difficulties that database administrators (DBAs) face when performing horizontal scaling?


The most apparent problems are rule changes and data migration. If we complete the sharding rule within the application, then the rule change means that we must re-release the application cluster that uses the new rule. The trouble caused by data migration can be more serious. Also, before the completion of data migration we need to write a special script to export and import data. Different table relationships and different services will make this script extremely complex. We must also consider incremental data synchronization, time points, data consistency, and other problems. A minor error can have severe implications affecting user data.

Is there a sharding rule that does not require rule changes and data migration when scaling the database?

Absolutely. We can modify the sharding rule into sharding by segments. To understand this, please refer to the following figure. Assume that we have three sub-databases and each has two sub-tables (using id modulo as the sharding rule). This means we can store a total of 30 million data entries. The sharding rule is that data with IDs falling in the range of [1, 10000000] will be stored in sub-database 0, data with IDs falling in the range of [10000001, 20000000] will be stored in sub-database 1, and data with IDs falling in the range of [20000001, 30000000] will be stored in sub-database 2. When the number of data entries exceeds 30 million, we can simply add another sub-database to store data with IDs falling in the range of [30000001, 40000000] without doing anything to the three existing sub-databases.

Likewise, we can also add new sub-databases based on time.

The advantages of this sharding rule are obvious.

###### Disadvantages of Sharding

Think about it, what is the purpose of sharding? The performance of a single database cannot satisfy our daily service requirements. We need to allocate the performance pressure of a single database onto multiple databases. However, the sharding method we have described above will eventually concentrate all the insert/update pressure on one database instance and it cannot efficiently distribute performance pressure. Therefore, the third question comes.

Are there any sharding solutions that can both avoid data migration costs and solve the single-database performance hotspot issue?

The answer is yes. Let us describe some of Alibaba Cloud TDDL team's horizontal scaling modes for this purpose.


We have shown the first recommended horizontal scaling mode in the above figure. When we only have one sub-database, we can set four sub-tables. We have used the simple ID modulo method in this example. When a single database reaches its maximum capacity, we can add a database instance to migrate the entire sub-tables 2 and 3 into a new database instance. The advantage is that we can simply migrate entire tables to a new database. Further, we don't have to conduct any further calculations or worry about which database a single data entry must be migrated to because of rule changes. When two databases are insufficient, we can add two more sub-databases, and migrate tables 1 and 3 into the new sub-databases.

However, there is a drawback with the above solution. When we scale up from one database to four databases, the data volume of each single data table has also increased. When the data volume of a single table exceeds a specified range, it may cause performance problems. When we used up the reserved sub-tables and each physical database contains only one data table, then we must work on the data tables to see if further scaling is required.

To solve the drawbacks of mode 1, there is a mode 2:

There are many similarities between mode 1 and mode 2. In the scaling stage of mode 2 we still choose the migration of an entire table. To make it simple, we will use two sub-tables.

As we have shown in the above figure, we expanded the sub-database, and migrated the entire table 1 into the new database. If the volume of data in a single table approaches 5 million items, we can create another sub-table in each of the sub-databases to store data in excess of those 5 million items. Then the sharding rule is changed to:

Using id % 2 to determine the sub-database, and store data with IDs falling in the range of [5000001, 10000000] into table_0_1 and table_1_1. In this case, we can meet both the 5 million data-volume threshold and solve the hotspot database issue at the same time.

As time passes by, we may upgrade our database capacity again. To do this we can simply purchase a couple of instances and migrate table_0_1 and table_1_1 to the new instances. Likewise, we can create new sub-tables for each sub-database to solve the 5 million data-volume threshold problem.

The above are scaling solutions increase database capacity exponentially. The cost of such database resources might not be affordable for small and medium-sized enterprises. The cost for upgrading from a 2-instance solution to a 4-instance solution doubles and it doubles again when upgrading from the 4-instance solution to an 8-instance solution.

###### Non-Exponential Scaling

The principle is similar. For example, when we upgrade from a 2-instance solution to a 3-instance solution, there is every chance that the data volume of our table_0 and table_1 may have already reached 5 million items. We can purchase a new instance to store these two "legacy data tables," and then scale up the rest by using mode 2. In this case, the single database hotspot pressure gets evenly distributed to two databases. Of course, we can also add a sub-table for each database, to ensure that each sub-database assumes 1/3 of the total pressure. However, this mode is very complex in terms of sharding rules.


###### Conclusion
A good design usually facilitates future maintenance. Horizontal scaling of databases is a technical pain point. However, we can reduce the workload to some extent by optimizing our sharding policies. This principle is applicable in terms of code writing, product designing, or any other aspects of our daily lives. When we have a design that is hard to achieve, we must think twice whether this design is reasonable or not, and if there are better solutions.

https://dzone.com/articles/mysql-database-horizontal-scaling-solution
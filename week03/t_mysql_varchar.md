## ARTS

### Tip

mysql中varchar(n)中的n的大小对性能有影响吗？

例如比如本来10 就ok了，但是写成100，或者1000

varchar是存储可变长度的字符串，简单的说我们定义表机构的时候指定的字段长度是最大长度，当字符串没有达到最大长度的时候以字符串的实际长度来存储的，不占用多余的存储空间。
一般情况下使用varchar(10)和varchar(100)保存'fuk'占用的空间都是一样的，但是更小的varchar(10)有着更好的性能。

比如：MySQL建立索引时如果没有限制索引的大小，索引长度会默认采用的该字段的长度，也就是说varchar(100)建立的索引存储大小要比varchar(10)建立索引存储大小大的多，加载索引使用的内存也更多。

比如：Values in VARCHAR columns are variable-length strings. The length can be specified as a value from 0 to 65,535. The effective maximum length of a VARCHAR is subject to the maximum row size (** 65,535 bytes, which is shared among all columns**) 就是说，所有的列长度加起来不能操作65,535 bytes大小，如果你的varchar长度设置大了，那么给其他列使用的长度大小就小了。关于这个更多介绍，请看[column-count-limit](https://dev.mysql.com/doc/refman/5.7/en/column-count-limit.html)

实际中根据使用场景选择合适的长度就好。



## ARTS

### Tip

## mysql 的查询结果中新增一个字段

例如有表名为tab1,只有字段为a，b，想在查询结果中新增字段c(固定值为100)，可以这样写：
```
SELECT T.*,100 as c FROM tab1 T;
```
结果：
![图片](https://uploader.shimo.im/f/PHSFYNpUb4w9eO7P.png!thumbnail)

如果100为字符串则是：
```
SELECT T.a,T.b,'100' as c FROM tab1 T;
```


## ARTS

### Tip

## mysql合并重复列值（一对多合并查询）

没用一对多前的查询语句效果如图：
![图片](https://uploader.shimo.im/f/uFQvzGh2JSUwj1P7.png!thumbnail)
期望效果如图：

![图片](https://uploader.shimo.im/f/o1OqvZvRdXc6pNT0.png!thumbnail)

可以看到把重复的数据合并成一行，实现一对多的效果。

第一张图sql：
```
SELECT u.`name`,u.email,t.`name` as role from (SELECT a.user_id,o.`name` from sso_application_role_user a LEFT JOIN sso_application_roles r on a.app_role_id=r.id LEFT JOIN sso_roles o on o.id=r.role_id WHERE r.app_id=12) as t LEFT JOIN sso_users as u on u.id=t.user_id where u.`status`=1 ORDER BY u.`name`
```
第二张图sql：
```
SELECT u.`name`,u.email,GROUP_CONCAT(t.`name`) AS role from (SELECT a.user_id,o.`name` from sso_application_role_user a LEFT JOIN sso_application_roles r on a.app_role_id=r.id LEFT JOIN sso_roles o on o.id=r.role_id WHERE r.app_id=12) as t LEFT JOIN sso_users as u on u.id=t.user_id where u.`status`=1 GROUP BY u.`name`
```

具体用法：
select GROUP_CONCAT(A.title) as tablename from tmp A;      --默认的逗号分隔
select GROUP_CONCAT(A.title SEPARATOR  ' ') as tablename from tmp A;   --用空格分隔





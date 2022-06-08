# 实用语句系列——sql

## 获取最近一天内的100条新闻，按发布时间倒序
```sql
select *
from newstable
where abs(datediff(sysdate(), pubtime))<=1 Order by pubtime Desc limit 100;
```

## 插入时如果id值已存在则更新该记录
```sql
INSERT INTO table1 (id, name) VALUES (1, 'record1') ON DUPLICATE KEY UPDATE name = 'record1';
```

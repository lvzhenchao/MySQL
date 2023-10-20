# 1、explain是MySQL性能调优的工具

# 2、type字段所需数据使用的扫描方式【由快到慢】
- null
- system：系统表，少量数据，往往不需要进行磁盘IO
- const：常量连接 where id = 1
- eq_ref：主键索引(primary key)或者非空唯一索引(unique not null)等值扫描
- ref：非主键非唯一索引等值扫描
- range：范围扫描
- index：索引树扫描
- ALL：全表扫描(full table scan)

# 3、Extra字段
## Using where
- explain select * from user where sex='no';
- Extra为Using where说明，SQL使用了where条件过滤数据；如果type是all，优化方法，把where过滤的属性上添加索引 
## Using index
- explain select id,name from user where name='shenjian';
- SQL所需要返回的【所有列数据】均在一棵索引树上，而无需访问实际的行记录
## Using index condition
- explain select id,name,sex from user where name='shenjian';
- 该SQL语句与上一个SQL语句不同的地方在于，被查询的列，多了一个sex字段
- 确实命中了索引，但不是所有的列数据都在索引树上，还需要访问实际的行记录：覆盖索引
## Using filesort
- explain select * from user order by sex;
- 含义：得到所需结果集，需要对所有记录进行文件排序
- 典型的，在一个没有建立索引的列上进行了order by，就会触发filesort，常见的优化方案是，在order by的列上添加索引，避免每次查询都全量排序
## Using temporary
- explain select * from user group by name order by sex;
- 含义：需要建立临时表(temporary table)来暂存中间结果
- 典型的，group by和order by同时存在，且作用于不同的字段时，就会建立临时表，以便计算出最终的结果集
## Using join buffer (Block Nested Loop)
- explain select * from user where id in(select id from user where sex='no');
- 含义：需要进行嵌套循环计算。 画外音：内层和外层的type均为ALL，rows均为4，需要循环进行4*4次计算
- 典型的，两个关联表join，关联字段均未建立索引，就会出现这种情况。常见的优化方案是，在关联字段上添加索引，避免每次嵌套循环计算
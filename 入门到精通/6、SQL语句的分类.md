# 语句分类
- DDL (data defination language)数据定义语言；作用：创建、删除、修改 库表结构（重点）
`
    数据库的相关操作
    查看所有数据库：
    show databses
    切换数据库：
    use 数据库名
    创建数据库：
    create database 数据库名
    删除数据库名：
    drop database 数据库名

    创建数据表结构：
    create table 表名 （
        列名称 列类型 其他限制词
        ......
    ）
    列出表字段：
    desc 表名
    展示表结构：
    show create database 数据库名
    修改、新增字段及类型：
    alter table 表名 modify 字段名称 类型;
    alter table 表名 change 原列名 新列名 类型;
    alter table 表名 add 列名 类型 限制词 after 字段名称;
    删除表的某一列：
    alter table 表名 drop 列名
    修改表名：
    alter table 表名 rename (to) 新表名

    查看当前所有数据表：
    show tables;

    清除所有数据：
    Truncate 表名


`

- DML (data manipulation language)数据操作语言；作用：增、删、改表的记录（重点）
`
新增
insert into 表名 (列名1，列名2...) values (列值1, 列值2...); 列和值一一对应
    编码问题：
    show variables like '%char%'; 查看编码
    set names gbk; 修改当前客户端展示数据的编码：只是临时改

更新修改
update 表名 set 列1=列值1... where 条件
删除
delete from 表名 where 条件


`

- DCL (data control language)数据控制语言；作用：用户的创建以及授权
`
不同数据库要对应不同的用户，并且权限也不一样，主要是安全问题；线上尽可能不用root身份
    use mysql; 自带的数据库
    user 表
    select user, host from user; host是限制这个ip才能访问

    flush privileges; 如果删除掉用户，需要刷新权限

    mysqld --skip-grant-tables 跳过权限表，不验证权限；会重新开一个进程，直接就进到库里了

    创建用户
    create user '用户名'@'IP地址' IDENTIFIED BY '密码'； 如果让所有的IP使用，要用%
    用户授权
    grant 权限1，权限2... on 数据库名.* To 用户名 @ IP地址或%；如果是所有数据库：*.*
        grant update,insert,delete on shop.* to 'test'@'192.168.1.106';
        grant all privileges on *.* to 'test'@'192.168.10.1' 所有数据库，所有权限
    撤销权限
    revoke 权限1，权限2... on 数据库名.* from 用户名@IP地址
    查看权限
    show grants for 用户名@IP地址
    删除用户
    drop user 用户名@IP地址


`
- DQL (data query language)数据查询语言；作用：查询数据（重点）






























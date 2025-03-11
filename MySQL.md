# DDL----数据库操作语言

库相关:

```mysql
#查询所有数据库
show databases;
#查询当前数据库
select database();
#创建数据库
create database [if not exists] 数据库名 [default charset utf-8] [collate 排序规则];
#删除
drop database [if exists] 数据库名;
#使用
use 数据库名;
```

表相关:

```mysql
#查询当前数据库的所有表
show tables;
#查询表结构
desc 表名;
#查询指定表的建表语句
show create table 表名;
#建表
create table_name(
	id int comment '注释',
    name varchar(20) comment '名字'
);
```

数据库的类型:

```mysql
数值:
tinyint,1B
smallint, 2B
mediumint, 3B
int, 4B
bigint, 8B
float, 4B
double, 8B
declmal, 不确定
文本:
char,0~255B, 定长
varchar,0~65535B, 变长
...
```

修改表:

```mysql
#添加字段
alter table 表名 add 字段名 类型 [comment] [约束];
#修改指定字段的数据类型
alter table tb_name modify 字段名 新数据类型;
#修改字段名和字段类型
alter table tb_name change 旧字段名 新字段名 类型 [comment] [约束];
#设置列为主键
alter table orders add constraint 主键的新名字 primary key(要改成主键的列的名字);
```

# DML----数据操作语言

添加数据

```mysql
#指定字段添加
insert into tb_name (字段1, 字段2, ....) values (v1, v2, ...);
#给所有字段添加
insert into tb_name values (v1, v2, ...);
#批量添加数据
insert into tb_name (字段1, 字段2, ....) values 
(v1, v2, ...),
(v1, v2, ...),
(v1, v2, ...);
```

更新数据

```mysql
update tb_name set 字段1=v1,字段2=v2, ... [where 条件];
```

删除数据

```mysql
delete from tb_name [where 条件]
```

# DQL---数据查询语言

```mysql
select
	字段名1, ...
from
	tb_name1, ...
where
	条件
group by
	分组字段列表
having
	分组后条件列表
order by
	排序字段列表
limit
	分页参数;
```

基础查询

```mysql
select 字段1 [as 别名1],... from tb_name;
#去重
select distinct * from tb_name;
```

条件

```mysql
between ... and ...([min, max])
in(...)在某个范围内多选一
案例:
select * from emp where age=18 or age=20 or age=60;
select * from emp where age in (18,20,60);
like 占位符 (_单个字符, %多个字符)
案例:
select * from emp where name like '__';
is null (是空)
and->&&
or->||
not->!
```

聚合函数

```mysql
select func(字段1,...) from tb_name;
count() 统计
max()
min()
avg() 平均值
sum() 求和
```

分组查询

```mysql
select 字段 from tb_name [where ..] group by 分组字段名 [having 条件];
#案例
select gender, count(*) from emp group  by gender;
```

排序查询

```mysql
select 字段 from tb_name order by 字段1 排序方式1, ....;
select * from tb_name order by age desc;#倒序
```

分页查询

```mysql
select 字段 from tb_name limit 起始索引, 查询记录数;
```

#  DCL==管理用户

查询用户

```mysql
use mysql;
select * from user;
```

创建用户

```mysql
create user '用户名'@'主机名' identified by 'password';
```

修改密码

```mysql
alter user '用户名'@'主机名' identified with mysql_native_password by 'password'
```

删除用户

```mysql
drop user '用户名'@'主机名';
```



## 约束

```mysql
非空 not null
唯一 unique
主键 primary key
默认值 default
检查 check
外键 foreign key
自动增长 auto_increment
```

外键约束

```mysql
#创建时添加
create table tb_name(
	字段名 数据类型,
    ...
    [constraint] [外键名称] foreign key(外键字段名) references 主表(主表列名)
);
#添加外键
alter table tb_name add constraint 外键名称 foreign key (外键字段名) references 主表(主表列名);
#删除外键
alter table tb_name drop foreign key 外键名称;
```

<img src="C:\Users\白乃常\AppData\Roaming\Typora\typora-user-images\image-20240916120301650.png" alt="image-20240916120301650" />

# 多表查询


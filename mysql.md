# MYSQL版本的使用方法

#### 先切换到[mysql分支](https://github.com/tangshi/easysql/tree/mysql)，再看下文！

#### 1. 定义模型
 
一个模型就是一张表，所有的数据记录都是模型的一个实例，如下定义了一个用户模型。

```
# models.py

import time, uuid

from easysql.db import next_id
from easysql.orm import Model, StringField, BooleanField, FloatField, TextField

class User(Model):

	# 表的名称
    __table__ = 'users'  

	# 表的属性
    id = StringField(primary_key=True, default=next_id, ddl='varchar(50)')
    email = StringField(updatable=False, ddl='varchar(50)')
    password = StringField(ddl='varchar(50)')
    admin = BooleanField()
    name = StringField(ddl='varchar(50)')
    image = StringField(ddl='varchar(500)')
    created_at = FloatField(updatable=False, default=time.time)


```

#### 2. 初始化数据库

```
-- schema.sql

drop database if exists awesome;

create database awesome;

use awesome;

grant select, insert, update, delete on awesome.* to 'www-data'@'localhost' identified by 'www-data';

create table users (
    `id` varchar(50) not null,
    `email` varchar(50) not null,
    `password` varchar(50) not null,
    `admin` bool not null,
    `name` varchar(50) not null,
    `image` varchar(500) not null,
    `created_at` real not null,
    unique key `idx_email` (`email`),
    key `idx_created_at` (`created_at`),
    primary key (`id`)
) engine=innodb default charset=utf8;

create table blogs (
    `id` varchar(50) not null,
    `user_id` varchar(50) not null,
    `user_name` varchar(50) not null,
    `user_image` varchar(500) not null,
    `name` varchar(50) not null,
    `summary` varchar(200) not null,
    `content` mediumtext not null,
    `created_at` real not null,
    key `idx_created_at` (`created_at`),
    primary key (`id`)
) engine=innodb default charset=utf8;

create table comments (
    `id` varchar(50) not null,
    `blog_id` varchar(50) not null,
    `user_id` varchar(50) not null,
    `user_name` varchar(50) not null,
    `user_image` varchar(500) not null,
    `content` mediumtext not null,
    `created_at` real not null,
    key `idx_created_at` (`created_at`),
    primary key (`id`)
) engine=innodb default charset=utf8;

```

把SQL脚本放到MySQL命令行里执行：

`$ mysql -u root -p < schema.sql`

#### 3. 操作数据库

```

# test_db.py

from models import User

from easysql import db

db.create_engine(user='www-data', password='www-data', database='awesome')

# 新建一条记录
u = User(name='Test', email='test@example.com', password='1234567890', image='about:blank')

# 将记录插入 User 表中
u.insert() 
print 'new user id:', u.id

# 查找记录
u1 = User.find_first('where email=?', 'test@example.com')
print 'find user\'s name:', u1.name

# 删除记录
u1.delete()

```



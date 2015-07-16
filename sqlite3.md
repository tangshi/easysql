# SQLITE3版本的使用方法

#### 先切换到[sqlite3分支](https://github.com/tangshi/easysql/tree/sqlite3)，再看下文！

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
$ sqlite3 /path/to/awesome.db

sqlite> create table users (
    `id` varchar(50) not null,
    `email` varchar(50) not null,
    `password` varchar(50) not null,
    `admin` bool not null,
    `name` varchar(50) not null,
    `image` varchar(500) not null,
    `created_at` real not null,
    primary key (`id`) 
  );

$ .tables

```

#### 3. 操作数据库

```
# test_db.py

from models import User
from easysql import db

# sqlite3 是基于文件的数据库，初始化时要指定数据库的文件名
db.create_engine(database='/path/to/awesome.db')

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

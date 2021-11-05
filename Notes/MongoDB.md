# 

## 创建

进入容器mongodb的文件夹

```bash
docker exec –it 容器名(或id) /bin/bash
```

使用docker进入mongodb

```bash
docker exec -it mongodb01[现在运行的容器名]  mongo[镜像名]
```



```bash
use test
```

当数据库存在时,则使用该数据库,否则创建该数据库

## 创建自定义用户

```bash
# (1) 给其他数据库test配置一个读写权限的用户
use test
db.createUser(
  {
    user: "myTester",
    pwd: "xyz123",
    roles: [ { role: "readWrite", db: "test" },
             { role: "read", db: "reporting" } ]
  }
)

# (2)mytest连接和验证。
mongo --port 27017 -u "myTester" -p "xyz123" --authenticationDatabase "test"
# 或登录后执行命令验证
use test
db.auth("myTester","xyz123")
```

## 查看已经创建的用户

```bash
# 查看创建的用户需要在管理员数据库上执行
use admin
db.system.users.find()

# 查看当前数据库用户
show users
```

## 删除用户

```bash
# 删除一个用户
use dbname
db.system.users.remove({user:"username"})

# 删除管理员
use admin
db.system.users.remove({user:"admin"})
```



## 查询

查询所有的数据

```bash
show dbs
```

## 创建指定数据库用户权限

### 进入amdin数据库

```bash
use admin
```

```bash
db.auth('amdin','123456')超级用户验证
```

创建指定数据库的用户权限,指定为test的用户权限

```bash
db.createUser(
	{
		user: "trent",
		pwd: "123456",
		roles: [ 
				{ role: "readWrite", db: "test" }
        ...可以是多个
        { role: "readWrite", db: "test10" }
		]
  } 
)
```

切换用户验证

```bash
db.auth('trent','123456')
```

使用该用户下的数据库

```bash
use test
```

向test数据库中插入一条数据

```bash
db.test.insert({"name":"te"})
```

查询test数据库

```bash
db.test.findOne()
```


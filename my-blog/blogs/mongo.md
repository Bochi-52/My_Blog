---
title: Mongo 课堂练习
date: 2021-11-10
tags:
 - mongo
categories:
 - 课堂练习
---



## 课堂练习

```
use mydb

db.bbs.insert({
	"id":1,
	"title":"MongoDB学习与交流",
	"des":"本帖专注于MongoDB学习与交流",
	"author":"武林盟主",
	"net":"http：//xxxx.html",
	"label":["技术交流","mongoDB","NoSql"],
	"love_num":"",
	"comment":[
		{user:"会飞的猪",
		 message:"lalalala",
		 like:true
		},{
		 user:"会飞的猪",
		 like:true
		}]
})

db.bbs.find()
db.bbs.find().pretty()
```

## 实验一

### (1) 创建数据库和集合

> **使用命令创建数据库myDB和集合collection1**

```
use myDB
db.createCollection("collection1")
```

> **使用命令创建数据库testDB和集合collection2**

```
use testDB
db.createCollection("collection2")
```

### (2) 查看数据库

> **使用命令查看当前所有数据库**

```
show dbs
```

> **使用命令切换到数据库testDB**

```
use testDB
```

> **使用命令显示当前的数据库**

```
db.getName()
db.current
```

### (3) 删除数据库

> **使用命令删除当前数据库testDB**

```
use testDB
db.dropDatabase()
```

> **使用命令查看当前所有数据库**

```
show dbs
```

### (4) 创建集合

> **使用命令切换到数据库myDB**

```
use myDB
```

> **使用命令显式创建集合students**

```
db.createCollection("students")
```

> **使用命令显式创建集合teachers，要求该集合大小固定为1G，集合中最多允许1万条记录**

```
db.createCollection("teachers",{capped:true,size:1073741824,max:10000})
```

>  **使用命令隐式创建集合employees，该集合里面有一条记录【姓名：张三，年龄：25，职位：程序员，部门：研发部，工资：9000】**

```
db.employees.insert({
	"id":1,
	"name":"张三",
	"age":25,
	"job":"程序员",
	"department":"研发部",
	"salary":9000
})
```

### (5) 查看集合

> 使用命令查看当前数据库当中的所有集合（两种命令都要截图）

```
show collections
show tables
```

### (6) 删除集合

> 使用命令删除集合employees

```
db.employees.drop()
```

> 使用命令查看当前数据库当中的所有集合（使用一种命令即可）

```
show collections
show tables
```

## 实验二

###  (1) 创建数据库mydb，给指定的集合添加文档。

```
use mydb
```

> 1. 用insert()向students集合中添加：_id为1001，姓名为张三，年龄为20的文档
>
> 2. 用insert()向students集合中添加：姓名为尼古拉斯赵四，年龄为40的文档。
>
> 3. 和save()向students集合中添加：姓名为尼古拉斯赵四，年龄为20的文档。
>
> 4. 查询students表中的内容

```
db.students.insert({"_id":1001,"name":"张三","age":20})
db.students.insert({"name":"尼古拉斯赵四","age":40})
db.students.save({"name":"尼古拉斯赵四","age":20})
db.students.find().pretty()
```

> 5. 用insert()向students集合中添加：_id为1001，姓名为李白，年龄为30的文档。
>
> 6. 用save()向students集合中添加：_id为1001，姓名为李白，年龄为30的文档。
>
> 7. 向students中添加自己的姓名，性别，班级，年龄信息，婚否等信息。
>
> 8. 向students中添加：姓名-张三，性别-男，年龄-19，地址-武昌区，父母-(姓名-张三他爸，年龄-49，工作-经商，姓名-张三他妈，年龄-45，工作-教师)，所选课程-[Java，MySQL]，成绩-78。注：本题添加要有缩进，一目了然。
>
> 9. 查询students表中的内容。

```
db.students.insert({"_id":1001,"name":"李白","age":30})
db.students.save({"_id":1001,"name":"李白","age":30})
db.students.insert({"_id":1052,"name":"石鑫鑫",“sex”:"男","class":"软工1904","age":20，"marriage":false})

db.students.insert({
	"name":"张三",
	“sex”:"男",
	"age":19，
	"address":"武昌区",
	"parents":[
		{"name":"张三他爸","age":49,"job":"经商"},
		{"name":"张三他妈","age":45,"job":"教师"}],
	"course":["Java","MySQL"],
	"score":78})

db.students.find().pretty()
```

> 10. 向test集合添加10000条数据，数据的内容为女1号-女10000号
```
for(i=1;i<10000;i++){
	db.test.insert({"name":"女"+i+"号"})
}
db.test.find().pretty()
```

### (2) 查找文档

> 删除之前的数据，丰富数据

```
db.students.deleteMany({})
db.students.insert({"name":"李白","sex":"男","age":12,"address":"西安"})
db.students.insert({"name":"张三","sex":"男","age":13,"address":"石家庄"})
db.students.insert({"name":"李四","sex":"女","age":10,"address":"武汉"})
db.students.insert({"name":"王五","sex":"男","age":20,"address":"北京"})
db.students.insert({"name":"赵六","sex":"男","age":23,"address":"黑龙江"})
db.students.insert({"name":"市鑫鑫","sex":"男","age":21,"address":"成都"})
db.students.insert({"name":"喳喳会","sex":"女","age":28,"address":"重庆"})
db.students.insert({"name":"凶温馨","sex":"男","age":26,"address":"天津"})
db.students.insert({"name":"要双","sex":"女","age":25,"address":"衡水"})
db.students.insert({"name":"阳挂","sex":"女","age":35,"address":"美国"})
db.students.insert({"name":"混军","sex":"女","age":45,"address":"英国"})
db.students.insert({"name":"落差","sex":"男","age":55,"address":"日本"})
db.students.find().pretty()
```

> 1. 使用命令查看students集合中姓名为张三的文档
>
> 2. 使用命令查看students集合中姓名为张三年龄为40的文档
>
> 3. 使用命令查找students集合中的男生的记录
>
> 4. 使用命令查找students集合中的第一条记录

```
db.students.find({"name":"张三"}).pretty()
db.students.find({$and:[{"name":"张三"},{"age":40}]}).pretty()
db.students.find({"sex":"男"}).pretty()
db.students.findOne()
```

### (3) 删除文档

> 1. 使用命令显示students集合中的文档数
>
> 2. 使用命令删除students集合中姓名为张三，年龄为13的文档
>
> 3. 使用命令删除test集合中的所有文档

```
db.students.find().count()
db.students.remove({$and:[{"name":"张三"},{"age":13}]})
db.test.deleteMany({})
```

### (4) 更新数据库

> 1. 数据准备，向users集合插入数据

```
db.users.deleteMany({})
db.users.insert({"_id":1001,"name":"张三丰","sex":"男","age":70,"college":"老年大学"})
db.users.insert({"_id":1002,"name":"张无忌","sex":"男","age":30,"college":"武当学院"})
db.users.insert({"_id":1003,"name":"赵敏","sex":"女","age":30,"college":"皇家学院"})
db.users.insert({"_id":1004,"name":"周芷若","sex":"女","age":18,"college":"峨眉学院"})
db.users.insert({"_id":1005,"name":"宋青书","sex":"男","age":32,"college":"武当学院"})
db.users.insert({"_id":1006,"name":"方世玉","sex":"男","age":30,"college":"少年学院"})
db.users.insert({"_id":1007,"name":"于谦","sex":"男","age":32})
db.users.insert({"_id":1008,"name":"郭德纲","sex":"男","age":32})
db.users.find().pretty()
```

> 2. 显示users集合当中方世玉的名字修改为洪七公
> 3. 显示users集合当中方世玉的名字修改为洪七公，college改为“家里蹲”

```
db.users.find({"name":"方世玉"}).pretty()

db.users.update({"name":"方世玉"},{"name":"洪七公","college":"家里蹲"})
db.users.update({"_id":1006},{"name":"洪七公","college":"家里蹲"})

db.users.find({"_id":1006}).pretty()
```

> 4. 使用$set修改器将姓名为于谦的记录添加一个爱好字段，于谦有三大爱好：抽烟，喝酒，烫头

```
db.users.update({"name":"于谦"},{$set:{"hobby":["抽烟","喝酒","烫头"]}})
db.users.find({"_id":1007}).pretty()
```

> 5. 使用$set修改器将年龄为32的所有记录college改为清华大学

```
db.users.update({"age":32},{$set:{"college":"清华大学"}},false,ture)
db.users.find({"college":"清华大学"})
```

> 6. 使用upsert参数将姓名为郭德纲的年龄添加10岁
>
> 7. 使用upsert参数将姓名为郭德纲的college更新为德云社
>
> 8. 将郭德纲的name字段改为fullname

```
db.users.update({"name":"郭德纲"},{$inc:{"age":10}})
db.users.update({"name":"郭德纲"},{$set:{"college":"德云社"}})
db.users.update({"name":"郭德纲"},{$rename:{"name":"fullname"}})
db.users.find({"_id":1008})
```

> 9. 利用$unset删除 姓名为于谦的爱好字段

```
db.users.update({"name":"于谦"},{$unset:{"hobby":""}})
db.users.find({"_id":1007})
```












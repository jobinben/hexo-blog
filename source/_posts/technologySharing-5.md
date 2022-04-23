---
title: MongoDB修仙录
date: 2022-04-23 19:17:30
tags:
---
## 创建库的步骤

    在MongoDB中，集合(`collections`)代表的就是MySQL里的表。
    文档(`documents`)代表的是就是MySQL里的行。


创建库时, `use 库名`, 就是进入一个数据库，如果没有这个数据库，用`use`进入这个库后，插入一条数据，便可以创建了这个库并添加一条数据。

## 炼气期:

### （1）查询数据

1. 查询数据库，`show dbs`

2. 增加一条数据, `db.表名.insert({"name":"dabing", "age": 18})`

3. 查询数据库里的集合(也就是表), `show collections`

4. 查询集合的数据,`db.表名.find()`

<!--more-->

5. `db.表名.find({age:18})` 查询指定age等于18的数据

6. `db.表名.find({age:{$gt:18}})` 查询age高于18的数据

7. `db.表名.find({age:{$gte:18, $lte:20}})`查询age>=18且age<=20的数据

8. `db.表名.find({name:/da/})` 模糊查询name包含da的数据

9.  `db.表名.find({}, {name:1,age:1})` 查询指定列的数据，这条语句就是只显示`name`列和`age`列,花括号`{}`里面还可以加条件

10. `db.表名.find().sort({age:1})` 查询按照age排序，`1`升序 `-1`降序

11. `db.表名.find().limit(5)` 查询前5条数据

12. `db.表名.find().skip(10)` 查询10条以后的数据，这个结合`limit()`适合做`分页查询`。

13. `db.表名.find().count()` 查询表里有多少条数据，可以加条件就针对条件的统计多少条数据，`db.表名.find({age:{$gt:18}}).count()`统计年龄大于18的数据多少条

14. `db.表名.find({$or:[{age:18}, {age:20}]})` 查询age=18或者age=20的数据

15. ` db.user.find({name:"jobin"}).explain("executionStats")`   "explain("executionStats")" 分析查询速度。

    

### （2）插入数据

1. `db.表名.insert({name:'jobin'})`  若插入的数据主键已经存在，则会抛 org.springframework.dao.DuplicateKeyException 异常，提示主键重复，不保存当前数据。
2. `db.表名.insertOne({age:18})` `插入一条数据
3. `db.表名.insertMany([{age:18}, {age:20}])` 插入多条数据

### （3）更新数据

1. `db.表名.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})` 修改第一条发现的数据
2. `db.表名.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}}, {multi: true})` 修改发现的多条数据

### （4）删除数据

1. `db.collections(表名/集合).remove({name:'jobin'},justOne:1)` 删除一条`name=jobin`的数据，`justOne:true`可选，不选就是删除匹配条件的全部数据。
2. `remove() `方法已经过时了，现在官方推荐使用 `deleteOne()` 和 `deleteMany() `方法。
3. `db.collections.deleteMany({})`删除集合下全部文档(数据)
4. `db.collections.deleteMany({ name : "A" })`删除 `name = A` 的全部文档(数据)
5. `db.collections.deleteOne( { name: "D" } )`删除` name = D `的一个文档(数据)


## 筑基期:

    下面的collections是集合名或表名。

### （1）索引

    注意在 3.0.0 版本前创建索引方法为 db.collection.ensureIndex
    ()，之后的版本使用了 db.collection.createIndex() 方法，
    ensureIndex() 还能用，但只是 createIndex() 的别名。

1. `db.collections.ensureIndex({"username": 1})` 创建索引
2. `db.collections.ensureIndex({"username":1, age: -1})` 创建复合索引，`1和-1`还是代表升序和降序的意思。第一个参数是`username`，索引会基于`username`，当你单独查询`username`时，也可以命中索引。
3. `db.collections.getIndexes()` 获取当前集合的索引
4. `db.表名.dropIndex({"username": 1})` 删除索引

#####  新的创建索引方法

1. `db.collections.createIndex({"username": 1})` 创建索引
2. `db.collections.createIndex({"username":1, age: -1})` 创建复合索引，`1和-1`还是代表升序和降序的意思。第一个参数是`username`，索引会基于`username`，当你单独查询`username`时，也可以命中索引。
3. `db.values.createIndex({"username":1, age: -1}, {background: true})`在后台创建索引：建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background" 可选参数。 "background" 默认值为false。

### （2）唯一索引

1. `db.collections.ensureIndex({"username": 1}, {"unique": true})` 创建唯一索引，关键字`unique`。唯一索引相当于主键，`username`列被创建了唯一索引，那么`username`的值不能存在重复，当插入一条`username`的数据时和之前的有重复值的话，会抛出错误。

### （3）权限

##### 创建超级管理用户:

1. 第一步: 
   1. `use admin` 进入admin系统数据库
   2. `db.createUser({user: 'admin', pwd: '123456', roles:[{role: 'root', db: 'admin'}]})` 创建超级用户，账户为admin,密码为123456 规则为超级管理员，对应的数据库是admin。

2. 第二步: 
   1. 找到MongoDB的配置文件(mongod.cfg),进行备份。`
   2. 本人的配置路径是:`D:\after-tools\mongoDB\server\bin
   3. 然后用对mongod.cfg文件添加开启权限验证语句:`security: authorization: enabled`
   4. 现在如果自己连接mongo的话,是无法查看到数据库
   5. 然后再cmd输入`services.msc`对MongoDB服务进行重启，就可以更新好配置
3. 第三步:
   1. 尝试直接Mongo直接连接数据库，进入后将无法查看到数据库
   2. 如果想连接上数据库有以下方式连接:
   3. `mongo admin -u 用户名 -p 密码` 或者 `mongo 127.0.0.1:27017/test -u 用户名 -p 密码`
   4. `mongo admin` 直接进入库, 然后再验证 `db.auth('用户名', '密码')`

#### 创建数据库管理用户

    也就是普通用户，允许这个用户只能操作某个库的权限

      1. `user testdb` 进入testdb数据库
      2. `show users` 查看是否已经创建了用户
      3. `db.createUser({user: 'dabing', pwd: '123456', roles:[{role: 'dbOwner', db: 'testdb'}]})` 创建普通用户，账户为dabing,密码为123456，规则为数据库管理角色，对应的数据库是testdb。
      4. `db.dropUser('用户名')` 删除此用户的权限
      5. 登录操作跟上述一样。

## 结丹期

### （1）Aggregation 管道操作符与表达式

1. 管道操作符：
   1. `$project`: 增加、删除、重命名字段
   2. `$match`: 条件匹配，只满足条件的文档才能进入下一阶段
   3. `$limit`: 限制结果的数量
   4. `$skip`: 跳过文档的数量
   5. `$sort`: 条件排序。
   6. `$group`: 条件组合结果、统计
   7. `$lookup`: 用以引入其他集合的数据（表关联查询）

2. ` db.order.aggregate([{$project:{order_id:1, uid: 1}}])`: 只查询指定字段
3. `> db.order.aggregate([{$project:{order_id:1, all_price: 1}}, {$match: {all_price: {$gt:50}}}])`: 查询指定字段并all_price>50
4. ` db.order.aggregate([{$group:{_id: "$order_id", total: {$sum:"$all_price"}}}])`: 统计总价格并分组
5. `db.order.aggregate([{$project:{order_id:1, all_price: 1}}, {$match: {all_price: {$gt:50}}}, {$sort:{"order_id": -1}}])`: 降序
6. `db.order.aggregate([{$project:{order_id:1, all_price: 1}}, {$match: {all_price: {$gt:50}}}, {$limit:1}])`:显示一条数据
7. `db.order.aggregate([{$project:{order_id:1, all_price: 1}}, {$match: {all_price: {$gt:50}}}, {$skip:1}])`: 跳过一条数据

#### `$lookup`表关联

`db.order.aggregate([{$lookup:{ from: "order_item", localField:"order_id", foreignField:"order_id", as:"items"}}])`: 
`from`: '要关联的表', `localField`: '当前表的字段', `foreignField`: '关联表的字段', `as`: '一个放查询到关联表数据的数组'

`db.order.aggregate([{$lookup:{ from: "order_item", localField:"order_id", foreignField:"order_id", as:"items"}}, {$project: {order_id:1, trade_no:1, items:1}}])`:
显示指定的字段，但是`items`:1,必须指定加上，才能显示出关联表的结果信息


### （2）数据库的备份和还原

#### 无设置密码版本

备份导出:

`mongodump -h ip地址 -d 数据库 -o 导出路径`

还原导入:

`mongorestore -h ip地址 -d 数据库 导入路径` 

#### 有设置密码版本

备份导出:

`mongodump -h ip地址 -d 数据库 -u 数据库 -p 密码 -o 导出路径`

还原导入:

`mongorestore -h ip地址 -d 数据库 导入路径 -u 数据库 -p 密码` 


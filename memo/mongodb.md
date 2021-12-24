## MongoDB

#### 连接
>mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]

#### 显示所有库
>show dbs

#### 数据库不存在，则创建数据库，否则切换到指定数据库
>use DATABASE_NAME

#### 删除库
>db.dropDatabase()

#### 创建集合
>db.createCollection(name, options)

#### 删除集合
>db.collection.drop()

#### 插入
>db.COLLECTION_NAME.insert(document)  

或  

>db.COLLECTION_NAME.save(document)

#### 更新
 ``` shell
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
 ```

 #### 删除文档
 ``` shell
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
 ```

 #### 查询文档
 >db.collection.find(query, projection)  
 >db.col.find().pretty()  
 >db.collection.findOne(query, projection) 

 | 操作 | 格式 | 范例 | RDBMS中的类似语句 |
 | ---- | ---- | ---- | ---- |
 | 等于 | {<key>:<value>} | db.col.find({"by":"菜鸟教程"}).pretty() | where by = '菜鸟教程' |
 | 小于 | {<key>:{$lt:<value>}} | db.col.find({"likes":{$lt:50}}).pretty() | where likes < 50 |
 | 小于或等于 | {<key>:{$lte:<value>}} | db.col.find({"likes":{$lte:50}}).pretty() | where likes <= 50 |
 | 大于 | {<key>:{$gt:<value>}} | db.col.find({"likes":{$gt:50}}).pretty() | where likes > 50 |
 | 大于或等于 | {<key>:{$gte:<value>}} | db.col.find({"likes":{$gte:50}}).pretty() | where likes >= 50 |
 | 不等于 | {<key>:{$ne:<value>}} | db.col.find({"likes":{$ne:50}}).pretty() | where likes != 50 |

 #### 排序
 >db.COLLECTION_NAME.find().sort({KEY:1})

 ### pymongo4.0

#### 连接
```python
myclient = pymongo.MongoClient("mongodb://username:{}@10.123.11.22:27017/database".format(urllib.parse.quote_plus('123@abc')))
```

#### 进库
```python
mydb = myclient["database"]
```

#### 建/选collection
```python
mycol = mydb["todo_list"]
```

#### 查找第一条
```python
x = mycol.find_one()
```

#### 格式化显示
```python
print(json_util.dumps(x,indent=2))
```

#### 带条件查找结果总数
```python
mycol.count_documents({'timestamp': {'$gt': '2021-11-11 10:00:00'},'$or':[{'level':'ERROR'},{'level':'FATAL'}]})
```


---
layout: post
title: MongoDB常用基础命令
categories: nosql
tags: nosql mongodb
---

* content
{:toc}

## 1、单条件查询
```
db.logEntity.find({'logType':"interfaceLog"})
```
## 2、多条件查询
```
db.logEntity.find({'logType':"interfaceLog","createTime":{$gte:"2019-10-31",$lte:"2019-10-31"}})  
.sort({"createTime":-1}).limit(10).skip(1)
1 为指定按升序创建索引， -1为按降序创建索引。
```
## 3、按条件删除
```
db.logEntity.remove({'logType':"interfaceLog"}})
```
## 4、创建索引
```
db.logEntity.createIndex({createTime:-1})
1 为指定按升序创建索引， -1为按降序创建索引。
```
## 5、按条件统计数据
```
db.logEntity.count({logType:'userlog',createTime: {$gte:'2019-11-1',$lte:'2019-11-11'}})
```
## 6、按条件统计去重后的数据
```
db.logEntity.distinct('userName',{logType:'userlog',createTime: {$gte:'2019-11-1',$lte:'2019-11-11'}}).length
```
## 7、分组统计group
```
db.logEntity.aggregate([
    {$match:{logType:'userlog',createTime: {$gte:'2019-11-1',$lte:'2019-11-11'}}},
    {$group:{
        _id: '$userName',
        acc:{$sum: 1}
    }},
    {$sort: {acc: -1}},
    {$limit:100},
    {$skip:0}
])
```
## 8、模糊查询
查询 title 包含"教"字的文档：
```
db.col.find({title:/教/})
```
查询 title 字段以"教"字开头的文档：
```
db.col.find({title:/^教/})
```
查询 titl e字段以"教"字结尾的文档：
```
db.col.find({title:/教$/})
```


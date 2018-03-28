### mongodb索引详讲

1.创建索引  
```db.[documentName].ensureIndex({"key":1/-1}):1为正序索引，-1为倒序索引```  

2，索引需要注意的地方  
  创建索引的时候注意1是正序创建索引-1是倒序创建索引  
  索引的创建在提高查询性能的同事会影响插入的性能，对于经常查询少插入的文档可以考虑用索引  
  符合索引要注意索引的先后顺序  
  每个键全建立索引不一定就能提高性能呢，索引不是万能的  
  在做排序工作的时候如果是超大数据量也可以考虑加上索引用来提高排序的性能


3,唯一索引  
唯一索引的意义就是可以确保集合的每个文档的指定键都有唯一值。  
```db.[documentName].ensureIndex({"keyName":1},{unique:true})```  
建立唯一索引之后，文档中就不能插入重复的数值,insert和save都会报错

4，剔除重复值  
在建立唯一索引之前有可能在文档中已经存在重复数值了，处理方式  
```db.[documentName].ensureIndex({"keyName":1},{unique:true,dropDups:true})```

但是在mongodb3.0版本之后，dropDups已经不支持了；使用下列方法解决：
```
  用以下方法解决在重复数据的集合里建立唯一索引
  1.首先把集合数据导出
  mongoexport -d uxgourmet -c CollectedUrl -o CollectedUrl.json 
  2.删除集合数据
  db.CollectedUrl.remove({})
  3.在集合上创建唯一索引
  db.CollectedUrl.ensureIndex({uri:1},{unique:true})
  4.把导出数据再次导入集合中
  mongoimport -d uxgourmet -c CollectedUrl --upsert D:/CollectedUrl.json   
  注意要加上--upsert选项
  --upsert   insert or update objects that already exist
```
5,如何详细查看本次查询使用那个索引和查询数据的状态信息  
```
     db.[documentName].find({"keyName":""}).explain()
     
     “cursor” : “BtreeCursor name_-1“ 使用索引
      “nscanned” : 1 查到几个文档
      “millis” : 0 查询时间0是很不错的性能
```  

6,后台执行创建索引的过程  
因为创建索引的过程中会暂时锁表（表不能访问查询等）问题，可以将建立索引的过程放在后台处理  
```db.[documentName].ensureIndex({"keyName":1},{background:true})```  

7,批量和精确的删除索引  
```
  db.runCommand({dropIndexes:"documentName",index:"index_name"})  
  db.runCommand({dropIndexes:"documentName",index:"*"})
  
```

ensureIndex()创建索引  
getIndexes()查看索引  
dropIndex删除索引

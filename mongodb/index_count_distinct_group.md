### mongodb索引, 去重，分组详讲

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
  db.[documentName].dropIndex({"indexName":1})
```

8.mongodb中建立二维索引，也就是地图索引  
建立二维索引:  
```db.[dcumentName].ensureIndex({"gis":"2d"},{min:-1,max:201})```  
- $near查询，按照目标点查询距离最近的最多100个点  
```db.map.find({gis:{$near:[70,80]}},{_id:0,gis:1}).limit(3)  查出最近的3个点坐标```  
- $within:$within可以代替$near来查询一个形状之内的点，也支持$box(矩形)，$center(圆形)  
```
 db.map.find({gis:{$within:{$box:[ [50,50], [190,190] ]}}},{_id:0,gis:1})  
 db.map.find({gis:{$within:{$center:[50,50],70}}},{_id:0,gis:1})
```

ensureIndex()创建索引  
getIndexes()查看索引  
dropIndex删除索引

### count，distinct,group  
- count:在find的查找中，可以根据相应的查找条件，获得数据条数  
```db.[documentName].find({查询器}).count():返回一个数字，表示查询匹配的数目```  

- distinct:去重，获取集合中指定字段的不重复值，并以数组的形式返回  
具体用法1,：  
```
db.collection_name.distinct(field,query,options)

 

field -----指定要返回的字段(string)

query-----条件查询(document)

options-----其他的选项(document)
```

2,利用runCommand进行调用distinct  
```db.runCommand({distinct:"documentName",key:"field"}).values```

- group:分组统计  
```
db.runCommand({group:{
	           ns:集合名字,
              Key:分组的键对象,
              Initial:初始化累加器,
              $reduce:组分解器,
              Condition:条件,
              Finalize:组完成器
      }})
      分组首先会按照key进行分组,每组的 每一个文档全要执行$reduce的方法,
      他接收2个参数一个是组内本条记录,一个是累加器数据.
1.key： 这个就是分组的key 
2.initial： 每组都分享一个初始化函数，特别注意：是每一组initial函数。 
3.reduce： 这个函数的第一个参数是当前的文档对象，第二个参数是上一次function操作的累计对象。有多少个文档， $reduce就会调用多少次。 
4.condition： 这个就是过滤条件。 
5.finalize： 这是个函数，每一组文档执行完后，多会触发此方法。
```
```很完整的调用参照：
db.runCommand({group:{
	ns:"persons",
	$keyf:function(doc){
		if(doc.counTry){
			return {country:doc.counTry}
		}else{
			return {country:doc.country}
		}
	},
	initial:{m:0},
	$reduce:function(doc,prev){
		if(doc.m > prev.m){
			prev.m = doc.m;
			prev.name = doc.name;
			if(doc.country){
				prev.country = doc.country;
			}else{
				prev.country = doc.counTry;
			}
		}
	},
	finalize:function(prev){
		prev.m = prev.name+" Math scores "+prev.m
	},
	condition:{m:{$gt:90}}
}})
```



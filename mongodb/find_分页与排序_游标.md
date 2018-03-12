### find详讲，分页与排序，游标和其他知识

### find详讲 
1.  指定返回的键  
db.[documentName].find({条件},{键指定}) 
条件：使用查询操作符指定查询条件  
键指定：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。在这里有点像没用，只是指定了需要返回的键

数据准备  --->persons.json  
在这个查找中，默认会把_id添加到查找结果中，如果不想出现的话就把```_id:0```添加到键指定  

1.1 查询出所有数据的指定键  
db.persons.find({},{name:1,age:1,country:1,_id:0})


2.  查询条件  
比较操作符号:  
```
小于：$lt    {age:{$lt:22}}  
小于等于：$lte {age:{$gte:22,$lte:27}}  
大于：$gt  
大于等于：$gte  
不等于：!=  $ne  {age:{$ne:25}}
```

3.查询条件  
2.1 查询出年龄在25与27之间的学生    
      ```db.persons.find({age:{$gte:25,$lte:27}},{_id:0,age:1,name:1,country:1})```
      
      {age:{$gte:25,$lte:27}}:或的关系直接在一个括号写就可以  
      
2.2 查询出所有不是韩国学生的数学的成绩  
    ``` db.persons.find({country:{$ne:"Korea"}},{_id:0,m:1,country:1})```  
    
4.包含或者不包含  
$in或$nin：  两个作用对象是数组不是其他对象
2.3 查询国籍是中国或美国的学生信息
  ```db.persons.find({country:{$in:["China","USA"]}})```  
  
2.4 查询国籍不是中国或美国的学生信息  
    ```db.persons.find({country:{$nin:["China","USA"]}})```  
    
 5.OR查询  
 $or:他  接收一个数组  
 2.4  查询语文成绩大于85或英语大于90的学生  
 ```db.persons.find({$or:[{c:{$gt:85}},{e:{$gt:90}}]},{_id:0,e:1,c:1,name:1})```  
 
 6.NULL  
 把中国国籍的学生上增加新的键sex  
 ```db.persons.update({country:"China"},{$set:{sex:"m"}},false,true)```  
 
 2.5  查询出所有sex等于NULL的学生  
 ```db.persons.find({sex:null},{_id:0,country:1,name:1})```  
 ```db.persons.find({sex:{$in:[null]}},{_id:0,country:1,name:1})```
 
 7  正则查询  
 2.6查询出名字中存在"li"的学生的信息
 ```db.persons.find({name:/li/i},{_id:0,name:1})```  
 
8.  $not的使用  
 $not可以用到任何地方进行取反操作  
 2.7 查询出名字中不存在"li"的学生的信息  
 
 ```db.persons.find({name:{$not:/li/i}},{_id:0,name:1})```  
 $not和$nin的区别在于$not 可以用在任何地方，而$nin是用到集合上的  
 
 9. 数组查询$all和index应用  
 2.8 查询喜欢看MONGODB和JS的学生  
 ```db.persons.find({books:{$all:["JS","MONGODB"]}},{_id:0,name:1,books:1})```  
 2.9 查询第二本是是JAVA的学生信息  
 ``` db.persons.find({"books.1":"JAVA"},{_id:0,name:1})```  
 
 10. 查询指定长度数组$size他不能与比较查询符一起使用（这是弊端）  
  2.10 查询出喜欢的书籍是4本的学生  
  ``` db.persons.find({books:{$size:4}},{_id:0,name:1})```  
  
  2.10 查询出喜欢的书籍数量大于3本的学生  
  1.增加字段size  
  ```db.persons.update({},{$set:{size:4}},false,true)```  
  
  2.改变书籍的更新方式，每次增加书籍的时候，size加1  
  ```db.persons.update({},{$push:{books:"ORACLE"},$inc:{size:1}})```  
  
  3.利用$gt进行查询  
  ``` db.persons.find({size:{$gt:3}})```  
  
  2.11 利用shell查询出jim喜欢看的书的数量  
 ```
 var persons = db.persons.find({name:"jim"})
while(persons.hasNext()){
	obj = persons.next();
        print(obj.books.length)
} 
```


      课间小结
               1.mongodb 是NOSQL数据库但是他在文档查询上还是很强大的
               2.查询符基本是用到花括号里面的更新符基本是在外面
               3.shell是个彻彻底底的JS引擎,但是一些特殊的操作要靠他的
                   各个驱动包来完成(JAVA,NODE.JS)
                   
                   
 ---------------------------------------------------
 11.	$slice操作符返回文档中指定数组的内部值  
   2.11 查询出jim书架中第2~4本书  
   ``` db.persons.find({name:"tom"},{books:{$slice:[1,3]},_id:0})```  
   
   2.12	 查询出最后一本书  
   ``` db.persons.find({name:"tom"},{books:{$slice:-1},_id:0})```
   
11.文档查询  
    
      为jim添加学习简历文档 jim.json 
      
      2.13查询出在K上过学的学生
      
      1. 这个我们用绝对匹配可以完成,但是有些问题(找找问题?顺序?总要带着score?)
           db.persons.find({school:{school:"K",score:"A"}},{_ id:0,school:1})
      2.为了解决顺序的问题我可以用对象”.”的方式定位
           db.persons.find({"school.score":"A","school.school":"K"},{_ id:0,school:1})
      3.这样也问题看例子:
 	    db.persons.find({"school.score":"A","school.school":”J”},{_ id:0,school:1})
          同样能查出刚才那条数据,原因是score和school会去其他对象对比
      4.正确做法单条条件组查询$elemMatch
           db.persons.find({school:{$elemMatch:{school:"K",score:"A"}}})

 



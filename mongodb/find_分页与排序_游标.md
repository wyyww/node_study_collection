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
 
 2.7  正则查询  
 2.6查询出名字中存在"li"的学生的信息
 

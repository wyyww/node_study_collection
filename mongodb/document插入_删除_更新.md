### 数据插入，删除，更新

####  撤入数据
1.  插入文档  
     db.[documentName].insert({_id:"001",name:"yangyyy"})
     
2.  批量插入文档  
    在shell中db.[documentName].insert([{}，{},{}])是错误的，这样插入的是一个数组  
    在shell中不支持批量插入  
    想批量插入数据可以使用mongo的应用驱动或者shell的for循环：  
    ```
    for(var i=0; i<10; i++){
     db.persons.insert({name:i})
    }```
    
    
3.save操作  
 save操作和insert操作区别在于的当遇到_id相同的情况下  
 save完成保存操作  ，并且覆盖之前的数据  
 insert则会报错
 
 ____________________________
 
### 删除数据
1.  删除列表中的所有数据

    db.[documentName].remove({空})  
    集合本身和索引不会删除  
    
 2. 根据条件删除数据  
    db.[documentName].remove({});  全部删除
    ``` db.text.remove({name:”uspcat”})```  
    
    
  3.小技巧  
  > 如果你想删除一个数据量十分庞大的集合，直接删除该集合并且重新建立索引的方法，比直接用remove的效率高很多；使用drop删除
  
  
  注意：现在remove()官方已经过时了，推荐使用deleteOne()和deleteMany()方法  
  如删除全部文档：  
  ```db.[documentName].deleteMany({})```  
  删除status为A的所有文档:   
  ```db.[documentName].deleteMany({ status : "A" })```  
  删除status为D的一个文档:  按照集合顺序删除第一个  
  ```db.inventory.deleteOne( { status: "D" } )```  
  
________________________
  
  
### 更新数据
  
  1. 强硬的文档替换式更新操作   
          db.[documentName].updata({查询器},{修改器}):会直接覆盖之前的操作，文档全部替换（_id除外）
        
  2. 主键冲突的时候会报错并且停止更新操作  
        因为是强硬替换，当替换的文档和已有的文档ID冲突的时候，则系统会报错
      
  3.insert Or Update操作  
    目标：查询器查出来数据就执行更新操作，查不出来就创建操作
    用法：db.[documentName].updata({查询器},{修改器}, true )
    
   4.批量更新   
     默认情况下当查询器查询出*多条数据*的时候默认就修改第一条数据，而实现批量修改
     
       db.[documentName].({查询器},{&set:{修改器}},false,true);
          批量更新只能用$set等操作符进行更新
          
          
  5. 使用修改器来完成局部更新操作  
  ![image](https://github.com/wyyww/wxyyww_images/blob/master/screenShot/Screenshot_2018-03-11-15-27-28.png)    
  
  set:有这个键则实行更新，没有这个键则添加这个键  
  inc:可以对数字数据进行追加，自动增加  
  unset:删除某个键  
  push:数组追加元素  
  1.如果指定的键是数组则追加新的值；  
  2. 如果指定的键不是数组则中断当前操作  
  3. 如果不存在指定的键则创建数组类型的键值对  
  pushAll:批量往数组中追加元素  
  addToSet:当数组中存在此项则不操作，不存在则添加此项
  pop:从指定数组删除一个值，若1则删除最后一个数值，若-1则删除第一个数值  
  pull:删除一个指定的数值  
  pullAll:
  
 ![image](https://github.com/wyyww/wxyyww_images/blob/master/screenShot/Screenshot_2018-03-11-15-58-12.png)    
  
  
6.$addToSet与$each结合完成批量数组更新  
   db.text.update({_ id:1000},{$addToSet:{books:{$each:["JS","DB"]}}})  
   
   $each会循环后面的数组把每一个数值进$addToSet操作  
   
7.   存在分配与查询效率  （一定要理解）
     当document被创建的时候，DB为其分配内存和预留内存当修改操作  
     不超过预留内存的时候则速度非常快反而超过了就要分配新的内存则会消耗时间

8.runCommand函数与findAndModify函数   
     runCommand函数可以执行mongoDB中的特殊函数  
     findAndModify就是特殊函数之一，它用于返回update或者remove后的文档  
 ```
     runCommand({
          findAndModify:"progresses",
          query:{查询器},
          sort{排序},
          new:true,
          update{更新器},
          remove:true
     }).value
     
     例如：
      ps = db.runCommand({
          findAndModify:"persons",
          query:{name:"text"},
         
          update{$set:{email:"1221"}},
          new:true
     }).value
 ```
     局限就是findAndModify一次只能更新一次文档，不能批量处理文档

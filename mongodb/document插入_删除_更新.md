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

    db.[documentName].remove()  
    集合本身和索引不会删除  
    
 2. 根据条件删除数据  
    db.[documentName].remove({});
    
  3.小技巧  
  > 如果你想删除一个数据量十分庞大的集合，直接删除该集合并且重新建立索引的方法，比直接用remove的效率高很多；使用drop删除
  
  
  ________________________
  
  
  ### 更新数据
  
  1. 强硬的文档替换式更新操作   
          db.[documentName].updata({查询器},{修改器}):会直覆直接覆盖之前的操作，文档全部替换了,如何局部更新  
          
  2. 主键冲突的时候会报错并且停止更新操作  
        因为是强硬替换，当替换的文档和已有的文档ID冲突的时候，则系统会报错
      
  3.insert Or Update操作  
    目标：查询器查出来数据就执行更新操作，查不出来就替换操作  
    用法：db.[documentName].updata({查询器},{修改器}, true )
    
   4.批量更新   
     默认情况下当查询器查询出*多条数据*的时候默认就修改第一条数据，而实现批量修改
       db.[documentName].({查询器},{修改器},false,true);
          
          
  5. 使用修改器来完成局部更新操作  
  ![image](https://github.com/wyyww/wxyyww_images/blob/master/screenShot/Screenshot_2018-03-11-15-27-28.png)    
  
  set:有这个键则实行更新，没有这个键则添加这个键  
  inc:可以对数字数据进行追加，自动增加  
  unset:删除某个键  
  push:数组追加元素  
  1.如果指定的键是数组则追加新的值；  
  2. 如果指定的键不是数组则中断当前操作  
  3. 如果不存在指定的键则创建数组类型的键值对
  

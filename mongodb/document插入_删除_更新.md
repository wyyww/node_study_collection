### 数据插入，删除，更新

####  撤入数据
1.  插入文档  
     db.[documentName].insert({_id:"001",name:"yangyyy"})
2.批量插入文档  
    在shell中db.[documentName].insert([{}，{},{}])是错误的，这样插入的是一个数组  
    在shell中不支持批量插入  
    想批量插入数据可以使用mongo的应用驱动或者shell的for循环：  
    ```
    for(var i=0; i<10; i++){
    ... db.persons.insert({name:i})
    .}
```
    
3.save操作  
    save操作和insert操作区别在于的当遇到_id相同的情况下  
    save完成保存操作  ，并且覆盖之前的数据
    insert则会报错
  
### 删除数据
1.  删除列表中的所有数据

    db.[documentName].remove()  
    集合本身和索引不会删除  
    
 2. 根据条件删除数据  
    db.[documentName].remove({});
    
  3.小技巧  
  > 如果你想删除一个数据量十分庞大的集合，直接删除该集合并且重新建立索引的方法，比直接用remove的效率高很多；使用drop删除

## 配置shell客户端

shell的内部就是JavaScript客户端
mongo 127.0.0.1:27017/admin


1 创建一个数据库  

  user [databaseName]:但是你什么也不干就离开的话这个空数据库就会被删除；他只是在缓存区待着，不干任何活退出则自动删除了
  
2 查看所有数据库

    ```show dbs```
    
3 给指定数据库添加集合并且添加记录

    ```db.[docuemntName].insert({...})```
    
     db.persons.insert<{name:"uspacr"}>：当使用一个数据库的时候，db指代的就是当前数据库，在此会创建一个persons的集合,  
      并添加一个name信息，同时mongodb会自动添加一个id
      
4 查看数据库中的所有文档

    show collections
    
    db.system.indexes.find()://我创建的数据库并没有system.indexes这个集合 
    
5 查询指定文档的数据

   查询所有的文档数据 db.[docuemntName].find()
    
   查询第一条数据 db.[docuemntName].findOne()
   
6 更新文档数据

    db.[documentName].update({查询条件},{更新内容})
    
    例子：db.persons.update({name:"extjs"},{$set:{name:"extjs333"}}):set用来修改数据，也可以添加内容  
    
          db.persons.update({name:"extjs"},{$set:{age:23,name:"extjs333"}})
          
7 删除文档中的数据(可以删除任何数据)

    db.[documentName].remove({})

8.  删除数据库中的集合

    db.[docuemntName].drop()
    
9.删除数据库  

    db.dropDatabase()
    
10. Shell的help  
    里面有所有的shell可以完成的命令帮助  
     全局的help数据库相关的db.help()集合相关的db.[docuemntName].help()
     
 11.mongoDB的API   
    通过db.stats()获取数据库状态  
    db.help()  
    db.[documentName].help()获取集合可用的操作
    
 12.  数据库和集合命名规范  
 *  不能是空字符串
 *  不得含有'',（空格）、，、$、/、\、和\O（空字符）
 *  应全部小写
 *  最多64个字节
 *  数据库名不能与现有的系统保留库同名，如admin,local,config
 
 这样的集合名字也是合法的：  
 db-text但是不能通过db.[documentName]得到了  
 需要改名为db.getCollection(docuemntName);  
 因为db-text需要被当成减法操作
 
 13.  mongoDB的shell内置JavaScript引擎可以直接执行JavaScript代码，可以使用eval函数  
      db.eval()

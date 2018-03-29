### mongodb中的命令执行器，固定集合，GridFs文件系统，服务端脚本

#### 命令执行器runCommand  
- 在mongodb中rumCommand可以用来执行大部分的操作  
```db.rumCommand({drop:"map"}):删除一个集合```

- 查询MongoDB提供的命令  
```db.listCommands()```

- mongodb中常用命令集合  
```
         3.1查询服务器版本号和主机操作系统
         db.runCommand({buildInfo:1})    
         3.2查询执行集合的详细信息,大小,空间,索引等……
         db.runCommand({collStats:"persons"})
         3.3查看操作本集合最后一次错误信息
         db.runCommand({getLastError:"persons"})
```

#### 固定集合  
```
 2.1固定集合默认是没有索引的就算是_id也是没有索引的
 2.2由于不需分配新的空间他的插入速度是非常快的
 2.3固定集合的顺是确定的导致查询速度是非常快的
 2.4最适合的是应用就是日志管理
```

2,创建一个固定集合  
创建一个固定集合，要求大小为100字节，可以存储10个文档  
``` db.createCollection("myColl",{size:100,capped:true,max:10})```  
把一个普通集合转化为一个固定集合  
``` db.runCommand({convertToCapped:[documentName],size:100000})```


3,对一个固定集合，反向排序，默认排序方式是插入顺序排序  
``` db.myColl.find().sort({$natural:-1})```

4,尾部游标,可惜shell不支持java和php等驱动是支持的  
这是个特殊的只能用到固定级和身上的游标,他在没有结果的时候也不回自动销毁他是一直等待结果的到来


#### GridFs文件系统    入门介绍
概念：    GridFS是mongoDB自带的文件系统他用二进制的形式存储文件大型文件系统的绝大多是特性GridFS全可以完成  

使用GridFs:  
- 在cmd下查看GridFs支持的功能  
``` mongogiles --help```  
- 上传一个文件  
``` mongofiles -d foobar -l "C:\projectDirectory\a.txt" put "a.txt"```  
- 在shell下不支持查看上传的文件，只能通过mongodb的客户端或者mongoVUE来查看上传的文件  
```  shell不支持没法看
3.4查看文件内容
     C:\Users\thinkpad>mongofiles -d foobar get "a.txt“
     VUE可以查看,shell无法打开文件
3.5查看所有文件
     mongofiles -d foobar list
3.5删除已经存在的文件VUE中操作
     mongofiles -d foobar delete 'a.txt'
     
     集合查看
     db.fs.chunks.find() 和db.fs.files.find() 存储了文件系统的所有文件信息

```


#### 服务端脚本  
```
1.Eval
    1.1服务器端运行eval,已经移除
          db.eval("function(name){ return name}","uspcat")
2.Javascript的存储
    2.1在服务上保存js变量活着函数共全局调用
          1.把变量加载到特殊集合system.js中
             db.system.js.insert({_id:"name",value:”uspcat”})
          2.调用
             db.eval("return  name;")
System.js相当于Oracle中的存储过程,因为value不单单可以写变量
还可以写函数体也就是javascript代码
```


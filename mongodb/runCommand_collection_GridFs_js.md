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

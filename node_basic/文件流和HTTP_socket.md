### 文件流的概念，HTTP服务器模块，sockets相关

### 文件流
> 流的概念:程序中输入或输出的一个连续的字节序列;  
> 文件流就是以面向对象的概念对文件数据进行的抽象  
> 文件流定义了一些对文件数据的操作方式  
API:  实际上和通过buffer读取文件一样，是通过缓冲区一点一点取
fs.createReadStream() =>得到一个ReadbleStream  
fs.createWriteStream()=>得到一个WritableStream

比如从一个水桶往另外一个水桶倒水的方式：
* 直接倒进去
* 通过缓冲区一点一点取
* 通过管道化进行

大文件直接读取的问题：
* 大文件拷贝，内存受不了
* 没有进度显示


####  文件读取写入的两种好用方式
```
const fs = require('fs');
const path = require('path');

let readStream = fs.createReadStream(path.join(__dirname,'../github.css'));
let writeStream = fs.createWriteStream(path.join(__dirname,'../github1.css'));

1,//buffer流的方式进行读取，写入
readStream.on('data', (chunk) => {
    
  writeStream.write(chunk,(err) =>{
    console.log('写入中');
  })
});
readStream.on('end',(err) =>{
  console.log("读取结束")
})

2,//管道的方式进行读取写入
writeStream.on('pipe',(src) =>{
  console.log(src === readStream);
})
//管道的方式写入,支持链式编程,但每次写入的到的需要是一个管道流
readStream
      .pipe(writeStream)

```


### socket基础
>   socket就是指客户端和服务端之间*建立*的*双向的* *抽象的*连接；socket也只是一种连接，可以同时获取客户端和服务的的信息  

*   客户端和服务端的区别就在于发送请求
*   创建一个服务器必须要有一个监听端口，否则服务器没有任何意义
*   端口的监听会有优先原则，先占用的端口不能再被别人使用
*   telnet客户端：window系统中，可以用来与服务端建立连接的客户端

建立一个socket服务端：
```
//建立一个socket链接

const net = require('net');

//创建一个socket服务器
const serer = net.createServer(socketConnect);

//当客户端与我有连接的时候才会触发函数
function socketConnect(socket){
    var client = socket.address();
    console.log(client.address);
}

const port = 2080;
 //监听特定的端口
serer.listen(port,(err) =>  {
    //成功监听 2080 端口之后执行，如果监听失败（端口被别人占用了）
    if(err){
        console.log('端口被占用了')
    }else{
        console.log(`服务器正常启动${port}`);
    }
})
```

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

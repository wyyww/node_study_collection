### 核心模块，包，文件操作，文件流，网络操作
> 核心模块的意义：如果仅仅在服务器运行JavaScript代码，意义并不大，无法实现任何功能（文件操作，网络访问）   

##### Node中的常用核心模块
* path：用于处理文件路径
* fs:用来操作文件系统（CRUD）
* chile_process：用来创建子进程，现在系统多为多核CPU，而node不能充分利用多核系统，多用这个模块进行硬件虚拟化(将多核系统虚拟成多个单核系统)
* util:提供了一些实用的小工具，常用于变量的类型判断
* http:提供HTTP服务器的功能；在Node中不需要http服务器，如PHP使用apache服务器，java使用javaEE
* url:用于解析URL,如主机号，端口，查询字符串等
* querystring：解析URL中的查询字符串；常用于解析查询过程中的url的查询字符串
* crypto:提供加密和解密功能

### NPM
> 包的概念在于将一些优先设计好的功能或者说API封装到一个文件夹，并提供给开发者使用

##### 包的加载机制
* 先在系统核心模块中查找（优先级最高），不要创建和现有的包重名的包
* 再然后到node_modules目录中查找，一般npm加载的就从node_modules查找

##### nvm管理node和npm管理包的问题
> nvm用来管理node的版本，而npm是跟随node一起安装的，使用npm全局安装的包会默认安装到当前所用node版本所在目录下，如果nvm改变使用的node版本，则全局的包在新的版本中不可用  ; npm config ls -l

> 将NPM配置为全局的目录：npm config set prefix C:\dev\nodejs；配置过后还需要将该路径配置到环境变量中，否则无法使用  
> 添加nrm到系统中，用来设置各个镜像包的地址
常用npm命令：  
* npm install
* npm init
* npm search
* npm info:可以传上包的id,打印包的信息
* npm list：打印项目所有的依赖项,未下载的会警告
* npm outdated:判断包是否更新
* npm update:更新包
* npm cache:缓存的包

------------------

### 文件操作
> --在文件操作的过程中，都“必须”使用物理路径(绝对路径)  
> --相关模块：fs,path,readline(读取大文本，一行一行地读),fs-extra(第三方)  

##### path中的一些常用属性
* path.basename(path[, ext]):截取路径的文件名
* path.delimiter：获取不同操作系统中默认的路径分隔符，window是；，Linus是：. 和 process.env.Path.split(path.delimiter)使用,末尾为空成员 
* path.dirname(path):获取当前目录名称  
* path.extname(path):获取后缀名，如，'.html','.txt'  
* path.isAbsolute(path)  
* path.join([...paths]):拼接路径  
* path.normalize(path)  ：常规化一个地址，去除斜线，转义斜线什么的，主要为window设计
* path.parse(path):转化，将路径字符串转换为一个对象，包含文件目录，文件名，文件扩展名  
* path.format(pathObject)  :将对象路径转成字符串  
* path.posix  
* path.relative(from, to)  ：获取后面相对于前面路径的一个相对路径
* path.resolve([...paths])  ：绝对路径，跟join差不多
* path.sep:获取当前操作系统中默认的路径分隔符；'foo/bar/baz'.split(path.sep);  
* path.win32

####  文件读取的同步和异步，文件编码的问题
> 区别就在于同步会阻塞代码的执行，异步则不会阻塞

##### Buffer,缓冲区处理二进制
> --Node中需要操作文件，网络通信是不可能完全通过字符串的方式进行操作，内存不够用，因此引入了二进制的缓冲区
* -- 缓冲区的概念就是内存中操作数据的容器，只是数据容器而已 
* -- 通过缓冲区可以很方便的操作二进制文件  
* -- 打文件操作的时候必须有缓冲区  
* -- readFile的方式确实是使用Buffer，但也是一次性读取  
* --读取文件时没有指定编码默认读取的是一个Buffer

> 文件编码可能出现乱码：只有中文才会有编码问题；  使用*iconv-lite*库进行编码转换
> 读取文件的时候，直接读取出现的就是二进制的buffer流，而不是中文数据；解决的方式就是  

 ```  
 fs.readFile(path.join(_ _ dirname,'../module.js'),(err,data) => {`  
 `if(err) throw err;`  
 `console.log(data.toString('utf8'))`  
 `})
 ```


#### 通过Buffer流的方式读取大文件
```
//通过流的方式读取文件  
const fs = require('fs');  
const path = require('path');  
var iconv = require('iconv-lite');  
const readline = require('readline');  
let filename = path.join(__dirname,'./beyond.txt');  
let readStreamer = fs.createReadStream(filename);  
let data ='';  
readStreamer.on('data',(chunk) =>{  
// console.log(chunk.toString());  
data +=iconv.decode(chunk, 'gbk');  
})  
readStreamer.on('end',() =>{  
console.log(data);  
})
```


对于大文件来说，如果通过直接都去进行操作，若文件过大必然会有阻塞卡顿，通过文件流的方式读取可以节省内存消耗和时间，具体读取过程如下：
* 在内存中根据系统创建一个Buffer读取流（buffer大小由系统指定）
* 从文件中每次读取和buffer空间大小相同的内容，读取到缓冲区中
* 通过buffer的data事件监听每次读取缓冲区的内容，取到每次读取的文件片段
* 用字符串将文件的片段chunk进行拼接
* 通过buffer的end事件判断文件读取结束，并对文件内容进行处理

#### 通过readline进行逐行读取
```
//通过流的方式读取文件 
const fs = require('fs');  
const path = require('path');  
var iconv = require('iconv-lite');  
const readline = require('readline');  
let filename = path.join(__dirname,'./beyond.txt');   
let readStreamer = fs.createReadStream(filename).pipe(iconv.decodeStream('gbk'));  
//确定每次给你的就是一行内容  
const rl = readline.createInterface({input: readStreamer});  
rl.on('line', (line) => {  
   console.log(`Line from file: ${line}`);  
 });
 ```


#### 文件写入
> 同步，异步，写入流进行写入
* fs.writeFile(path.join(_ _ dirname,'../temp.txt'), JSON.stringify({id:10}), (err) => {}),异步写入文件
* fs.writeFileSync(file, data[, options])，同步写入文件
* 通过文件流的方式写入文件  
> var streamWriter = fs.createWriteStream(path.join(__dirname,'../temp.text'));  
setInterval(() =>{  
streamWriter.write("hello",() =>{  
   console.log(+1);  
   })},1000);  

> 文件写入可能出错的地方：意外错误，文件权限问题，文件夹找不到（Node不会自动创建文件）

 #### 追加文件
 > fs.appendFile(path.join(_ _ dirname,'../temp.txt'), JSON.stringify({id:10}), (err) => {})
 
 追加和写入的区别是：写入默认的是覆盖原本的文件
 
 
 #### 打印当前目录下所有的文件信息
 ```
 //打印当前目录下所有文件  
 const fs = require('fs');  
 const path = require('path');  
 //获取当前有没有传入目标路径  
 var target = path.join(__dirname,process.argv[2] || './')  
 //为了避免文件大小导致的读取时间不同，可使用同步读取来避免交叉结果    
 fs.readdir(target,(err,files) => {  
    files.forEach((file)=>{  
    // console.log(path.join(target,file));  
    fs.stat(path.join(target,file),(err,stats) => {  
    console.log(`${stats.mtime }\t${stats.ctime}\t${file}`)  
    })  
    })  
 })
 ```
 
 #### 自己实现一个创建层级目录
 > 因为系统之间的差异，在windows和Linux系统下的现象不同，一般出错都在于Windows  
 * 不同系统之间的文件分隔符，如："/","\\"
 * 传入路径为相对路径和绝对路径的处理
 * 创建文件时候需要判断文件是否存在
 * Windows中创建很多层目录导致目录无法删除，处理方法：使用winArR添加压缩，设置为添加压缩后删除文件，再把压缩包删除
 
 mkdirs('demo2/demo3/demo4');
 ``` 
 //创建层级目录

const fs = require('fs');
const path = require('path');

//创建文件，定义模块成员，导出模块成员，载入模块，使用模块

function mkdirs(pathname,callback){
    //首先需要判断的就是传入的是否为一个绝对路径，不需要加前缀了
    pathname = path.isAbsolute(pathname) ? pathname : path.join(__dirname,pathname);

    //获取需要创建的部分
    // pathname = pathname.replace(__dirname,'');
   let relativePath = path.relative(__dirname,pathname);//目录对比,获取创建目录,注意posix中不同系统的差异   posix(linux系统)
    // console.log(relativePath)
    let folders = relativePath.split(path.sep);//[ 'demo1', 'demo2' ]

    
    try{
        var pre = '';
        folders.forEach( folder =>{
          
            fs.mkdirSync(path.join(__dirname,pre,folder))
            pre = path.join(pre,folder);
        })
        callback && callback(null);
    }catch(error){
        callback && callback(error);
    };
   
}

module.exports = mkdirs;
```
 
----------------------

#### 文件中的__dirname不能乱用
> 上面的案例中虽然可以创建层级目录，但是如果案例调用路径书写方式修改则就无法运行，原因就在于__dirname的使用上

mkdirs('./demo2/demo3/demo4');
```
function mkdirs(pathname,callback){
   
    // console.log(module.parent)//拿到的就是调用我的js文件路径
    var root = path.dirname(module.parent.filename);
   

    
    pathname = path.isAbsolute(pathname) ? pathname : path.join(root,pathname);

   let relativePath = path.relative(root,pathname);
 
    let folders = relativePath.split(path.sep);//[ 'demo1', 'demo2' ]

    
    try{
        var pre = '';
        folders.forEach( folder =>{
          
            //判断文件是否存在
        //    if(!fs.existsSync(path.join(root,pre,folder))){
        //        //文件目录不存在
        //        fs.mkdirSync(path.join(root,pre,folder))
        //    }

           try{
            //如果文件不存在
            fs.statSync(path.join(root,pre,folder));
           }catch(error){
            fs.mkdirSync(path.join(root,pre,folder));
           }
            pre = path.join(pre,folder);
        })
        callback && callback(null);
    }catch(error){
        callback && callback(error);
    };
   
}
```


### 文件监视
#### 通过文件监视实现自动转换markdown文件转换；*marked*插件可以进行转换

> 文件打开之后是内存当中存在，保存就是把内存的内容在保存到磁盘

实现思路：
* 通过文件监视watchFile监视指定md文件
* 当文件内容发生变化的时候，借助“marked”包提供的“markdown” to “html”功能将改变后的md文件转换成html文件
* 再将得到的HTML替换到模板中
* 最后利用Browser Sync模块实现浏览器自动刷新

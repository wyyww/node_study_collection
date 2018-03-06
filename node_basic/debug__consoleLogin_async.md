### debug调试  
调试的目的是为了*排除bug*  

#### node调试的方式  
* 最原始的方式：console.log() 
* 通过node的原生对象debug进行调试，n表示继续进行，watch可以监听某个数据的变化，有一些其他的属性用来帮助调试；*使用很少*  
* visualCode调试：小蜘蛛调试，通过visualCode左侧的小蜘蛛提供调试功能；  
* 第三方调试工具：--npm install node-inspector  -g通过Google浏览器进行调试，比较麻烦    --npm install devtool -g有问题

### 使用Node模拟两次^c实现退出当前应用环境  
`  
setInterval(() =>{
    process.stdout.write('1\n');
  },1000);
  var exiting = false;
  process.on('SIGINT',()=>{
    if(exiting){
      //退出node进程
      process.stdout.write('退出');
      process.exit();
    }
    else{
      //第一次按下
      process.stdout.write('再次按下^C退出');
      exiting = true;
      
      setTimeout(() => {exiting = false;},2000);
    }
  })`  
  > 这里有两点需要注意的，第一点就是使用SIGINT来捕获当前进程接收到的信号（比如crtl+C）,第二点就是两次^C之间是在一定时间间隔内按下才能实现退出

### 控制台登录  
> 所谓控制台登录就是指网站接收用户输入账户和密码，将其和账户进行对比进行登录的一系列操作。


### 异步回掉函数


### 非阻塞异步  

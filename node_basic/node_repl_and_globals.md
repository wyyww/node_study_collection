## node入门全局作用于以及全局变量和函数  

### node REPL  

repl是nodejs中的一个模块（Read-Eval-Print-Loop），repl在node中既可以作为一种独立的运行程序运行相对应的代码，也能够包含在其他的应用程序中作为程序的一部分使用;  
相当于node的一个开发环境,和chrome中的控制台具有类似的功能。  
>+repl在我们日常的开发中主要担当的就是脚本的调试，测试以及自身代码的检测等.  

在node中我所熟知的几个常用的功能如下：  
* 作为常用的计算器使用   
* 测试使用，在node中全局对象global的对象都会在repl中，执行global即可读取global全部对象  
* 执行js脚本  
* node --use_strict :使用严格模式的js脚本  
* .exit(): 推出node的repl环境  
* _ :下划线指取到上一次运算的结果

### Console  

console属于nodejs中的一个输出到可控制台的模块，node中的console和浏览器端的console是属于不同的模块；  
node中有可能会常用的参数如下：  
* log() 将内容输出到终端  
* log("%s") 控制相关的字符串输出  
* 将错误信息输出到文件  
* time()等将输出代码的执行时间

### 全局对象  

在node中需要注意的全局对象  
1. global
>在浏览器中，顶级作用域就是全局作用域，但是在node中，顶级作用域不是全局作用域，使用var定义的变量只是该模块的本地内容；  
>浏览器中顶级作用域为window，在node中为global
2.  console
3.  process：用于获取当前的*node进程信息*，一般用于获取环境变量之类的  
  * argv:用于获取当前进程的命令行参数数组，第一二个参数分别为node和node的执行js文件，后面才是输入的参数
   * process.env：指向当前shell的*环境变量*，比如process.env.HOME。
  * process.pid：当前进程的进程号。
  * process.version：Node的版本，比如v0.10.18。
  * process.platform：当前系统平台，比如Linux。
  * process.title：默认值为“node”，可以自定义该值。
  * process.execPath：运行当前进程的可执行文件的绝对路径。
  * process.stdout：指向标准输出。
  * process.stdin：指向标准输入。
  * process.stderr：指向标准错误。  
  >process.stdout.write() 向控制台输出指定内容  
  >node如何实现清空控制台：  
  * 通过getWindowSize()获取控制台宽高，然后打印等高的换行实现；  
  * 通过process.stdout.write('\033[2J')和process.stdout.write('\033[0f')  
  
  ### node中的全局函数
   *     setInteval(callback,millisecond)   
  * clearInterval(timer)
  * setTimeout(callbak,millisecond)
  * clearTImeout(timer)
  * Buffer:Class  
  > -用于操作二进制数据  
  

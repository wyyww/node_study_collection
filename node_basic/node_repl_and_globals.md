## node入门全局作用于以及全局变量和函数  

### node REPL  

repl是nodejs中的一个模块（Read-Eval-Print Loop），repl在node中既可以作为一种独立的运行程序运行相对应的代码，也能够包含在其他的应用程序中作为程序的一部分使用；  相当于node的一个开发环境。  
>+repl在我们日常的开发中主要担当的就是脚本的调试，测试以及自身代码的检测等.  

在node中我所熟知的几个常用的功能如下：  
* 作为常用的计算器使用   
* 测试使用，在node中全局对象global的对象都会在repl中，执行global即可读取global全部对象  
* 执行js脚本  

### Console  

console属于nodejs中的一个输出到可控制台的模块，node中的console和浏览器端的console是属于不同的模块；  
node中有可能会常用的参数如下：  
* log() 将内容输出到终端  
* log("%s") 控制相关的字符串输出  
* 将错误信息输出到文件  
* time()等将输出代码的执行时间

### 全局对象  

在node中需要注意的全局对象  
* global
>+在浏览器中，顶级作用域就是全局作用域，但是在node中，顶级作用域不是全局作用域，使用var定义的变量只是该模块的本地内容；浏览器中顶级作用域为window，在node中为global
* console
* process

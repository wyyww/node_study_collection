什么是nodeJS？

自从2009年node的问世以来，一直备受关注，在GitHub上的项目数也是急速增长，基于node的社区，npm也成为全球最大的开源社区。

# nodeJS简介

### 客户端的JavaScript是什么样的  
一，客户端JavaScript是什么？  
>+脚本语言  
>+运行在浏览器中  
>+一般用来做客户端的交互（interactive）  
>+JavaScript是一种弱类型、动态类型、基于原型，广泛用于客户端浏览器的解释性脚本语言。

二，JavaScript的运行环境？  
>+是不是运行在浏览器？  
>+浏览器说法不够严谨  
>+运行在浏览器内核中的JS引擎（engine）

-   浏览器的作用：  
1，当用户打开浏览器输入url地址，将url封装成对应的请求报文  
2，服务器端返回响应报文时，对响应报文进行解析，主要包括：  
    >+渲染HTML  
    >+渲染css  
    >+渲染页面  
    >+解析JS
>+所以说浏览器最大的作用就是将url地址封装成请求报文并发出去，接收响应报文并解析渲染到页面上！！！！

 三，浏览器中的JavaScript可以做什么？  
 >+操作DOM（对DOM元素进行增删、查改、注册事件）  
 >+操作BOM（页面跳转、历史纪录、location，console.log(),alert(),）  
 >+AJAX/跨域  
 >+EcmaScript  
 >+处理表单，读取浏览器的**cookie**

 四，浏览器中的JavaScript不能做什么？  
 >+操作文件（文件和文件夹的增删查改）  
 >+不能操作系统信息  
 >+不能关闭不是自己打开的浏览器窗口  
 >+原因：运行环境特殊（JavaScript的运行环境是在我们所不熟悉的人的浏览器端运行的）

五，在开发人员能力相同的情况下，编程语言的能力取决于什么？  
>+语言本身？  
>+语言本身只提供了语言所必须的变量定义、流程控制、运算符、函数定义、面向对象之类的内容
     
>+编程语言的能力取决于语言的   *平台（环境）*
    
      
>+针对于JS来讲，常说的只是ES，运行在浏览器上很多功能都是浏览器的执行引擎提供的  
>+BOM和DOM可以说是浏览器提供的一些接口
    
>+Java既是一种语言也是一种平台，  Java是运行在Java虚拟机上的（跨操作系统）  
>+PHP既是一种语言也是一种平台，使用php-fpm解析，具体可看：https://segmentfault.com/q/1010000007110088?_ea=1240894
>+C#语言的平台：.NET Framework(Windows)  
>+C#语言也可以运行在MONO平台上  
>+因为有需求将C#运行在Linux上，才有MONO的出现
    
六，JavaScript只能运行在浏览器上吗？  
>+很显然不是的  
>+javascript的运行取决于执行环境，而且环境没有特定的平台。  

## 什么是Node  
>+Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.   
Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.  
    
#### Node 就是 JavaScript 语言在服务器端的运行环境
#### 所谓“运行环境（平台）”有两层意思：  
首先，JavaScript 语言通过 Node 在服务器运行，在这个意义上，Node 有点像 JavaScript 虚拟机；  
其次，Node 提供大量工具库，使得 JavaScript 语言与操作系统互动（比如读写文件、新建子进程），在这个意义上， Node 又是 JavaScript 的工具库。  
#### 重点理解
    Node是一个JavaScript的运行环境（平台），不是一门语言，也不是JavaScript的框架；  
    Node的实现结构；
    Node可以用来开发服务端应用程序，Web系统；
    基于Node的前端工具集


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

# nodeJS的NVM环境配置以及常用的Windows命令  

一，Windows下命令行工具Powershell和Cmd的区别？  
>+CMD占用系统内存小，Powershell占用内存多  
>+CMD只支持传统的windows命令，不支持.net库，也不支持Linux命令，Power shell支持  
>+简言之，Power shell是CMD的超集，具体见：https://www.jianshu.com/p/931ae4c34120  

二，Windows下卸载nodejs  
>+从控制面板卸载node，然后把对应的安装目录删除  

三，从nvm来实现node的版本控制，nvm安装，配置环境变量  
## 安装包的方式安装  
安装包下载链接：  
    Mac OSX： darwin  
    Windows：  
        x64  
        x86  
    安装操作：  
        一路  
        Next...  
        更新版本  
    操作方式：  
        重新下载最新的安装包；  
        覆盖安装即可；  
### 问题：  
以前版本安装的很多全局的工具包需要重新安装  
无法回滚到之前的版本  
无法在多个版本之间切换（很多时候我们要使用特定版本）  
### NVM工具的使用  
Node Version Manager（Node版本管理工具）  
由于以后的开发工作可能会在多个Node版本中测试，而且Node的版本也比较多，所以需要这么款工具来管理  
## 安装操作步骤  
    下载：nvm-windows  
    解压到一个全英文路径  
    编辑解压目录下的settings.txt文件（不存在则新建）  
    root 配置为当前 nvm.exe 所在目录  
    path 配置为 node 快捷方式所在的目录  
    arch 配置为当前操作系统的位数（32/64）  
    proxy 不用配置  
    配置环境变量 可以通过 window+r : sysdm.cpl  
    NVM_HOME = 当前 nvm.exe 所在目录  
    NVM_SYMLINK = node 快捷方式所在的目录  
    PATH += %NVM_HOME%;%NVM_SYMLINK%;  
    打开CMD通过set [name]命令查看环境变量是否配置成功  
    PowerShell中是通过dir env:[name]命令  
    NVM使用说明：  
    https://github.com/coreybutler/nvm-windows/  
    NPM的目录之后使用再配置
## 配置Python环境  
Node中有些第三方的包是以C/C++源码的方式发布的，需要安装后编译 确保全局环境中可以使用python命令  

## 环境变量的概念  
>环境变量就是操作系统提供的系统级别用于存储变量的地方  
    
    Windows中环境变量分为系统变量和用户变量 
    
    环境变量的变量名是不区分大小写的
    
    特殊值：
        PATH 变量：只要添加到 PATH 变量中的路径，都可以在任何目录下搜索
## Windows下常用的命令行操作
    切换当前目录（change directory）：cd
    创建目录（make directory）：mkdir
    查看当前目录列表（directory）：dir
        别名：ls（list）
    清空当前控制台：cls
        别名：clear
    删除文件：del
        别名：rm
    注意：所有别名必须在新版本的 PowerShell 中使用


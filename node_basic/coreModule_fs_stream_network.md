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










### node中的模块化机制
> node的模块化采用的是CommonJs规范，模块与文件是一一对应的关系，即加载一个模块，实际上对应的就是加载一个模块文件  

#### CommonJs规范额特点
* 所有代码运行在当前模块作用域中，不会污染全局作用域  
* 模块同步加载，根据代码中出现的顺序依次加载  
* 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。  

  在node中模块的分类主要有三种  
  1.  文件模块：所谓文件模块就是以文件命名的方式实现的功能模块文件（一般都是自定义的）  
  2.  核心模块：核心模块指代的就是Node平台自带的基本功能模块，称API  
  3.  第三方模块：第三方开发人员开源的功能模块，npm  
  
### CMD和AMD以及CommonJs定义模块的方式  
1.  CMD  的可以接收三个参数，第三个参数可为函数，对象或字符串  
* require 是一个方法，接受 模块标识 作为唯一参数，用来获取其他模块提供的接口。  
* exports 是一个对象，用来向外提供模块接口。  
* module 是一个对象，上面存储了与当前模块相关联的一些属性和方法  

` define('moduleName','dependModule'，function(require,exports,module){`  

   `  var x = require('./x');`  
 
   ` // 无法立刻得到模块 x 的属性 a`  
  
   `   console.log(x.a); // undefined` 
   
`})`  

2. AMD规范定义方式，
`define("alpha", ["require", "exports", "beta"], function (require, exports, beta) {`  

       `exports.verb=function() {` 
       
        `   return beta.verb();` 
        
       `    //Or:returnrequire("beta").verb();` 
       
        `}` 
  `});`  
  
  3. CommonJs的规范定义方式  
  module.js  
  
  `function  add(a,b){`  
   `return a + b;`  
  `}`  
  
  module.exports = {add};
  
---------------------------------------  

### 模块内的全局环境（伪，该全局环境的意思指代仅在模块内部空间为全局变量）  
  
1. __dirname：用于获取当前文件所在目录的完成路径（文件上一层），在REPL环境无效；
2. __filename:用于获取当前文件完整的路径，在REPL环境无效；    
  * 在Node中文件查找是以入口文件为基准查找文件，要求所有文件操作都必须为*绝对路径*  
3. module:模块对象  
4. exports: 映射到module.exports的别名，一般导出模块exports = module.exports = {}  
5. require:用于引入一个模块

#### module对象  
> module对象是由Node提供的一个module构建函数，所有的模块都是module的实例；每个模块都会有一个module对象，该对象就指向当前模块  
> （1）id：当前模块的bi  
（2）exports：表示当前模块暴露给外部的值  
（3）parent： 是一个对象，表示调用当前模块的模块  
（4）children：是一个对象，表示当前模块调用的模块  
（5）filename：模块的绝对路径  
（6）paths：从当前文件目录开始查找node_modules目录；然后依次进入父目录，查找父目录下的node_modules目录；依次迭代，直到根目录下的node_modules  
（7）loaded：一个布尔值，表示当前模块是否已经被完全加载  
 (8)exports:表示模块对外输出的值  
 
 #### 模块定义的原则
 * 一个新的JS文件就是一个模块  
 * 一个有用的模块应该是有输入输出的，否则模块就失去了定义的价值  
 * 模块内部是一个独立封闭的作用域（避免冲突）
 * 模块之间通过导入导出的方式协同  
 * 导出的方式：  
 > - exports.name = value;  
 > - module.exports = { name : value };
 
 
 
### require加载模块  
> require的基本功能是读入并执行一个Javascript文件，然后返回该模块的exports对象，若未发现制定模块，报错  

##### 如何实现简单的require步骤
* 查找文件路径  
* 读取文件内容（该过程需要同步进行，异步则文件内容在回掉中无法取到）  
* 执行文件内不得JS代码（文件内部的代码需要构造一个私有空间，防止污染全局；实际上就是需要构造一个数据容器，用来装模块导出的成员）  
* 返回值（exports）

##### require加载模块时的规则
1. 加载文件
> --require加载文件是按照一定的优先级加载：JS文件----->json文件（json文件直接按照json格式写即可，一般json文件都是项目配置相关）------>.node文件----->省略扩展名（加载目录下的文件）

2. 加载核心模块（对于不加'./'和'../'的则默认加载一个node核心模块或者从当前目录向上搜索node_modules目录中的文件）
> - require('fs');   =>加载核心模块中的文件系统模块  
> - require('my_modules');  =>各级node_modules文件中就近搜索my_nodules.js文件

### require加载模块的缓存
* 第一次加载某个模块时，Node会缓存该模块；以后在加载该模块，就直接从还缓存中取出该模块的module.exports属性（不会再次执行该模块）
* 如果需要多次执行模块中的代码，一般可以让模块暴露函数，每次加载都会重新执行函数来实现多次执行  
* 模块的缓存可以通过require.cache属性得到，同样也可以删除

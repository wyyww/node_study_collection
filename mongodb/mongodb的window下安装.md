### mongodb使用
####  window下载安装  

<p><strong><span style="font-size:18px;">一、先登录Mongodb官网<a href="https://www.mongodb.com/download-center#community" target="_blank">https://www.mongodb.com/download-center#community</a> 下载 &nbsp;&nbsp;安装包。32、64位的都行。</span></strong><br></p>
<p><img src="http://img.blog.csdn.net/20170901194620456?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" width="761" height="364"></p>
<p><strong><span style="font-size:18px;">二、安装MongoDB</span></strong></p>
<p>下载后的安装包：<img src="http://img.blog.csdn.net/20170901195010442?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p>安装比较简单，类似于普通QQ软件，中间主要是选择“Custom”自定义 安装路径修改下：C:\mongodb</p>
<p>然后不断“下一步”，安装至结束。</p>
<p><br></p>
<p>安装比较容易。难点在启动Mongodb的服务以及将MongoDB设置成Windows服务，加配置文件在windows的“服务”中找到。<br></p>
<p><span style="font-size:18px;">&nbsp;<br></span></p>
<p><strong><span style="font-size:18px;">三、先创建数据库文件的存放位置</span></strong></p>
<p>在MongoDB下创建data，在data下再创建db：C:\mongodb\data\db</p>
<p>因为启动mongodb服务之前需要必须创建数据库文件的存放文件夹，否则命令不会自动创建，而且不能启动成功。</p>
<p><br></p>
<p><strong><span style="font-size:18px;">四、启动MongoDB服务</span></strong></p>
<p><strong>1.打开cmd命令行<br></strong></p>
<div class="content-list-text">
<p><strong>2.进入C:\mongodb\bin目录（注意：先输入c:进入c盘，然后输入cd C:\mongodb\bin ）</strong></p>
<p><strong>3.输入如下的命令启动mongodb服务：mongod --dbpath C:\mongodb\data\db </strong></p>
<p><img src="http://img.blog.csdn.net/20170901201220331?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br></p>
</div>
即是在第三步创建的数据库存放文件路径下启动。<br><p></p>
<p><strong>4.在浏览器输入http://localhost:27017 （27017是mongodb的端口号）查看，若显示：</strong></p>
<p><img src="http://img.blog.csdn.net/20170901201613178?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br></p>
<p>则表示，连接成功。如果不成功，可以查看端口是否被占用。</p>


###  配置本地window系统服务 或者 添加mongodb系统变量都可以
<p>但是在本地windows“服务”中，是没有配置上mongodb&nbsp;服务的，可以打开“服务”看下<br></p>
<p><img src="http://img.blog.csdn.net/20170901202030579?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p><br></p>
<p><span style="font-size:18px;">五、配置本地windows mongodb&nbsp;服务</span></p>
<p>这样可设置为 开机自启动，可直接手动启动关闭，可通过命令行net start MongoDB&nbsp;启动。该配置会大大方便。</p>
<p><strong>1.先在data文件下创建一个新文件夹log（用来存放日志文件）</strong><br></p>
<p><img src="http://img.blog.csdn.net/20170901202603073?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br></p>
<p><strong>2.在Mongodb新建配置文件mongo.config</strong></p>
<p><img src="http://img.blog.csdn.net/20170901202813960?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p><br>
可能很多人都不会创建.config配置文件。那给大家介绍下简单的方法：</p>
<p><span style="color:#FF0000;">先创建一个mongo.txt文件，再打开，点击”另存为“，将底下的文件类型更改为”全部类型“，并更改文件名称为mongo.config。</span></p>
<p>这样就可以创建一个config的配置文件了。</p>
<p><br></p>
<p><strong>2.用记事本打开<span style="color:#FF0000;">mongo.config</span>&nbsp; ，并输入：</strong></p>
<p></p>
<p>dbpath=D:\software\MongoDB\data\db</p>
logpath=D:\software\MongoDB\data\log\mongo.log
<p><br></p>
<p><strong>3.用管理员身份打开cmd:</strong></p>
<p>可能还有很多人不会管理员身份打开cmd。这也介绍下：</p>
<p>在下图路径下找到cmd&nbsp;的运行文件</p>
<p><img src="http://img.blog.csdn.net/20170901203802105?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p>然后右键，以管理员身份运行。打开后发现在顶端比普通打开的多了”管理员“三个字</p>
<p><img src="http://img.blog.csdn.net/20170901203946672?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p><br></p>
<p><strong>4.配置windows服务：</strong></p>
<p>cmd先跳转到 D:\software\MongoDB\bin目录下。</p>
<p>输入：mongod --config D:\software\Mongodb\mongo.config --install --serviceName "MongoDB"</p>
<p>即根据刚创建的mongo.config配置文件安装服务，名称为MongoDB。</p>
<p><img src="http://img.blog.csdn.net/20170901204630250?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p>完成后，再次查看本地的服务。</p>
<p><img src="http://img.blog.csdn.net/20170901205003614?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGVzaHVzaHVu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p>如果成功的话，会发现本地服务多了”MongoDB"服务。</p>
<p>这就大功告成了。哈哈~~~<br></p>
<p>可以通过：“开机自启动，可直接手动启动关闭，命令行net start MongoDB&nbsp;启动”。</p>
<p>开启后，可以正常连接了。可以用pycharm等IDE连接，</p>
<p><br></p>

### 添加mongodb的系统变量：
 在window系统高级设置中，给mongodb添加系统变量：C:\mongodb\bin即可
 
 ## 添加mongodb系统变量和添加本地window服务的区别
 1. 添加系统变量：  
 添加系统变量之后，可以直接通过：```C:\Users\jmepj>mongod --dbpath C:\mongodb\data\db```启动mongodb数据库（注意这是直接从C盘启动）;  
 也可以通过： ```C:\mongodb\bin> mongod --dbpath C:\mongodb\data\db```启动数据库    
 同时使用```C:\mongodb\bin> mongo``` 或者```C:\Users\jmepj>mongo```都可以进行数据库连接  
 
 2. 添加MongoDB本地服务  
 通过```win+x```可以使用cmd管理员身份进行本地设置，命令行中输入```net start MongoDB```即可开启MongoDB本地服务  
 开启MongoDB服务之后```mongodb.lock```将会被MongoDB占用从而被锁住；通过```C:\Users\jmepj>mongod --dbpath C:\mongodb\data\db```  
 这样我们的mongodb数据库就会实现开机自启动了，而不需要我们自己启动了；通过```net stop MongoDB```关闭服务
 
<p>pycharm连接mongodb，可以使用<span style="font-size:12px;"><strong>pycharm配置mongo插件连接</strong>。</span></p>
<p>介绍个博文你学习安装：<br></p>
<p><a href="http://blog.csdn.net/heshushun/article/details/77777752" target="_blank">http://blog.csdn.net/heshushun/article/details/77777752</a><br></p>
            </div>
                 




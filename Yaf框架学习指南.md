说起PHP框架，很多人的印象都停留在一个由PHP实现的基于MVC的各种功能组合的代码包。极少有人知道C语言也能写PHP框架，并且速度比PHP写的框架快上10倍以上。

Yaf是一款以C语言写的PHP框架。它以PHP扩展的方式运行框架。只实现了MVC最核心部分的功能：路由、MVC。

Yaf框架的最大用户当属新浪和百度。大家经常上的微博每一个页面基本上都是经过Yaf框架的手到达你的眼前。

我们用过很多PHP扩展。比如非常热门且实用的memcache扩展、PDO扩展等。所以，对于像Yaf这样的扩展也不会显得特别陌生。


Yaf可以说是国内非常著名PHP的C语言扩展。对么国外有没有这种同类似的扩展呢？答案是：有！它的名字叫：phalcon。它有一个被程序员戏谑的称呼：尔康。

那么，关于Yaf与Phalcon谁更优秀呢？我们现在就来一起对比一下吧。

1）Yaf的优点：
1. 用C语言开发的PHP框架, 相比原生的PHP, 几乎不会带来额外的性能开销.
2. 所有的框架类, 不需要编译, 在PHP启动的时候加载, 并常驻内存.
3. 更短的内存周转周期, 提高内存利用率, 降低内存占用率.
4. 灵巧的自动加载. 支持全局和局部两种加载规则, 方便类库共享.
5. 高性能的视图引擎.
6. 高度灵活可扩展的框架, 支持自定义视图引擎, 支持插件, 支持自定义路由等等.
7. 内建多种路由, 可以兼容目前常见的各种路由协议.
8. 强大而又高度灵活的配置文件支持. 并支持缓存配置文件, 避免复杂的配置结构带来的性能损失.
9. 在框架本身,对危险的操作习惯做了禁止.
10. 更快的执行速度, 更少的内存占用.

> 除了以上框架本身的优点。Yaf还具备稳定、用户基数大等特点。

2）phalcon的优点：
1. 用户C语言开发的PHP框架。性能与Yaf接近，相差5%左右。
2. 内存使用较低。与Yaf相当。
3. 维护团队强大。Yaf目前只有惠新宸一个人在维护。而Phalcon有几十人在维护。
4. 全栈式。Phalcon实现了DB、Cookie、Session、安全、日志等WEB 2.0功能的封装。
5. 文档齐全。


这两款框架各有各的优势。

> Yaf内核够精简稳定，所以，几乎不会遇到运行上的问题。风险可控，性能优异。当然，因为简单，所以，你需要实现DB的封闭、Session的扩展等操作。

> Phalcon框架由于走的是用C语言实现全栈式的框架。所以，一旦某个模块不稳定或者有BUG。将给调度和维护带来极大的挑战。但是，由于它封装的功能非常完善，所以，我们基本上可以做到开包即用。不用再重复去制造轮子。

+ Yaf 框架中文文档：[http://www.laruence.com/manual/](http://www.laruence.com/manual/)
+ Phalcon框架中文文档：[http://docs.phalconphp.com/zh/](http://docs.phalconphp.com/zh/)


OK！咱们现在开始安装Yaf框架吧！

## 一、安装Yaf框架。

Yaf扩展主页:[http://pecl.php.net/package/yaf](http://pecl.php.net/package/yaf)

```shell
$ wget http://pecl.php.net/get/yaf-2.2.9.tgz
$ tar -zxvf yaf-2.2.9.tgz
$ cd yaf-2.2.9
$ phpize
$ make 
$ make install
```

编译生成扩展之后，修改php.ini。在php.ini文件末尾增加如下配置：
```shell
[yaf]
yaf.use_namespace = 1
yaf.environ = 'product'
yaf.cache_config = 0
yaf.name_suffix = 1
yaf.lowcase_path = 1
```

配置说明：
+ yaf.user_namespace 为1是开启命名空间模式。0关闭。
+ yaf.environ 是默认情况下Yaf读取的环境配置是什么。
+ yaf.cache_config 是否缓存项目配置。
+ yaf.name_suffix 开启后缀。即为1之后，类名将以XxxModel.php、XxxController.php模式加载。
+ yaf.lowcase_path 路径信息中的目录部分都会被转换成小写。


## 二、创建第一个Yaf项目

### 有两种方式创建Yaf项目。
1. 手工创建目录。
2. 使用Yaf提供的命令生成项目目录。

因为 Yaf 提供的命令工具没有随 Yaf 源码一起。所以，要单独下载。下载地址：[https://github.com/laruence/php-yaf](https://github.com/laruence/php-yaf)

在该项目下面有一个tools文件夹，里面就是命令行工具。当然，由于 Yaf项目结构本身非常之简单。所以，我也懒得去这样整。特麻烦。话说程序员都是懒鬼？


### 一个经典的Yaf项目目录结构如下：

```
+ public
  |- index.php //入口文件
  |- .htaccess //重写规则    
  |+ css
  |+ img
  |+ js
+ conf
  |- application.ini //配置文件   
+ application
  |+ controllers
     |- Index.php //默认控制器
  |+ views    
     |+ index   //控制器
        |- index.phtml //默认视图
  |+ modules //其他模块
  |+ library //本地类库
  |+ models  //model目录
  |+ plugins //插件目录
```

### 入口文件代码如下：
```php
<?php
define("APP_PATH",  realpath(dirname(__FILE__) . '/../')); /* 指向public的上一级 */
$app  = new Yaf_Application(APP_PATH . "/conf/application.ini");
$app->run();
```

###  Apache之.htaccess
```
#.htaccess, 当然也可以写在httpd.conf
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule .* index.php
```

### Nginx之Rewrite
```
server {
  listen ****;
  server_name  domain.com;
  root   document_root;
  index  index.php index.html index.htm;

  if (!-e $request_filename) {
    rewrite ^/(.*)  /index.php/$1 last;
  }
}
```

### 配置文件

在Yaf中, 配置文件支持继承, 支持分节. 并对PHP的常量进行支持. 你不用担心配置文件太大造成解析性能问题, 因为Yaf会在第一个运行的时候载入配置文件, 把格式化后的内容保持在内存中. 直到配置文件有了修改, 才会再次载入.

> 一个简单的配置文件application/conf/application.ini
```
[product]
;支持直接写PHP中的已定义常量
application.directory=APP_PATH "/application/" 
```


### 控制器

> 在Yaf中, 默认的模块/控制器/动作, 都是以Index命名的, 当然,这是可通过配置文件修改的.
对于默认模块, 控制器的目录是在application目录下的controllers目录下, Action的命名规则是"名字+Action"

默认控制器application/controllers/Index.php

```php
<?php
class IndexController extends Yaf_Controller_Abstract {
   public function indexAction() {//默认Action
       $this->getView()->assign("content", "Hello World");
   }
}
?>
```

### 视图文件

> Yaf支持简单的视图引擎, 并且支持用户自定义自己的视图引擎, 比如Smarty.
对于默认模块, 视图文件的路径是在application目录下的views目录中以小写的action名的目录中.

一个默认Action的视图application/views/index/index.phtml

```
<html>
 <head>
   <title>Hello World</title>
 </head>
 <body>
  <?php echo $content;?>
 </body>
</html>
```

### 运行

在浏览器输入
```
http://www.yourhostname.com/application/index.php
```

不出意外应该可以看到：Heoole World。

## 三、其他

看文档吧。文档还是蛮完整的。[http://www.laruence.com/manual/](http://www.laruence.com/manual/)
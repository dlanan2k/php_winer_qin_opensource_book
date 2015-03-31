
Swoole 到底是什么？

> PHP语言的高性能网络通信框架，提供了PHP语言的异步多线程服务器，异步TCP/UDP网络客户端，异步MySQL，数据库连接池，AsyncTask，消息队列，毫秒定时器，异步文件读写，异步DNS查询。
Swoole可以广泛应用于互联网、移动通信、企业软件、云计算、网络游戏、物联网、车联网、智能家居等领域。 使用PHP+Swoole作为网络通信框架，可以使企业IT研发团队的效率大大提升，更加专注于开发创新产品。

以上的定义是 Swoole 作者给出的。

Swoole 是以 PHP 扩展的形式工作的。就像诸如 mysql、memcache等扩展一样。安装了此扩展，你就能使用该扩展提供的所有功能。那么 Swoole 到底可以用来干啥呢？

### Swoole功能特性：
+ 实现 Socket 通信机制的服务器层。用它封装之后可以用于 HTTP WEB开发、游戏开发等工作。
+ 实现 Socket 通信机制的客户端层。用它可以模拟 socket请求，以及模板HTTP客户端请求。
+ 实现了一个比PHP进程管理更先进易用的模块。
+ 异步IO。就是指：类crontab定时器、事件循环、异步文件读取。
+ 内存操作。内存锁、数据共享、数据缓存。实现高并发。
+ HTTPS Server。通过对 socket 封装实现 http协议的server。无须web server即可以运行。性能出众。
+ Web Socket。


## 一、环境依赖

+ 仅支持Linux，FreeBSD，MacOS，3类操作系统
+ Linux内核版本2.3.32以上
+ PHP5.3.10以上版本
+ gcc4.4以上版本或者clang
+ cmake2.4+，编译为libswoole.so作为C/C++库时需要使用cmake

## 二、安装 Swoole 扩展

swoole扩展安装非常简单。最常见的方式是通过到 [http://pecl.php.net/](http://pecl.php.net/) 网站下载。但是，由于swoole社区非常活跃，导致版本迭代非常之快。所以，推荐去swoole官方的github上下载。下载地址：[https://github.com/swoole/swoole-src](https://github.com/swoole/swoole-src)

> swoole有很多个版本，推荐下载 tags 中版本号最大的那个且是stable的版本。

```shell
$ wget https://github.com/swoole/swoole-src/archive/swoole-1.7.14-stable.zip
$ unzip swoole-1.7.14-stable.zip
$ phpize
$ make
$ make install
```

修改php.ini。在文件末尾增加以下配置
```
[swoole]
extension=swoole.so
```

更多扩展编译参数说明文档：[http://wiki.swoole.com/wiki/page/6.html](http://wiki.swoole.com/wiki/page/6.html)

## 三、Linux内核调整

要想swoole稳定且高性能运行，需要对Linux内核进行调整。关于内核的调整文档，官方网站说得非常详细：[http://wiki.swoole.com/wiki/page/11.html](http://wiki.swoole.com/wiki/page/11.html)



## 四、基于swoole的周边项目

swoole是一个偏底层的网络框架。基于它可以开发出更多的高级应用框架。基于swoole的项目：[我去](http://wiki.swoole.com/wiki/page/303.html)


## 五、示例展示

swoole提供了非常完整的示例。需要配合其文档来进行学习理解。
+ 官方示例地址：[我去看看](https://github.com/swoole/swoole-src/tree/swoole-1.7.14-stable/examples)
+ 方官文档地址：[我也看看](http://wiki.swoole.com/)
+ 非常官方文档：[推荐看看](https://github.com/LinkedDestiny/swoole-doc)

> 注意：建议所有示例在Linux系统下运行。不推荐在windows或Mac系统下运行。


## 六、swoole 其它

+ IDE自动提示工具：[必须看看](https://github.com/eaglewu/swoole-ide-helper)
+ WEB IM :[必须看看](https://github.com/matyhtf/php-webim)
+ swoole成员提示的视频：[http://www.tudou.com/home/shenzhe/](http://www.tudou.com/home/shenzhe/)




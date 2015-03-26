
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


## 一、安装 Swoole 扩展

## 二、Swoole 配置

## 三、
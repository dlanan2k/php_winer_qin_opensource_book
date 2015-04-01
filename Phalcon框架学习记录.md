
Phalcon是一个用C扩展写的PHP WEB框架。具备高性能和低资源消耗的特点。

Phalcon 源码托管地址：[https://github.com/phalcon/cphalcon](https://github.com/phalcon/cphalcon)

中文文档地址：[http://docs.phalconphp.com/zh/latest/index.html](http://docs.phalconphp.com/zh/latest/index.html)

API文档地址：[http://docs.phalconphp.com/zh/latest/api/](http://docs.phalconphp.com/zh/latest/api/)

windows DLL : [http://phalconphp.com/en/download/windows](http://phalconphp.com/en/download/windows)

## 一、安装 Phalcon 框架

官方提供的安装文档地址：[http://phalconphp.com/en/download](http://phalconphp.com/en/download)

以下为Mac系统安装流程（其实安装都差不多）：

```shell
git clone git://github.com/phalcon/cphalcon.git
cd cphalcon/build
sudo ./install
```

在php.ini文件末尾中增加以下内容：
```
extension=phalcon.so
```

重启webserver服务器，通过phpinfo()可以看到phalcon扩展已经成功加载。


## 二、Phalcon 辅助工具

我们接下来使用 composer 工具安装辅助工具。请确保您已经在系统中安装了composer。

创建一个目录，在目录下创建一个composer.json文件：
```
{
    "require": {
        "phalcon/devtools": "dev-master"
    }
}
```

运行 composer 安装器：
```shell
composer install
```

给phalcon.php创建symbolic link:
```shell
ln -s ~/devtools/phalcon.php /usr/bin/phalcon
chmod ugo+x /usr/bin/phalcon
```

## 三、创建第一个Phalcon框架

在你想要创建项目的目录下，执行如下命令：
```shell
$ phalcon create-project first_phalcon
```

将会生成如下目录结构：
```
├── app
│   ├── cache
│   ├── config
│   │   ├── config.php
│   │   ├── loader.php
│   │   └── services.php
│   ├── controllers
│   │   ├── ControllerBase.php
│   │   └── IndexController.php
│   ├── models
│   └── views
│       ├── index
│       │   └── index.volt
│       ├── index.volt
│       └── layouts
├── index.html
└── public
    ├── css
    ├── files
    ├── img
    ├── index.php
    ├── js
    └── temp
```

将public指向为允许访问的入口文件夹并访问即可。


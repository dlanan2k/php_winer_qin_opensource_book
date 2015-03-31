
根据 Zephir 的作者（出是phalcon的核心成员）的描述：Zephir是一门能使用语法与PHP相同且纯面向对象的编译型语言。它将代码编译为C语言，再生成PHP扩展。

### Zephir 有几个特性：
+ 类型可静可动。即支持PHP类似的动态类型、也可以支持类似C语言的静态类型。
+ 减少程序执行时间提升运行性能。毕竟是把代码编译为静态类型的C语言。
+ 采用面向对象OOP编译。
+ 内存安全。即不需要操心内存的管理问题。
+ 预编译器(AOT)提供可预测的性能。

Zephir是开放源代码的项目。代码托管在github：[https://github.com/phalcon/zephir](https://github.com/phalcon/zephir)


### Zephir 依赖以下库：

+ json-c：C语言JSON库。github地址：[https://github.com/phalcon/json-c](https://github.com/phalcon/json-c)
+ re2c : PHP语法解析器。官网地址：[http://re2c.org/](http://re2c.org/)

### 环境要求：
+ g++ >= 4.4/clang++ >= 3.x/vc++ 9
+ gnu make 3.81 or later
+ php development headers and tools

### 一、安装

#### 1）安装依赖re2c：

因为，我的是Mac系统，所以。我使用的是如下命令安装：
```shell
brew install re2c
```

如果你的是centos，那么应该使用如下命令安装：
```shell
yum install re2c
```

#### 2）安装Zephir

创建一个文件夹 zephir_test(名字随意)，然后，我们使用composer安装：
```shell
$ composer require phalcon/zephir
$ cd ./zephir_test/vendor/phalcon/zephir
$ git clone https://github.com/json-c/json-c.git
$ ./install-json
$ ./install -c
$ ./zephir_test/vendor/phalcon/zephir/bin/zephir compile
```

通过以上步骤就算安装好了。测试安装是否成功：
```
$ cd ./zephir_test/vendor/phalcon/zephir/bin
$ ./zephir
```

如果没有报错，说明就安装成功了。


#### 3）将 zephir 命令添加到 PATH中

```shell
$ vim /etc/profile
```
在文件末尾增加如下代码：
```shell
export PATH=$PATH:/Users/qlj/codespace/phpcode/zephir_test/vendor/phalcon/zephir/bin
```
激活使其生效：
```shell
$ source /etc/profile
```

### 二、创建第一个Zephir扩展

通过 Zephir 命令创建：
```shell
$ zephir init wlib
$ cd wlib/wlib
$ vim greeting.zep
```

增加如下代码：
```php
namespace Wlib;

class Greeting
{
    public static function say()
    {
        echo "hello world!";
    }
}
```

greeting.zep路径为wlib/wlib/greeting.zep。

则需要在greeting.zep上一级目录下执行如下命令编译：
```shell
$ zephir build
```

输入如下内容则编译成功：
```
Compiling...
Installing...
Password:
Extension installed!
Don't forget to restart your web server
```

这个时候，就已经生成了so文件。则只需要在php.ini末尾增加如下内容：
```
[wlib]
extension=wlib.so
```

测试代码：
```shell
$ vim test.php
```

增加如下代码：
```php
<?php
echo Wlib\Greeting::say(), "\n";
```

执行输出如下内容：
```shell
$ php test.php
```

输出：
```
hello world!
```



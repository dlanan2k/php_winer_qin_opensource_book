
Composer 是 PHP 用来管理依赖（dependency）关系的工具。你可以在自己的项目中声明所依赖的外部工具库（libraries），Composer 会帮你安装这些依赖的库文件。

PHP 开发过程中，难免会使用第三方包或自己的功能吧。那么，很容易出来第三方包之间的包名一样的情况。以及，同一包的不版本与其他包之间的依赖混乱的情况。

其它的语言也有类似的包依赖管理工具：Java有Maven，Python有pip，Ruby有gem，Nodejs有npm。

PHP的则是PEAR，不过PEAR坑不少：

+ 依赖处理容易出问题
+ 配置非常复杂
+ 难用的命令行接口

既然有问题，那一定会有人跳出来解决。于是，Composer就出来了。

> 运行 Composer 需要 PHP 5.3.2+ 以上版本。一些敏感的 PHP 设置和编译标志也是必须的，但对于任何不兼容项安装程序都会抛出警告。

## 一、Composer在各平台的安装

事实上只要你使用的系统中已经开启或支持Curl命令或将php可执行文件添加到了系统PATH中。那么，安装composer是相当的简单的。


在你的客户端运行如下命令安装 composer。默认安装最新版。

```shell
curl -sS https://getcomposer.org/installer | php
```

如果你的系统没有curl,则：
```shell
php -r "readfile('https://getcomposer.org/installer');" | php
```

官方英文安装文档：[https://getcomposer.org/download/](https://getcomposer.org/download/)

## 二、一些常用的 Composer 命令

假定你已经有一个新项目对应的文件夹名称是 test 。那么，在 test 项目下你要创建一个composer.json。这样，在之后使用composer.json命令才能成功。

### 1）安装扩展包

以安装 monolog 扩展为例。这个包可以在 packagist.org 搜索得到。可以看到每个包的引入条件。

在composer.json里面增加引入配置，增加之后composer.json文件内容如下：
```
{
    "require": {
        "monolog/monolog": "1.2.*"
    }
}
```

这个时候，我们可以在命令行的 test 项目目录下执行如下命令安装它：
```shell
composer install
```

这个时候在项目根目录下会生成一个 composer.lock 文件，以及一个 vendor 文件夹。在vendor文件夹下有一个 monolog 文件夹，说明我们已经安装成功。


### 2）更新扩展包

更新扩展包，可以一起更新，也可以单独更新某一个包。

```shell
composer update
```

以上命令会将当前扩展包整体更新。

```shell
composer update monolog/monolog
```

以上命令只会更新monolog/monolog扩展包。

当修改了 composer.json 文件且已经存在composer.lock文件的时候，使用如下命令并不会更新：
```shell
composer install
```
必须使用：
```shell
composer update
```

### 3）移除扩展包

扩展包移动命令会将扩展包在vendor文件夹中彻底删除。命令如下：
```shell
composer remove monolog/monolog
```

### 4）包版本号介绍

在前面的例子中，我们引入的 monolog 版本指定为 1.0.*。这表示任何从 1.0 开始的开发分支，它将会匹配 1.0.0、1.0.2 或者 1.0.20。

版本约束可以用几个不同的方法来指定。

| 名称 |	实例	| 描述 |
| --- | --- | --- |
| 确切的版本号|	1.0.2 | 	你可以指定包的确切版本。|
| 范围 |	>=1.0 >=1.0,<2.0 >=1.0 | 通过使用比较操作符可以指定有效的版本范围。  |
| 有效的运算符：| >、>=、<、<=、!=。 | 你可以定义多个范围，用逗号隔开，这将被视为一个逻辑AND处理。 AND 的优先级高于 OR。|
| 通配符 |	1.0.* | 	你可以使用通配符*来指定一种模式。1.0.*与>=1.0,<1.1是等效的。|
| 赋值运算符 | ~1.2 |	这对于遵循语义化版本号的项目非常有用。~1.2相当于>=1.2,<2.0。想要了解更多，请阅读下一小节。|

### 下一个重要版本（波浪号运算符）

~ 最好用例子来解释： ~1.2 相当于 >=1.2,<2.0，而 ~1.2.3 相当于 >=1.2.3,<1.3。正如你所看到的这对于遵循 语义化版本号 的项目最有用。一个常见的用法是标记你所依赖的最低版本，像 ~1.2 （允许1.2以上的任何版本，但不包括2.0）。由于理论上直到2.0应该都没有向后兼容性问题，所以效果很好。你还会看到它的另一种用法，使用 ~ 指定最低版本，但允许版本号的最后一位数字上升。

> 注意： 虽然 2.0-beta.1 严格地说是早于 2.0，但是，根据版本约束条件， 例如 ~1.2 却不会安装这个版本。就像前面所讲的 ~1.2 只意味着 .2 部分可以改变，但是 1. 部分是固定的。

### 5）稳定性

默认情况下只有稳定的发行版才会被考虑在内。如果你也想获得 RC、beta、alpha 或 dev 版本，你可以使用 稳定标志。你可以对所有的包做 最小稳定性 设置，而不是每个依赖逐一设置。

稳定性对应 composer.json文件中的 "minimum-stability": "dev"。

### 6）composer.lock - 锁文件

在安装依赖后，Composer 将把安装时确切的版本号列表写入 composer.lock 文件。这将锁定改项目的特定版本。

请提交你应用程序的 composer.lock （包括 composer.json）到你的版本库中。

这是非常重要的，因为 install 命令将会检查锁文件是否存在，如果存在，它将下载指定的版本（忽略 composer.json 文件中的定义）。

这意味着，任何人建立项目都将下载与指定版本完全相同的依赖。你的持续集成服务器、生产环境、你团队中的其他开发人员、每件事、每个人都使用相同的依赖，从而减轻潜在的错误对部署的影响。即使你独自开发项目，在六个月内重新安装项目时，你也可以放心的继续工作，即使从那时起你的依赖已经发布了许多新的版本。

如果不存在 composer.lock 文件，Composer 将读取 composer.json 并创建锁文件。

这意味着如果你的依赖更新了新的版本，你将不会获得任何更新。此时要更新你的依赖版本请使用 update 命令。这将获取最新匹配的版本（根据你的 composer.json 文件）并将新版本更新进锁文件。

```shell
php composer.phar update
```
如果只想安装或更新一个依赖，你可以白名单它们：

```shell
php composer.phar update monolog/monolog [...]
```
注意： 对于库，并不一定建议提交锁文件 请参考：库的锁文件.

更新文档，请参考：[http://www.phpcomposer.com/](http://www.phpcomposer.com/)
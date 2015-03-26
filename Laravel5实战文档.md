
Laravel 从2.0开始就被不少程序员所熟知并流行起来。目前最新的版本是5.0。Laravel 3.x 与之后的 4.x 和 5.x有着非常大的变化。通过官方文档或中文社区的文档可以看得出来。

中文文档地址：[点击查看](http://v3.golaravel.com/docs/)

因为 Laravel 4.x出来的时间不是特别长，以及 Laravel 5.x 比 4.x 在更优秀。所以，我们研究与使用就以 5.x 为基准。


## 一、安装 Composer

Composer 是 PHP 用来管理依赖（dependency）关系的工具。你可以在自己的项目中声明所依赖的外部工具库（libraries），Composer 会帮你安装这些依赖的库文件。

关于 Composer 的中文文档地址：[http://www.phpcomposer.com/](http://www.phpcomposer.com/)

### Linux & Mac 安装方式

```shell
$ sudo -s
$ curl -sS https://getcomposer.org/installer | php
$ mv composer.phar /usr/local/bin/composer
```

> 如果您的 Composer 版本过低，则会有警告提示。通过如此命令升级您的 Composer

```shell
sudo /usr/local/bin/composer self-update
```

> 关于 Composer 的命令操作以及包加载的优化，请查看上面的地址。


## 二、PHP 环境需求

+ PHP 版本 >= 5.4
+ Mcrypt PHP 扩展。此扩展是用来加密解密用的。支持多种加密方式。
+ OpenSSL PHP 扩展。为 https 提供驱动支持。当然，还有更多功能。
+ Mbstring PHP 扩展。处理各国语言字符的扩展。
+ Tokenizer PHP 扩展。PHP 源码分析工具。


## 三、Laravel 框架出击

### 1）使用 Composer 下载 Laravel 安装包：
```shell
composer global require "laravel/installer=~1.1"
```

> 以上命令在任何位置执行都可以。是全局有效。
> 提示：如果网络不好。请任性地使用VPN吧。没有？买呗！！！


命令正常执行，会有如下输出：

```
Changed current directory to /Users/qlj/.composer
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
  - Installing symfony/process (v2.6.5)
    Downloading: 100%

  - Installing symfony/console (v2.6.5)
    Downloading: 100%

  - Installing guzzlehttp/streams (2.1.0)
    Loading from cache

  - Installing guzzlehttp/guzzle (4.2.3)
    Loading from cache

  - Installing laravel/installer (v1.2.0)
    Downloading: 100%

symfony/console suggests installing symfony/event-dispatcher ()
symfony/console suggests installing psr/log (For using the console logger)
Writing lock file
Generating autoload files
```

> 请确定把 ~/.composer/vendor/bin 路径放置于您的 PATH 里， 这样 laravel 执行文件就会存在你的系统。

### 2）mac 下将 laravel 放到系统 PATH 变量中

```shell
vim ~/.bash_profile
```

在文件末尾追加以下代码：
```shell
export PATH=/Users/qlj/.composer/vendor/bin:$PATH
```

小贴士：mac和linux终端一般用bash来进行解析。当bash在读完了整体环境变量的/etc/profile并借此调用其他配置文件后，接下来则是会读取用户自定义的个人配置文件。bash读取的文件总共有三种：

~/.bash_profile 　　~/.bash_login  　　~/.profile

其实bash再启动是只读上面文件的一个，而读取的顺序则是依照上面的顺序。也就是说读到bash_profile就不读后面的了，如果bash_profile不存在，后面的才能有机会。

> 设置成功之后，请关闭当前会话的所有命令行窗口。因为，修改~/.bash_profile 这一类文件只能再下次启动命令窗口的时候才会加载。已经打开的窗口不能自动重新加载。这也是导致很多人设置成功之后，在命令输入 laravel 命令提示找不到命令的原因所在。

### 3）创建第一个 Laravel 项目

在上面的第2步中，我们已经将 ~/.composer/vendor/bin 放到了系统的 PATH 变量中。此时，我们在命令行中可以使用 laravel 命令来创建一个项目了。进入你项目所在的目录下：

```shell
cd /Users/qlj/codespace/phpcode
laravel new laravel
```
执行成功会输出如下信息：
```
Crafting application...
Generating optimized class loader
Compiling common classes
Application key [QivNTbKrvAGGv7nh1glCv5Ypw6sz1Z7h] set successfully.
Application ready! Build something amazing.
```

此时进入 laravel 目录下，可以看到已经为我们生成了一个完整的项目目录结构。关于目录结构我们后面再细细介绍。


### 4）让 Laravel 跑起来

+ 在 /etc/hosts 增加一个测试地址网站：test.laravel.com
+ 在 apache的http-vhosts.conf 同步增加一个虚拟服务器。代码如下：

```
<VirtualHost *:80>
    ServerAdmin 753814253@qq.com
    DocumentRoot "/Users/qlj/codespace/phpcode/laravel/public/"
    ServerName test.laravel.com
    ErrorLog "/private/var/log/apache2/test.laravel.com-error_log"
    CustomLog "/private/var/log/apache2/test.laravel.com-access_log" common
</VirtualHost>
```

在浏览器地址栏输入：[test.laravel.com](test.laravel.com)

如果看到了 Laravel 的 LOGO。说明，您已经安装成功了。


如果报以下错误：
```
Use of undefined constant MCRYPT_RIJNDAEL_128 - assumed 'MCRYPT_RIJNDAEL_128'
```

这是您没有安装 PHP mcrypt 扩展导致。w

Mac 系统下安装 mcrypt：

#### 4.1) mac 系统mcrypt库安装
```shell
brew install mcrypt
```
如果您的系统是 Centos 之类的系统，通过以下命令可以更新PHP或相关扩展所依赖系统扩展。

```
sudo -s
LANG=C
yum -y install gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers
```

#### 4.2）PHP mcrypt 扩展库安装

因为PHP5.3以上版本mcrypt核心中已经内置了mcrypt扩展。由于，不同版本的PHP默认开启与否不一样。因为，我的是Mac内置的PHP5.4.30没有开启。所以，有两种方法安装mcrypt扩展。

##### 4.2.1）源码完全安装
这种方式就是下载一个 php 源码码下来，然后完整编译php 源码安装。安装方式略。

##### 4.2.2）增量源码安装
这种方式下载一个与当前系统PHP版本一致的php源码包，然后，把源码里面的扩展包的mcrypt进行编译安装。这种方式花时间较少。不影响当前的PHP配置。

因为我不想改变现有环境的PHP。所以，我选择第二种增量安装方式。这种方式以前也没有试过。权当一次学习吧。

首先，我们去下载php源码。地址：[http://cn.php.net/releases/](http://cn.php.net/releases/)

我选择了中国的镜像下载点：地址[http://cn2.php.net/get/php-5.4.30.tar.gz/from/this/mirror](http://cn2.php.net/get/php-5.4.30.tar.gz/from/this/mirror)

下载成功之后，执行命令：
```shell
tar -zxvf mirror
cd /Users/qlj/Downloads/php-5.4.30/ext/mcrypt/
/usr/bin/phpize
./configure --with-mcrypt=/usr/local/homebrew/Cellar/mcrypt/2.6.8
make
make install
```

上面的./configure 后台是配置mcrypt的位置。因为 php 通过PHP的mcrypt扩展调用系统的mcrypt扩展来完成加密的。

修改php.ini文件。在文件末尾增加一行：
```
extension=mcrypt.so
```

#### 4.3 让项目的 storage 目录具备写入权限
```
chmod 0777 storage
```
Laravel 框架默认会将缓存、session、日志写入该目录。



### 5）Laravel5 目录结构介绍

* Laravel RootPath
* app
	* Commands
	* Console						-- 
	* Events						-- 所有应用事件定义在这里
	* Exceptions					-- 自定义异常
	* Handlers
	* Http							-- Controler在这里。因为Controller都是与Http请求相关。
	* Providers					-- 类似于四层架构的S层（服务层）
* bootstrap 						-- 启动框架时执行里面的程序
* config							-- 框架所有的配置全部在这里
* database						-- 数据库迁移等操作在这里
* public								-- 项目的入口文件
* resources						-- 图片、样式、JS等资源文件存放位置
* storage							-- 模板缓存、应用缓存、session、日志存放这里
* tests								-- phpunit单元测试程序放这里
* vendor							-- 第三方包以及 Laravel 核心包放这里

更多请看：[http://www.golaravel.com/laravel/docs/5.0/structure/](http://www.golaravel.com/laravel/docs/5.0/structure/)

### 6）GET && POST


### 7）数据库操作

7.1 配置
> 配置文件地址：app/config/database.php

Laravel 支持读写分离。配置非常简单。
```
'mysql' => [
    'read' => [
        'host' => '192.168.1.1',
    ],
    'write' => [
        'host' => '196.168.1.2'
    ],
    'driver'    => 'mysql',
    'database'  => 'database',
    'username'  => 'root',
    'password'  => '',
    'charset'   => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix'    => '',
],
```

> 目前 Laravel 支持四种数据库系统： MySQL、Postgres、SQLite、以及 SQL Server。

个人观点：Postgres 数据库在国外使用越来越广泛，并且性能以及管理优于MySQL。也是开源免费的关系型数据库。

7.2 执行 Select 查找
```php
$results = DB::select('select * from users where id = ?', [1]);
```
select 方法会返回一个 array 结果。

7.3 执行 Insert 语法
```php
DB::insert('insert into users (id, name) values (?, ?)', [1, 'Dayle']);
```

7.4 执行 Update 语法
```php
DB::update('update users set votes = 100 where name = ?', ['John']);
```

7.5 执行 Delete 语法
```php
DB::delete('delete from users');
```
> 注意： update 和 delete 语法会返回在操作中所影响的数据笔数。

7.6 事务操作
```php
DB::transaction(function()
{
    DB::table('users')->update(['votes' => 1]);
    DB::table('posts')->delete();
});
```

> 除了这种以原生语句操作数据库的方式外，还有一种就是采用对象方法的形式组装SQL语句并执行。

7.7 从数据表中取得所有的数据列
```php
$users = DB::table('users')->get();
foreach ($users as $user)
{
    var_dump($user->name);
}
```

7.8 从数据表中分块查找数据列
```php
DB::table('users')->chunk(100, function($users)
{
    foreach ($users as $user)
    {
        //
    }
});
```

7.9 从数据表中取得单一数据列的单一字段
```php
$name = DB::table('users')->where('name', 'John')->pluck('name');
```

7.10 取得单一字段值的列表
```php
$roles = DB::table('roles')->lists('title');
```

7.11 更多高级用法
```php
$users = DB::table('users')
                    ->where('votes', '>', 100)
                    ->orWhere('name', 'John')
                    ->get();

$users = DB::table('users')
                    ->whereNotBetween('votes', array(1, 100))->get();

$users = DB::table('users')
                    ->orderBy('name', 'desc')
                    ->groupBy('count')
                    ->having('count', '>', 100)
                    ->get();

DB::table('users')
        ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
        ->get();

DB::table('users')
            ->where('name', '=', 'John')
            ->orWhere(function($query)
            {
                $query->where('votes', '>', 100)
                      ->where('title', '<>', 'Admin');
            })
            ->get();
```

7.12 打印执行的SQL语句

这个功能可以说是非常实用且常用的功能了。

开启日志：
```sql
DB::connection()->enableQueryLog();
```

要得到执行过的查找纪录数组，你可以使用 getQueryLog 方法：
```sql
$queries = DB::getQueryLog();
```

---

7.13 Laravel 5 ActiverRecord 详解

> 以下是 Laravel 5 ActiveRecord 操作

由于 Model 这样的东西是属于多个系统（前台、后台、其他）。所以，一般情况下，我们都是将其放在一起。而不是与某个系统关联放一起。所以，我们将会用到 composer 来实现自动加载。

首先，在项目根目录下面创建一个 models 目录。当然，你也可以在其他位置创建这个目录。中需要在 composer里面能加载到即可。

其次，以创建 user 表的 model 为例。在 models 目录下创建一个 User.php 脚本。代码如下：
```php
<?php
namespace Models;
use Illuminate\Database\Eloquent\Model;

class User extends Model {
    protected $table      = 'users';
    protected $primaryKey = 'id';
    public $timestamps    = false;
}
```

> 注意。因为，我们的 users 类需要继承 Model。所以，需要：
```php
use Illuminate\Database\Eloquent\Model;
```

我们将 models 目录映射为 Models PHP命名空间。单单，使用 namespace Models的话，在项目中使用还不能正确加载。所以，我们需要 composer 工具来为我们正确让其他脚本能加载到。

修改项目根目录下的 composer.json 文件，找到 autoload 那块json。
```json
"autoload": {
		"classmap": [
			"database"
		],
		"psr-4": {
			"App\\": "app/",
			"Models\\" : "models/"
		}
	},
```

从上面可以看出。我已经将相对于项目根目录下的 models 映射为 Models 全名空间了。

现在，我们来使用 User 操作数据：
```php
use Models;
$users = Models\User::all();
var_dump($users);
```

更多关于Laravel ActiveRecord的操作参考中文文档：[http://www.golaravel.com/laravel/docs/5.0/eloquent/](http://www.golaravel.com/laravel/docs/5.0/eloquent/)


如果您曾经使用如下命令优化过 composer 包。那么，很有可能导致修改 composer.json没效果。
```shell
composer dump-autoload --optimize
```
那么，您需要修改之后，再执行此命令来清除之后的优化缓存。


### 8）缓存操作

Laravel 为各种不同的缓存系统提供一致的 API 。您可以在此为应用程序指定使用哪一种缓存系统， Laravel 支持各种常见的后端缓存系统，如 Memcached 和 Redis 。

> 配置文件：app/config/cache.php

8.1 保存对象到缓存中
```php
Cache::put('key', 'value', $minutes);
```

8.2 使用 Carbon 对象配置缓存过期时间
```php
$expiresAt = Carbon::now()->addMinutes(10);
Cache::put('key', 'value', $expiresAt);
```

8.3 若是对象不存在，则将其存入缓存中
```php
Cache::add('key', 'value', $minutes);
```
当对象确实被加入缓存时，使用 add 方法将会返回 true 否则会返回 false 。

8.4  确认对象是否存在
```php
if (Cache::has('key'))
{
    //
}
```

8.5 从缓存中取得对象
```php
$value = Cache::get('key');
```

8.6 取得对象或是返回默认值
```php
$value = Cache::get('key', 'default');
$value = Cache::get('key', function() { return 'default'; });
```

8.7 永久保存对象到缓存中
```php
Cache::forever('key', 'value');
```

8.8 有时候您会希望从缓存中取得对象，而当此对象不存在时会保存一个默认值，您可以使用 Cache::remember 方法：
```php
$value = Cache::remember('users', $minutes, function()
{
    return DB::table('users')->get();
});
```

8.9 从缓存中删除对象
```php
Cache::forget('key');
```

更多关于缓存的中文文档：[http://www.golaravel.com/laravel/docs/5.0/cache/](http://www.golaravel.com/laravel/docs/5.0/cache/)


### 9）Session操作

使用session之前，需要先对 Laravel 配置进行设置。默认的 session 保存在 storage 中。通过配置，我们可以保存到memcached、redis等缓存服务器中。

> 配置文件：app/config/session.php

9.1 读取 Session 值
```php
$value = Session::get('key');
// 如果没有取到值，则返回默认值。
$value = Session::get('key', 'default');
// 或
$value = session('key');
```

9.2 写入 Session 值
```php
Session::push('user.teams', 'developers');
```

9.3 从 Session 取回对象，并删除
```php
$value = Session::pull('key', 'default');
```

9.4 从 Session 取出所有对象
```php
$data = Session::all();
```

9.5 判断对象在 Session 中是否存在
```php
if (Session::has('users'))
{
    //
}
```

9.6 从 Session 中移除对象
```php
Session::forget('key');
```

9.7 清空所有 Session
```php
Session::flush();
```

9.8 重新产生 Session ID
```php
Session::regenerate();
```

更多关于 session 的中文文档：[http://www.golaravel.com/laravel/docs/5.0/session/](http://www.golaravel.com/laravel/docs/5.0/session/)


### 10）Cookie 操作

10.1 读取Cookie值
```php
$value = Request::cookie('name');
```

10.2 写入Cookie值
```php
$response = new Illuminate\Http\Response('Hello World');
$response->withCookie(cookie('name', 'value', $minutes));
```
稍微复杂点。如果使用use加载命令空间。代码会变成如下：
```php
$response = new Response('Hello World');
$response->withCookie(cookie('name', 'value', $minutes));
```

### 11）Laravel Commands 



### 12） Laravel artisan 命令

关于 artisan 命令的使用，我们在创建项目的时候，已经使用过一次了。有了这个命令，在开发中我们会省掉很多体力活。关于更多的命令使用，请看中文文档：[http://www.golaravel.com/laravel/docs/5.0/artisan/](http://www.golaravel.com/laravel/docs/5.0/artisan/)

### 13）分页

13.1 最简单的分页
```php
$users = DB::table('users')->paginate(15);
```

13.2 对 Eloquent 模型分页
```php
$allUsers = User::paginate(15);
$someUsers = User::where('votes', '>', 100)->paginate(15);
```

13.3 追回参数到URL末尾

在分页的时候，我们会搜索查询分页。默认情况下，是不会把URL中的参数附加上去的。于是，就有了以下的解决方案：
```php
$users = DB::table('users')->paginate(15)->appends(array('keyword' => '菠萝蜜'));
```

Laravel 分页是非常方便快捷的。更多的文档请参考中文文档：[http://www.golaravel.com/laravel/docs/5.0/pagination/](http://www.golaravel.com/laravel/docs/5.0/pagination/)


### 14）实用的辅助方法

14.1 产生给定控制器行为的网址。
```php
$url = action('HomeController@getIndex', $params);
```
注意：HomeController以及getIndex必须是存在的类和方法。否则，会报错。

14.2 产生给定路由名称的网址。
```php
$url = route('routeName', []);
```

14.3 产生资源的网址。
```php
$url = asset('img/photo.jpg');
```

14.4 产生给定路径的 HTTPS 完整网址。
```php
echo secure_url('foo/bar', $parameters = array());
```

14.5 产生给定路径的完整网址。
```php
echo url('foo/bar', $parameters = array(), $secure = null);
```

14.6 返回 取得现在 CSRF token 的值。
```php
$token = csrf_token();
```

14.7 打印给定变量并结束脚本执行。
```php
dd($value);
```

更多辅助方法参考中文文档：[http://www.golaravel.com/laravel/docs/5.0/helpers/](http://www.golaravel.com/laravel/docs/5.0/helpers/)


### 15）配置多个应用


### 16）配置ssdb存储session

#### 16.1 首先，我们要安装配置ssdb

1、下载
```shell
$ wget https://github.com/ideawu/ssdb/archive/master.zip
```

2、解压并安装
```shell
$ unzip master
$ cd ssdb-master
$ make 
$ make install
```

3、启动ssdb
```shell
$ ./ssdb-server ssdb.conf 
要在后台运行，执行命令：
$ ./ssdb-server -d ssdb.conf 
```

4、随服务器重启
```shell
$ vim /etc/rc.local
在文件末尾增加：
/usr/local/ssdb/ssdb-server -d /usr/local/ssdb/ssdb.conf
```

5、停止 ssdb-server
```shell
$ ./ssdb-server ssdb.conf -s stop
```

#### 16.2 实现 PHP SessionHandlerInterface 接口

说起 SessionHandlerInterface 接口大家可能都比较陌生。但是，他在如今的 PHP 框架中却充满了他的影子。比如我们常见的YII、ThinkPHP等框架都已经使用它很久了。他到底是什么东西呢？

SessionHandlerInterface 从名字上来解释就是 Session会话句柄接口。他的功能就是专门用来扩展 Session 会话的管理。通过扩展该接口，您可以将 session 会话的数据存储在任意您熟知的存储介质中。比如：mysql、memcache、redis等。

咱们来看看 SessionHandlerInterface 接口的定义：
```php
SessionHandlerInterface {
/* 方法 */
abstract public bool close ( void )
abstract public bool destroy ( string $session_id )
abstract public bool gc ( string $maxlifetime )
abstract public bool open ( string $save_path , string $name )
abstract public string read ( string $session_id )
abstract public bool write ( string $session_id , string $session_data )
}
```

1、open 方法通常用于将数据存储置于文件中的情况下，打开文件代码存储位置。因为，我们要将数据存放于内存或数据库这样的存储介质中。所以，一般来说，这个方法不需要实现。留空即可。

2、destroy 方法应该从永久存储移除与 $sessionId 关联的数据。

3、gc 方法应该销毁所有比给定 $lifetime UNIX 时间戳记还旧的 session 数据。对于会自己过期的系统如 Memcached 和 Redis，这个方法可以留空。

4、write 方法应该写入给定 $data 字串与 $sessionId 的关联到一些永久存储系统，例如：MongoDB、 Dynamo、等等。

5、read 方法应该返回与给定 $sessionId 关联的 session 数据的字串形态。当你的驱动取回或保存 session 数据时不需要做任何序列化或进行其他编码，因为 Laravel 将会为你进行序列化

6、close 方法，就像 open 方法，通常也可以忽略。对大部份的驱动来说，并不需要它。

知道了每个方法的功能。那么接下来，一起来实现将 session 数据保存到ssdb中。

以下是我实现的用于session数据存储到ssdb的类。已经通过真实项目检验。
```php
<?php
/**
 * Laravel 5.x 之 SSDB SESSION驱动。
 * 因为原本的 SessionHandler 类不满足 Laravel 5.x的要求，只能通过重写来满足框架的要求。
 */

namespace SSDB;

class LaravelSessionHandler extends SessionHandler {

    /**
     * @param Client $ssdb
     * @param int $ttl
     */
    public function __construct($save_path, $ttl = null, $prefix = 'sess_') {
        $this->_ttl = $ttl ?: ini_get('session.gc_maxlifetime');
        $this->_prefix = $prefix;

        $components = parse_url($save_path);
        
        if ($components === false || 
                !isset($components['scheme'], $components['host'], $components['port']) 
                || strtolower($components['scheme']) !== 'tcp') {
            throw new Exception('Invalid session.save_path: ' . $save_path);
        }
        
        $this->_client = new Client($components['host'], $components['port']);

        if (isset($components['query'])) {
            parse_str($components['query'], $query);
            if (isset($query['auth'])) {
                $this->_client->auth($query['auth']);
            }
        }
    }

    /**
     * @param string $save_path
     * @param string $name
     * @return boolean
     */
    public function open($save_path, $name) {
        return true;
    }
}

```

我是在ssdb/phpssdb包中继承并增加的。

以上代码因为用到了 ssdb ，所以，我们还需要安装 ssdb 客户端代码。

> 首先，在项目的 composer.json 中对应的节增加如下代码：

```php
	...
    "require": {
        ...
        "ssdb/phpssdb": "dev-master"
    }
	...
```

其次，在对应项目根目录下执行如下命令：
```shell
composer update
```

通过这样执行之后会在命令行输出以下内容，说明安装成功：
```
qinlj:laravel qlj$ composer update
Loading composer repositories with package information
Updating dependencies (including require-dev)
  - Installing ssdb/phpssdb (dev-master 4583553)
    Loading from cache

Writing lock file
Generating autoload files
Generating optimized class loader
```

#### 16.3）让 Laravel 接受 ssdb session 驱动

Laravel 框架提供了一个注册session扩展驱动类的方法。在项目根目录\app\Providers\AppServiceProvider.php文件中的boot()方法增加如下代码：
```php
// 注册 ssdb session 扩展。
Session::extend('ssdb', function($app)
{
	return new \SSDB\LaravelSessionHandler(config('session.connection'));
});
```
> 超级友情提示：Session、Cookie、Db等这种类对应的命名空间是：
```php
Illuminate\Support\Facades\Session
Illuminate\Support\Facades\Cookie
Illuminate\Support\Facades\Db
```

> 这是为了摒弃各种难用的底层驱动封装的方法提供的顶层友好的类。




### 17）静态资源



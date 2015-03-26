composer工具帮我们很好地管理了PHP依赖包的问题。但是，作为一名PHPer，我们总不能永远只使用别人的开发包。我们骄傲是程序员必须自己动手开发一个包给别人用或自用。

以下我将以github结合composer工具来进行示例讲解。

### 一、创建一个github项目

首先，你得有一个github账号。然后，创建一个github 项目。

1）登录github。点击顶部导航栏的“+”，选择“New repository”。跳转到创建页页。

2）Repository name 我填写的是"utils"。可以填写你想要的名字。Description 是可选的。这是对这个项目的描述。接着下面的选项是让我们填写该项目是“public”还是"private"。选择"public"。接着“Initialize this repository with a README”建议勾选，给我们创建初始化一个项目文档首页。其他选项保持默认即可。根据提示选择即可。

- public：所有人都能访问您创建的项目。这也是github推荐的方式。免费。
- private：收费。只有指定的人才能访问这个项目。

3）点击“Create Repository”按钮，即可创建成功。


其次，我们要将这个github项目clone到本地。

1）在本地电脑创建一个目录localgit（名字随意）且保证系统中已经安装了git命令。

2）已创建的github项目右下角有一个"HTTPS clone URL"，点击那里的复制按钮把 url 地址复制出来。我的是：
> https://github.com/winerQin/utils.git

3）在localgit父目录下。执行如下命令：
```shell
$ git clone https://github.com/winerQin/utils.git localgit
```
此命令会输出如下内容，代表执行成功：
```shell
Cloning into 'utils'...
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
Checking connectivity... done.
```

### 二、创建composer.json

用过composer命令安装第三方开发包的人一定对composer.json不陌生。composer.json文件定义了包名、包的描述、作者、授权协议等信息。总之，一个项目要调用这个开发包，通过composer.json可以知道该怎样去加载文件。

因为，我们在本地已经有了一个github项目localgit。进入这个目录。使用composer命令智能化生成composer.json。
```shell
$ composer init
```
此时，会提示我们输入包名：
```
Package name (<vendor>/<name>) [root/localgit]:
```
输入：winerqin/localgit 并回车确认。注意：不能用大写字母和特殊字符。

> 因为，我们使用的是github。那么，推荐vendor为小写的用户名。name为仓库名称。

回车之后会提示输入包描述：
```
Description []:
```
这个你自己看着办。我比较懒。直接回车。

这个时候，会提示你输入作者和邮箱。默认是你初始安装git命令时配置的作者和邮箱。
```
Author [winerQin <753814253@qq.com>]:
```
因为，我的就是这个，所以，我直接就回车了。接着会填写最低稳定版本:
```
Minimum Stability []:
```
我直接填写了dev。如果你乱填写，则会提示如下：
```
Invalid minimum stability "sdfsd". Must be empty or one of: stable, RC, beta, alpha, dev
```
接着：
```
License []:
```
直接回车。

接着:
```
Would you like to define your dependencies (require) interactively [yes]?
```

输入No。

接着：
```
Would you like to define your dev dependencies (require-dev) interactively [yes]?
```

输入No。

接着后面都输入Yes即可。其实，根据英文提示，都知道要干嘛了。

此时，在localgit目录下会生成一个composer.json文件。内容如下：
```
{
    "name": "winerqin/localgit",
    "license": "MIT",
    "authors": [
        {
            "name": "winerQin",
            "email": "753814253@qq.com"
        }
    ],
    "minimum-stability": "dev",
    "require": {}
}
```

其实，这个文件完全可以手工创建，按照这个格式。


### 三、创建项目内容

创建项目内容很简单，按照既定目录结构去创建目录和文件即可。然后，再到composer.json里面修改一下让其知道即可。

一般情况下一个完整的开发包结构如下：

-- src 源码目录。这个名称按照所有程序员的约定不会更改名称。必不可少的。虽然，可以改为其他名称。但是不推荐。
-- tests 单元测试目录。此目录不是必须的。一些超高规格的开发者会提供这个目录让便让其他程序员测试协作。

在 src 目录我们创建一个类：
```php
/**
 * 全局数据验证类。
 * @author winerQin
 */

namespace winer;

class Validator {

	/**
	 * 判断是否为合法的用户名。
	 * @param string $username
	 * @param int $min_len 最小长度。包含。
	 * @param int $max_len 最大长度。包含。
	 * @param string $mode 用户名模式：digit、alpha、digit_alpha、chinese、digit_alpha_chinese、mix
	 *			digit：数字
	 *			aplha：字母
	 *			digit_alpha：数字和字母
	 *			chinese：中文
	 *			digit_alpha_chinese：数字字母中文
	 *			mix：混合型：数字字母中文下划线破折号
	 * @return bool
	 */
	public static function is_username($username, $min_len, $max_len, $mode = 'mix') {
		switch ($mode) {
			case 'digit':
				return preg_match("/^\d{{$min_len},{$max_len}}$/", $username) ? true : false;
				break;

			case 'alpha':
				return preg_match("/^([a-zA-Z]){{$min_len},{$max_len}}$/", $username) ? true : false;
				break;

			case 'digit_alpha':
				return preg_match("/^([a-z0-9_-]){{$min_len},{$max_len}}$/", $username) ? true : false;
				break;

			case 'chinese':
				return (preg_match("/^[\x7f-\xff]{{$min_len},{$max_len}}$/", $username)) ? true : false;
				break;

			case 'digit_alpha_chinese':
				return (preg_match("/^[\x7f-\xff|0-9a-zA-Z]{{$min_len},{$max_len}}$/", $username)) ? true : false;
				break;

			case 'mix':
			default:
				return (preg_match("/^[\x7f-\xff|0-9a-zA-Z-_]{{$min_len},{$max_len}}$/", $username)) ? true : false;
				break;
		}
	}

	/**
	 * 判断是否为合法的密码。
	 * @param string $password
	 * @param int $min_len 最小长度。包含。
	 * @param int $max_len 最大长度。包含。
	 * @param string $mode 用户名模式：digit_alpha、mix
	 *			digit_alpha：数字和字母
	 *			mix：混合型：数字字母下划线破折号
	 * @return bool
	 */
	public static function is_password($password, $min_len, $max_len, $mode = 'mix') {
		switch ($mode) {
			case 'digit_alpha':
				return preg_match("/^([a-z0-9]){{$min_len},{$max_len}}$/", $password) ? true : false;
				break;

			case 'mix':
			default:
				return (preg_match("/^[0-9a-zA-Z-_]{{$min_len},{$max_len}}$/", $password)) ? true : false;
				break;
		}
	}

	/**
	 * 判断是否为QQ号码。
	 * @param string $qq
	 * @return boolean
	 */
	public static function is_qq($qq) {
		return preg_match('/^[1-9]\d{4,12}$/', $qq) ? true : false;
	}

	/**
	 * 判断是否为手机号码。
	 * @param string $mobilephone
	 * @return boolean
	 */
	public static function is_mobilephone($mobilephone) {
		return preg_match('/^13[\d]{9}$|14^[0-9]\d{8}|^15[0-9]\d{8}$|^18[0-9]\d{8}$/', $mobilephone) ? true : false;
	}

	/**
	 * 判断是否为座机号码。
	 * @param string $telphone
	 * @return boolean
	 */
	public static function is_telphone($telphone) {
		return preg_match('/^((\(\d{2,3}\))|(\d{3}\-))?(\(0\d{2,3}\)|0\d{2,3}-)?[1-9]\d{6,7}(\-\d{1,4})?$/', $telphone) ? true : false;
	}

	/**
	 * 判断是否为邮政编码。
	 * @param string $zipcode
	 * @return boolean
	 */
	public static function is_zipcode($zipcode) {
		return preg_match('/^[1-9]\d{5}$/', $zipcode) ? true : false;
	}

	/**
	 * 判断字母是否在某个区域内。用于判断某个字符只能介于[a-h](包含)之间的类似情况。
	 * @param string $alpha 原值。
	 * @param string $start_alpha 起始值。
	 * @param string $end_alpha 截止值。
	 * @return boolean
	 */
	public static function alpha_between($alpha, $start_alpha, $end_alpha) {
		if (Validator::is_alpha($alpha) === false) {
			return false;
		}
		if (Validator::is_alpha($start_alpha) === false) {
			return false;
		}
		if (Validator::is_alpha($end_alpha) === false) {
			return false;
		}
		if ($start_alpha >= $end_alpha) {
			return false;
		}
		if ($alpha < $start_alpha) {
			return false;
		}
		if ($alpha > $end_alpha) {
			return false;
		}
		return true;
	}
	
	/**
	 * 判断数字是否在某个区域之间。[2, 10],包含边界值。
	 * @param int $value 原值。
	 * @param int $start_value 起始值。
	 * @param int $end_value 截止值。
	 * @return boolean
	 */
	public static function number_between($value, $start_value, $end_value) {
		if (is_numeric($value) === false || is_numeric($start_value) === false || is_numeric($end_value) === false) {
			return false;
		}
		if ($start_value >= $end_value) {
			return false;
		}
		if ($value < $start_value) {
			return false;
		}
		if ($value > $end_value) {
			return false;
		}
		return true;
	}

	/**
	 * 验证是否为中文。
	 * @param string $char
	 * @return bool
	 */
	public static function is_chinese($char) {
		if (strlen($char) === 0) {
			return false;
		}
		return (preg_match("/^[\x7f-\xff]+$/", $char)) ? true : false;
	}

	/**
	 * 判断是否为字母、数字、下划线（_）、破折号（-）。
	 * @param string $str
	 * @return boolean
	 */
	public static function is_alpha_dash($str) {
		return preg_match('/^([a-z0-9_-])+$/i', $str) ? true : false;
	}
	
	/**
	 * 验证身份证号码是否合法。
	 * @param string $vStr
	 * @return bool
	 */
	public static function is_idcard($vStr) {
		$vCity = array(
	        '11','12','13','14','15','21','22',
	        '23','31','32','33','34','35','36',
	        '37','41','42','43','44','45','46',
	        '50','51','52','53','54','61','62',
	        '63','64','65','71','81','82','91'
	    );
	    if (!preg_match('/^([\d]{17}[xX\d]|[\d]{15})$/', $vStr)) {
	    	return false;
	    }
	    if (!in_array(substr($vStr, 0, 2), $vCity)) {
	    	return false;
	    }
	    $vStr = preg_replace('/[xX]$/i', 'a', $vStr);
	    $vLength = strlen($vStr);
	 
	    if ($vLength == 18) {
	        $vBirthday = substr($vStr, 6, 4) . '-' . substr($vStr, 10, 2) . '-' . substr($vStr, 12, 2);
	    } else {
	        $vBirthday = '19' . substr($vStr, 6, 2) . '-' . substr($vStr, 8, 2) . '-' . substr($vStr, 10, 2);
	    }
	    if (date('Y-m-d', strtotime($vBirthday)) != $vBirthday) {
	    	return false;
	    }
	    if ($vLength == 18) {
	        $vSum = 0;
	        for ($i = 17 ; $i >= 0 ; $i--) {
	            $vSubStr = substr($vStr, 17 - $i, 1);
	            $vSum += (pow(2, $i) % 11) * (($vSubStr == 'a') ? 10 : intval($vSubStr, 11));
	        }
	        if($vSum % 11 != 1) {
	        	return false;
	        }
	    }
	    return true;
	}
	
	/**
	 * 验证日期时间格式。
	 * -- 1、验证$value是否为$format格式。
	 * -- 2、只能验证格式，不能验证时间是否正确。比如：2014-22-22
	 * @param string $format 格式。格式如：Y-m-d 或H:i:s
	 * @param string $value 日期。
	 * @return boolean
	 */
	public static function is_date_format($format, $value) {
		return date_create_from_format($format, $value) !== false;
	}
	
	/**
	 * 判断是否为整数。
	 * @param string $str
	 * @return boolean
	 */
	public static function is_integer($str) {
		return filter_var($str, FILTER_VALIDATE_INT) !== false;
	}
	
	/**
	 * 判断是否为字母数字。
	 * @param string $str
	 * @return boolean
	 */
	public static function is_alpha_number($str) {
		return preg_match('/^([a-z0-9])+$/i', $str) ? true : false;
	}

	/**
	 * 判断是否为字母。
	 * @param string $str
	 * @return boolean
	 */
	public static function is_alpha($str) {
		return preg_match('/^([a-z])+$/i', $str) ? true : false;
	}
	
	/**
	 * 验证IP是否合法。
	 * @param string $ip
	 * @return bool
	 */
	public static function is_ip($ip) {
		return filter_var($ip, FILTER_VALIDATE_IP) !== false;
	}
	
	/**
	 * 验证URL是否合法。
	 * -- 合法的URL：http://www.baidu.com
	 * @param string $url
	 * @return bool
	 */
	public static function is_url($url) {
		return filter_var($url, FILTER_VALIDATE_URL) !== false;
	}

	/**
	 * 判断email格式是否正确。
	 * @param string $email
	 * @return bool
	 */
	function is_email($email) {
		return filter_var($email, FILTER_VALIDATE_EMAIL) !== false;
	}

	/**
	 * 是否必需。
	 * @param string $str
	 * @return bool
	 */
	public static function is_require($str) {
		return strlen($str) ? true : false;
	}

	/**
	 * 判断字符串是否为utf8编码，英文和半角字符返回ture。
	 * @param string $string
	 * @return bool
	 */
	public static function is_utf8($string) {
		return preg_match('%^(?:
					[\x09\x0A\x0D\x20-\x7E] # ASCII
					| [\xC2-\xDF][\x80-\xBF] # non-overlong 2-byte
					| \xE0[\xA0-\xBF][\x80-\xBF] # excluding overlongs
					| [\xE1-\xEC\xEE\xEF][\x80-\xBF]{2} # straight 3-byte
					| \xED[\x80-\x9F][\x80-\xBF] # excluding surrogates
					| \xF0[\x90-\xBF][\x80-\xBF]{2} # planes 1-3
					| [\xF1-\xF3][\x80-\xBF]{3} # planes 4-15
					| \xF4[\x80-\x8F][\x80-\xBF]{2} # plane 16
					)*$%xs', $string) ? true : false;
	}
	
	/**
	 * 检查字符串长度。
	 * @param string $str 字符串。
	 * @param int $min 最小长度。
	 * @param int $max 最大长度。
	 * @param bool $is_utf8 是否UTF-8字符。
	 * @return boolean
	 */
	public static function len($str, $min = 0, $max = 255, $is_utf8 = false) {
		$len = $is_utf8 ? mb_strlen($str) : strlen($str);
		if (($len >= $min) && ($len <= $max)) {
			return true;
		} else {
			return false;
		}
	}
}
```

修改composer.json。修改后的内容如下：
```
{
    "name": "winerqin/utils",
    "license": "MIT",
    "authors": [
        {
            "name": "winerQin",
            "email": "753814253@qq.com"
        }
    ],
    "minimum-stability": "dev",
    "require": {
		"php": ">=5.3.0"
	},
	"autoload": {
        "psr-4": {
            "winer\\": "src/",
        }
    }
}
```

关于 PSR-4规范，请看文档：[我去](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md)

关于composer.json说明请看此博文：[我去](http://my.oschina.net/u/248080/blog/359008)


到此，我们的开发包已经搞定。接下来我们测试测试这个包是否可用。


### 四、测试开发包

在localgit目录下，通过composer来安装测试。

```shell
$ composer install
Loading composer repositories with package information
Installing dependencies (including require-dev)
Nothing to install or update
Writing lock file
Generating autoload files
```
输出以上信息说明安装成功咯。

在目录下会有一个vendor文件夹。

此时会在vendor/composer/autoload_psr4.php中生成命名空间和目录的映射关系，被包在一个数组中：
```php
<?php

// autoload_psr4.php @generated by Composer

$vendorDir = dirname(dirname(__FILE__));
$baseDir = dirname($vendorDir);

return array(
    'winer\\' => array($baseDir . '/src'),
);
```

如果发布成packagist包然后进行安装的话，到时候这里就不是$baseDir了而是$vendorDir。稍后我们再发布到packagist。

在tests目录创建 ValidatorTest.php，内容如下：
```php
<?php
require '../vendor/autoload.php';

use \winer;

$mobilephone = '18665027895';
$ok = winer\Validator::is_mobilephone($mobilephone);
if ($ok) {
	echo '是手机号码';
} else {
	echo '不是手机号码';
}

```

通过php命令来运行：
```php
php ValidatorTest.php
```

输入：
```
是手机号码
```

### 五、发布到packagist

packagist指的是packagist.org。这个网站是composer默认下载开发包的资源引用网站。所以，我们得在此网站注册一个账号。然后添加我们开发包的github项目。

1）将本地代码提交到github。
在发布之前，我们要把localgit的代码提交到github上去。
```shell
$ git add .
$ git commit -m "composer.json教程代码"
$ git push
```

此时，访问github的utils仓库，发现我们的刚刚提交的代码都已经安静地在里面了。


2）发布g到packagist

请确认已经登录。在packagist.org首页的右上角有一个"Submit Package"按钮。点击即可进入开发包提交的界面。在页面最下方有一个输入框，输入我们github的utils项目地址：
> https://github.com/winerQin/utils

再点击"Check ->"，经过几秒钟的等待会显示验证成功，并显示出提交的按钮。点击提交即完成了开发包的提交了。

那么，接下来这个开发包就可以在任何支持composer的PHP框架中使用了。恭喜您！！！

3）让我们的github代码更新能让packagist.org自动更新

登录github。选择代码仓库。我的是utils。然后，右边选择"settings" -> “Webhooks & Services” -> "Add Service" -> 下拉列表中选择 "packagist" -> 根据提示填写packagist账号，以及packagist提供的token。域名填写：http://packagist.org。

就这样，以后更新就会自动更新packagist.org上了。是不是好开心？是的。

> 超级提示：我们可以模仿已有的第三方包进行开发。

关于git中文文档请看：[我去](http://git.oschina.net/progit/index.html)
# think-addons

The ThinkPHP 6 Addons Package

此版本基于 zzstudio/think-addons 二开部分代码

## 安装

> composer require xianrenqh/think-addons

## 配置

### 生成配置

系统安装后会自动在 config 目录中生成 addons.php 的配置文件， 如果系统未生成可在命令行执行

```php
php think addons:config 
```

快速生成配置文件

### 公共配置

```php
'addons' => [
    // 是否自动读取取插件钩子配置信息（默认是开启）
    'autoload' => true,
    // 当关闭自动获取配置时需要手动配置hooks信息
    'hooks' => [
	    // 可以定义多个钩子
        'testhook'=>'test' // 键为钩子名称，用于在业务中自定义钩子处理，值为实现该钩子的插件，
					// 多个插件可以用数组也可以用逗号分割
	],
    'route' => [],
    'service' => [],
];
```

或者在\config目录中新建`addons.php`,内容为：

```php
<?php
return [
	// 是否自动读取取插件钩子配置信息
    'autoload' => false,
    // 当关闭自动获取配置时需要手动配置hooks信息
    'hooks' => [
        // 可以定义多个钩子
        'testhook'=>'test' // 键为钩子名称，用于在业务中自定义钩子处理，值为实现该钩子的插件，
                    // 多个插件可以用数组也可以用逗号分割
    ],
    'route' => [],
    'service' => [],
];
```

## 创建插件

> 创建的插件可以在view视图中使用，也可以在php业务中使用

安装完成后访问系统时会在项目根目录生成名为`addons`的目录，在该目录中创建需要的插件。

下面写一个例子：

### 创建test插件

> 在addons目录中创建test目录

### 创建钩子实现类

> 在test目录中创建 Test.php 类文件。注意：类文件首字母需大写，文件夹要和目录名保持一直

```php
<?php

namespace addons\test;

use think\App;
use think\Addons;

class Test extends Addons
{

    //后台菜单
    public $admin_list = [
        [
            //方法名称
            "action" => "index",
            //菜单标题
            'title'  => '测试添加插件菜单',
            //icon图标，默认为 fa-list
            'icon'   => '',
            //附加参数 例如：a=12&id=777
            "params" => "",
            //默认排序
            'sort'   => 0,
            //状态，1是显示，0是不显示
            'status' => 1
        ],
        [
            //方法名称
            "action" => "index2",
            //菜单标题
            'title'  => '测试添加插件菜单',
            //icon图标，默认为 fa-list
            'icon'   => '',
            //附加参数 例如：a=12&id=777
            "params" => "",
            //默认排序
            'sort'   => 0,
            //状态，1是显示，0是不显示
            'status' => 1
        ]
    ];

    // 该插件的基础信息
    /*public $info = [
        'name'        => 'test',    // 插件标识
        'title'       => '插件测试',    // 插件名称
        'description' => 'thinkph6插件测试',    // 插件简介
        'status'      => 1,    // 状态
        'author'      => 'byron sampson',
        'version'     => '0.1'
    ];*/

    /**
     * 插件安装方法
     * @return bool
     */
    public function install()
    {
        return true;
    }

    /**
     * 插件卸载方法
     * @return bool
     */
    public function uninstall()
    {
        return true;
    }

    /**
     * 实现的testhook钩子方法
     * @return mixed
     */
    public function testhook($param)
    {
        // 调用钩子时候的参数信息
        dump($param);
        // 当前插件的配置信息，配置信息存在当前目录的config.php文件中，见下方
        dump($this->getConfig());

        // 可以返回模板，模板文件默认读取的为插件目录中的文件。模板名不能为空！
        return $this->fetch('info');
    }
}
```

### 创建插件配置文件

> 在test目录中创建config.php类文件，插件配置文件可以省略。

```php
<?php
return array(
    'display'  => array(
        'name'    => 'display',
        'title'   => '是否显示:',
        'type'    => 'radio',
        'options' => array(
            1 => '显示',
            0 => '不显示',
        ),
        'value'   => '0',
    ),
    'appid'    => array(
        'name'  => 'appid',
        'title' => '熊掌号appid',
        'type'  => 'text',
        'value' => '1111',
        'tip'   => '熊掌号请前往<a href="https://ziyuan.baidu.com/xzh/home/index" target="_blank">熊掌号平台</a>获取Appid和Token',
    ),
    'type'     => array(
        'name'    => 'type',
        'title'   => '开启同步登陆：',
        'type'    => 'checkbox',
        'options' => array(
            'qq'     => 'qq',
            'sian'   => 'sian',
            'weixin' => 'weixin',
        ),
        'value'   => 'sian,weixin',
    ),
    'textarea' => array(
        'name'  => 'textarea',
        'title' => '测试textarea：',
        'type'  => 'textarea',
        'value' => 'testdddddd',
        'tip'   => '测试textarea提示',
    ),
    'compress' => array(
        'name'    => 'compress',
        'title'   => '是否启用压缩',
        'type'    => 'select',
        'options' => array(
            1 => '启用压缩',
            0 => '不启用',
        ),
        'value'   => '0',
        'tip'     => '压缩备份文件需要PHP环境支持gzopen,gzwrite函数',
    ),
    'pic'      => array(
        'name'  => 'pic',
        'title' => '固定图片',
        'type'  => 'image',
        'value' => '',
        'tip'   => '选择固定则需要上传此图片',
    ),
);

```

### 创建插件说明文件：info.ini

~~~
name = test
title = 测试
description = 测试
author = 小灰灰
website = https://xiaohuihui.net.cn
version = 1.0.0
has_adminlist = 1
status = 1
~~~

### 创建钩子模板文件

> 在test->view->index目录中创建info.html模板文件，钩子在使用fetch方法时对应的模板文件。

```html
<h1>hello tpl</h1>

如果插件中需要有链接或提交数据的业务，可以在插件中创建controller业务文件，
要访问插件中的controller时使用addon_url生成url链接。
如下：
<a href="{:addons_url('Action/link')}">link test</a>
或
<a href="{:addons_url('test://Action/link')}">link test</a>
格式为：
test为插件名，Action为controller中的类名[多级控制器可以用.分割]，link为controller中的方法
```

### 创建插件的controller文件

> 在test目录中创建controller目录，在controller目录中创建Index.php文件
> controller类的用法与tp6中的controller一致

```php
<?php
namespace addons\test\controller;
class Index
{
    public function link()
    {
        echo 'hello link';
    }
}
```

## 使用钩子

> 创建好插件后就可以在正常业务中使用该插件中的钩子了
> 使用钩子的时候第二个参数可以省略

### 模板中使用钩子

```html

<div>{:hook('testhook', ['id'=>1])}</div>
```

### php业务中使用

> 只要是thinkphp6正常流程中的任意位置均可以使用

```php
hook('testhook', ['id'=>1])
```

### 插件公共方法

```php
/**
 * 处理插件钩子
 * @param string $event 钩子名称
 * @param array|null $params 传入参数
 * @param bool $once 是否只返回一个结果
 * @return mixed
 */
function hook($event, $params = null, bool $once = false);

/**
 * 读取插件的基础信息
 * @param string $name 插件名
 * @return array
 */
function get_addons_info($name);

/**
 * 获取插件Plugin的单例
 * @param string $name 插件名
 * @return mixed|null
 */
function get_addons_instance($name);

/**
 * 插件显示内容里生成访问插件的url
 * @param $url 在插件控制器中可忽略插件名，在非插件中生成时需指定插件名。例：插件名://控制器/方法
 * @param array $param
 * @param bool|string $suffix 生成的URL后缀
 * @param bool|string $domain 域名
 * @return bool|string
 */
function addons_url($url = '', $param = [], $suffix = true, $domain = false);
```

```php
/**
 * 获取完整配置信息
* @param $name 插件名
* @return void
 */
function get_addons_fullconfig($name)

/**
 * 写入配置文件.
 * @param string $name  插件名
 * @param array  $array 配置数据
 * @return bool
 * @throws Exception
 */
function set_addons_fullconfig($name, $array)


/**
* 获取插件列表
 */
function get_plugins_list()
```

## 插件目录结构

### 最终生成的目录结构为

```html
www  WEB部署目录（或者子目录）
├─addons           插件目录
├─app           应用目录
│  ├─controller      控制器目录
│  ├─model           模型目录
│  ├─ ...            更多类库目录
│  │
│  ├─common.php         公共函数文件
│  └─event.php          事件定义文件
│
├─config                配置目录
│  ├─app.php            应用配置
│  ├─cache.php          缓存配置
│  ├─console.php        控制台配置
│  ├─cookie.php         Cookie配置
│  ├─database.php       数据库配置
│  ├─filesystem.php     文件磁盘配置
│  ├─lang.php           多语言配置
│  ├─log.php            日志配置
│  ├─middleware.php     中间件配置
│  ├─route.php          URL和路由配置
│  ├─session.php        Session配置
│  ├─trace.php          Trace配置
│  └─view.php           视图配置
│
├─view            视图目录
├─route                 路由定义目录
│  ├─route.php          路由定义文件
│  └─ ...
│
├─public                WEB目录（对外访问目录）
│  ├─index.php          入口文件
│  ├─router.php         快速测试文件
│  └─.htaccess          用于apache的重写
│
├─extend                扩展类库目录
├─runtime               应用的运行时目录（可写，可定制）
├─vendor                Composer类库目录
├─.example.env          环境变量示例文件
├─composer.json         composer 定义文件
├─LICENSE.txt           授权说明文件
├─README.md             README 文件
├─think                 命令行入口文件
```

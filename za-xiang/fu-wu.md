# 轻量服务

### 初衷

应用容器较臃肿，不适合开发插件和小型主题。可制作插件时，想用面向对象，且不想创建常量和全局变量，因此决定封装一个更精简的应用容器，即轻量服务。

### 服务

轻量服务具备以下特点：

1. 加载插件就实例化服务，因此服务是最简单的类，仅实现应用容器最需要的简版功能
2. 创建服务后添加钩子，服务的一些方法也是钩子执行的回调函数。
3. 实现功能的前提条件是实例化服务

**服务类**

服务继承抽象类  `Impack\WP\Service\Base` ，且需要实现 `provider` 方法，方法用于添加钩子和注册单例。简单的服务类示例：

```php
<?php

namespace App;

use Impack\WP\Service\Base;

class Service extends Base
{
    protected static $instance; // 缓存单例
    
    public function provider()
    {
        $this->singleton('view', \Impack\WP\Service\View::class);

        $this->bindAction(null, 'init');
    }
    
    public function init()
    {
        // init钩子执行的代码
    }
}
```

如果用到路径功能，可以重写构造函数，在实例化时就执行一些方法：

```php
public function __construct($path, $URL = '')
{
    static::setInstance($this)->setPath($path)->setURL($URL);
    $this->provider();
}
```

{% hint style="success" %}
服务的名称前缀默认是 imwp ，如需自定义请重写 $prefix 属性。名称前缀很重要，可以避免读写数据库时混用其他插件的数据
{% endhint %}

插件将在入口文件中按需求创建服务。例如，仅在访问后台管理时创建服务：

```php
if(is_admin()){
    (new \App\Service())->provider();
}
```

或者全局创建具备路径功能的服务：

```php
new \App\Service(plugin_dir_path(__FILE__), plugin_dir_url(__FILE__));
```

**基础方法**：

|       方法       |            说明           |
| :------------: | :---------------------: |
|     prefix     |        给指定键名添加前缀        |
|   bindAction   |      指定对象方法与动作钩子绑定      |
|   bindFilter   |      指定对象方法与过滤钩子绑定      |
|    singleton   |           注册单例          |
|      make      |           解析单例          |
|    instance    |           添加单例          |
| forgetInstance |         移除已解析的单例        |
|     create     |  static::  make方法的静态用法  |
|   getInstance  |     static::  返回服务单例    |
|        i       | static::  getInstance别名 |

### 路径处理

`Impack\WP\Service\PathTrait` 为服务提供了路径和URL相关的功能。

|          方法          |            说明           |
| :------------------: | :---------------------: |
|    setPath/setURL    |     分别是设置执行目录和对应URL     |
|         path         |          返回执行目录         |
|          URL         |        返回执行目录URL        |
| publicPath/publicURL | 静态资源的根目录和URL，默认 /assets |
|       viewPath       |   模板文件目录，默认 /templates  |
|      configPath      |    配置文件目录，默认 /config    |

### 模板视图

&#x20;`Impack\WP\Service\View` 简单提供加载模板的功能。

#### **用法：**

1. 在服务类注册单例：`$this->singleton('view', \Impack\WP\Service\View::class);`
2.  templates 目录下创建模板文件，例如 **header.php**&#x20;

    ```php
    <?php extract($args); ?>

    <header>
        <h1><?=$title;?></h1>
    </header>
    ```
3.  渲染模板

    ```php
    Service::create('view')->render('header', ['title' => '标题']);
    ```

#### **视图文件查找规则**：

先查找执行目录的 `.view.php` 文件，不存在时查找 `viewPath` 目录下的 `.php` 文件。

#### **公共方法**：

|        方法       |                   参数                  |         说明        |
| :-------------: | :-----------------------------------: | :---------------: |
|      render     | $filename, $data = \[], $once = false |       加载模板文件      |
| getTemplateFile |               $filename               | 解析模板文件路径（文件可能不存在） |

### 配置

`Impack\WP\Service\Config` **** 简单提供读取服务配置项的功能。这些选项是程序运行时用到的参数信息，并非数据库的 option ，选项以数组形式写到php文件中，程序运行时可以修改加载到内存的数据，但不能直接修改文件里的数据。

#### **用法：**

1. 在服务类注册单例：`$this->singleton('config', \Impack\WP\Service\Config::class);`
2.  插件目录下创建配置文件 **service.config.php**&#x20;

    ```php
    <?php

    return [
        'admin_pages' => [
            [
                'title'  => '服务后台设置页面',
                'render' => \App\Admin\Pages\ServicePage::class,
            ],
        ],
    ];
    ```
3. 读取选项： `Service::create('config')->get('admin_pages')`

#### 配置文件查找规则：

先查找服务执行目录的 `service.config.php` 文件，不存在时查找 `configPath` 目录下的 `service.php` 文件。

#### **公共方法：**

|   方法   |           参数          |   说明   |
| :----: | :-------------------: | :----: |
|   get  | $key, $default = null |  读取选项  |
|   set  |      $key, $value     |  设置选项  |
| forget |          $key         |  移除选项  |
| getAll |           -           | 读取所有选项 |

{% hint style="success" %}
服务的定位是轻量的，因此默认仅识别一个名为 service.config.php 的配置文件，如果项目存在多个配置文件，则需要其他强大的类，或者扩展这个类。
{% endhint %}

### 选项模型

&#x20;`Impack\WP\Service\Option` 实现了简单的option数据模型，支持与表单字段关联，读写配置时不需考虑键名前缀，底层自动处理；不需要指定选项默认值，支持自动使用表单配置的默认值。

#### **用法：**

1.  实现模型类。例如 **\App\Options\Test**

    ```php
    <?php

    namespace App\Options;

    use Impack\WP\Service\Option;

    class Test extends Option
    {
        protected $name = 'test-fields';
    }
    ```
2.  创建表单配置文件，文件名是 **$name** 属性值。例如 **config/test-fields.php** 文件内容：

    ```php
    <?php

    return [
        'title'   => [
            'label' => '标题',
            'value' => '一个标题',
        ],
        'content' => [
            'label' => '内容',
            'value' => '好多内容',
            'type'  => 'textarea',
        ],
    ];
    ```
3. 注册单例： `$this->singleton('testOption', \App\Options\Test::class);`
4. 读取选项： `Service::create('testOption')->get('title');`

#### 表单配置文件查找规则：

表单配置和[服务配置项](fu-wu.md#pei-zhi)的规则一样。

#### **公共方法：**

|       方法      |           参数          |        说明       |
| :-----------: | :-------------------: | :-------------: |
|      save     |         $data         | 保存一组数据，键名前缀可有可无 |
|      get      | $key, $default = null |       读取选项      |
|      set      |      $key, $value     |       设置选项      |
|     delete    |          $key         |       删除选项      |
|     getAll    |           -           |      读取所有选项     |
|   deleteAll   |           -           |      删除所有选项     |
|   getDefault  |      $key = null      |   读取全部或指定的默认值   |
|   getFields   |           -           |      读取表单配置     |
|    getName    |           -           |     返回表单配置名称    |
|    fullKey    |          $key         |    返回带前缀的完整键名   |
| getFieldValue |         $field        |    检索表单字段的默认值   |

{% hint style="success" %}
选项读取后缓存到对象内存，下次读取直接从缓存获得数据，不重新查询数据库。设置和删除选项不仅影响缓存的数据，也同时更改数据库数据。
{% endhint %}


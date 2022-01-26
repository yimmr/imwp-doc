# 核心

### **概要**

核心类提供路径拼接、绑定钩子和管理名称前缀等基本功能。使用这些功能前主类需要继承核心类 `Impack\WP\Base\Core` 并实现 `provider()` 方法，此方法将在实例化时自动执行，因此可以在方法内**添加钩子**、**注册单例**等。一个简单的例子：

```php
<?php
namespace App;

use Impack\WP\Base\Core;

class Theme extends Core
{
    protected static $instance;

    public function provider()
    {
        $this->action('after_setup_theme');
    }
    
    public function after_setup_theme()
    {
        // do something...
    }
}
```

### 核心类属性

|     名称    |  权限 | 默认值 |                                 说明                                 |
| :-------: | :-: | :-: | :----------------------------------------------------------------: |
|   config  |  公开 |     |                        可选的把属性指向 `config` 单例                        |
|   prefix  | 受保护 |     |                   名称前缀。一般用在 `meta` 和 `option` 键名                   |
|  path/url | 受保护 |     |                                基本路径                                |
| instances | 受保护 | \[] | 单例列表，可直接添加单例到数组中，或使用 [ContainerTrait](kuo-zhan.md#dan-li-rong-qi)  |
|  bindings | 受保护 | \[] | 类名列表，可直接添加类名到数组中，或使用 [ContainerTrait](kuo-zhan.md#dan-li-rong-qi)  |

### **核心类支持的方法**

|             方法             |          说明         |
| :------------------------: | :-----------------: |
|           prefix           |       创建带前缀的名称      |
|          hiddenKey         | 创建带前缀且隐藏的 `MetaKey` |
|           action           |     动作钩子与对象方法绑定     |
|           filter           |     过滤钩子与对象方法绑定     |
| path/publicPath/configPath |  拼接路径（基于实例化时提供的目录）  |
|        URL/publicURL       | 拼接URL（基于实例化时提供的URL） |
|              i             |   （**静态方法**）返回主类单例  |

### **实例化主类**

主类一般在主题 `functions.php` 文件中实例化，文件中还可以引入 `composer` 实现类自动加载：

```php
<?php
require_once __DIR__ . '/vendor/autoload.php';

new \App\Theme(get_stylesheet_directory(), get_stylesheet_directory_uri(), 'mytheme');
```

{% hint style="success" %}
提供的路径不能尾随斜杠，也不自动去除斜杠，如果想预防意外错误可以这样写：<mark style="color:blue;">`new \App\Theme(rtrim(__DIR__,'\/'));`</mark> <mark style="color:blue;"></mark><mark style="color:blue;"></mark> 。
{% endhint %}

#### **快捷绑定钩子**

使用 `action` 和 `filter` 可以将对象方法与钩子绑定，此方式要求钩子名称与方法名一致。例如将主类实例的 `init` 方法添加到 `init` 钩子：

```php
public function provider()
{
    $this->action('init');
}
```

如果参数值是数组则可以将绑定外部对象方法：

```php
if(\is_admin()){
    $this->action([$object, 'admin_init']);
}
```

{% hint style="success" %}
如果普通方法使用驼峰式命名，钩子回调使用下划线式命名，有些时候就可以通过命名风格区分它们。
{% endhint %}

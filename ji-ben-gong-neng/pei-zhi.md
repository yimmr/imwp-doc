# 配置

### 概要

有时调用添加后台页面、Customize API 和自定义帖子类型等功能需要提供多个选项，如果将这些参数提取到单独的文件中，那么后续调整参数会变得更轻松，代码也更简洁。

### 配置文件

一般配置文件存放于主题的 `mytheme/config` 目录中，插件同理。配置文件约束：

* 仅支持 .php 文件
* **仅返回一个值，不写逻辑复杂的代码**
* 可执行三元运算、调用函数或方法等简单的代码

例如一个名为 `default.php` 的配置文件提供了页面主菜单的一些参数：

```php
<?php

return [
    'navs' => [
        'primary' => [
            'theme_location'  => 'primary',
            'name'            => __('主菜单', 'mytheme'),
            'container'       => 'nav',
            'container_id'    => 'primary-menu',
            'container_class' => 'menu',
            'menu_id'         => 'menu',
            'fallback_cb'     => false,
        ],
    ],
];
```

### 与配置文件交互

如果需求不复杂，可以简单自定义读取配置文件的方法，同时可用核心类的 `configPath()` 拼接文件路径。或者使用封装好的 `Impack\WP\Base\Config` 类来与配置文件交互，具体用法可参考下方教程。

主类使用 [ContainerTrait](kuo-zhan.md#dan-li-rong-qi) 时应调用 `useConfig()` 创建单例，反之手动创建单例，`Impack\WP\Base\Config` 类使用主类的 `configPath()` 获取文件路径，使用前确保主类已实现该方法。

#### **在 `provider()` 中创建 `Config` 单例**：

```php
public function provider()
{
    // 使用 ContainerTrait 时调用这个方法
    $this->useConfig();

    // 未使用 ContainerTrait 时手动创建单例
    $this->config = new \Impack\WP\Base\Config($this);
}
```

创建 Config 单例时并不加载配置文件，只有调用 `get()` 和 `has()` 时才加载配置文件，加载配置文件时数据将得到临时缓存，缓存持续到程序运行结束。此外，`set()` 和 `forget()` 仅作用于已缓存的数据，不修改配置文件。

{% hint style="success" %}
一个配置文件默认仅加载一次，如果想重新加载配置，应调用 `loadConfig(`) 方法，需提供不带扩展名的文件名，同时第二个参数为**真**。
{% endhint %}

#### **读取配置**

读取方式是以键名作为参数调用 `get()` 方法，顶层键名是不带扩展名的文件名，例如获取 `default.php` 文件返回的所有数据：

```php
$this->config->get('default');
```

想获取嵌套数组的指定值时，可使用点分隔符拼接键名的形式传参：

```php
$this->config->get('default.navs.primary');
```

#### **判断/设置/移除**

判断、设置及移除功能分别对应 `has()` 、 `set()` 、 `forget()` 方法对应，键名所支持的用法与 `get()` 一致。

{% hint style="danger" %}
调用 `set()` 方法时，若键名为 <mark style="color:yellow;">`null`</mark> ，将把所有缓存的数据重置为第二个参数提供的值，请确保值是数组，以免出现意外错误。&#x20;
{% endhint %}

#### 功能参考

|     名称     |           参数           |     说明     |
| :--------: | :--------------------: | :--------: |
|     has    |          $key          |   是否存在配置项  |
|     get    |  $key, $default = null |    读取配置值   |
|     set    |   $key, $value = null  |    设置配置项   |
|   forget   |          $keys         | 移除单个或一组配置项 |
| loadConfig | $name, $reload = false |   加载配置文件   |

****

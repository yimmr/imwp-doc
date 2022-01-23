# 配置文件

### 配置文件

配置文件存放于主题目录下的 `config` 目录，每个配置文件都是 PHP 文件，但它只返回一个数组。例如创建后台的配置文件 admin.php ：

```php
<?php

return [
    'menu_pages' => [],
];
```

### Config类

应用容器默认绑定了 `Impack\WP\Config` 类，别名是 `config` ，这个类的实例不仅可以读取配置文件数据，还可以增删改数据，但你不应该使用它修改配置文件，不仅影响文件格式，还可能导致数据丢失，因此你应该手动修改配置文件。

创建Config实例：

```php
$app->make('config');
```

调用 `get` 方法读取数据，支持用点分隔键名读取深层数组值，如下写法可读取配置文件 admin.php 的 menu\_pages 的值：

```php
$app->make('config')->get('admin.menu_pages');
```

如果觉得上面代码有点长，也可以使用数组写法：

```php
$app['config']['admin.menu_pages'];
```

{% hint style="success" %}
admin 是文件名后面是数组键名，如果键名只是 admin 将得到配置文件的全部数据。配置文件的数据会缓存到对象中，只会引入一次原文件
{% endhint %}

**四个交互方法：**

|   方法名  |    功能   |
| :----: | :-----: |
|   get  |  读取配置项  |
|   set  |  设置配置项  |
| forget |  移除配置项  |
|   has  | 是否存在配置项 |

**支持与 wp\_options 表交互**

Config类除了与配置文件交互，还支持与 `wp_options` 表交互，它基于WP `get_option` 函数实现，也支持增删改查。用法是将键名改为 `option` 并带上选项名，例如读取网站URL：

```php
echo $this->app->make('config')->get('option.siteurl');
```

已读的数据都缓存在 option 中，因此可以读取所有已读的选项：

```php
print_r($this->app->make('config')->get('option'));
```

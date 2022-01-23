# 菜单页面

菜单页面组件主要职责是提供页面视图，不区分子菜单或顶级菜单，你可以根据需求以顶级或子级菜单的形式添加，并且可以随时更改。

### 创建组件

组件需要继承类 `Impack\WP\Components\MenuPage` 并实现 `view` 方法。 `view` 方法输出页面视图，如果有样式脚本可以在静态方法 `enqueue` 引入。

```php
<?php

namespace App\Admin\Pages;

use Impack\WP\Components\MenuPage;

class Theme extends MenuPage
{
    public static function enqueue()
    {
        // 引入脚本样式
    }

    public function view()
    {
        // 输出页面
    }
}
```

### 添加菜单

父类仅实现替代 `add_menu_page` 相关函数的静态方法，因此依然要通过 `admin_menu` 钩子添加后台菜单页面。

单个添加菜单的方式是子类静态调用 `add` 方法，需要提供键值对数组的 [菜单配置](../cai-dan-ye-mian.md#cai-dan-ye-pei-zhi-xiang) 。子类调用该方法时可以省略 `render` 选项，因为子类就是页面组件。

添加顶级菜单：

```php
\App\Admin\Pages\Theme::add([
    'slug'   => 'mytheme',
    'render' => 'bloginfo',
]);
```

添加外观的子菜单：

```php
\App\Admin\Pages\Theme::add([
    'parent' => 'theme',
    'name'   => '子菜单',
    'slug'   => 'subtheme',
    'render' => 'bloginfo',
]);
```

批量添加菜单的方式是使用父类静态调用 `addMany` 方法：

```php
\Impack\WP\Components\MenuPage::addMany([
    [
        'slug'   => 'mytheme',
        'render' => 'bloginfo',
    ],
    [
        'slug'     => 'theme',
        'children' => [
            [
                'name'   => '子菜单',
                'slug'   => 'subtheme',
                'render' => 'bloginfo',
            ],
        ],
    ],
]);
```

**补充说明：**

* 批量添加时，请提供 `render` 项，没有该项页面可能无法显示
* `children` 的子菜单可以省略 `parent` 选项

### 页面样式

`Impack\WP\Components\MenuPage::enqueueScripts` 方法用于处理菜单页面的脚本样式，该方法不会全局引入，只引入当前页面用到的脚本样式。可以用作 `admin_enqueue` 的回调：

```php
\add_action('admin_enqueue_scripts', [Impack\WP\Components\MenuPage::class, 'enqueueScripts']);
```

### 页面属性

渲染页面时可以设置页面属性，页面属性将填充到组件对象的 `props` 数组，组件实现了未定义属性的魔术方法 **\_\_get/\_\_set** ，因此直接调用对象的未定义属性即可读取页面属性，此外 `getProps` 方法返回全部页面属性：

```php
$this->getProps(); // ['title' => '页面标题']
$this->title; // 页面标题
$this->title = '新标题';
$this->message = '简单消息'; // 在内部新增属性
```

{% hint style="info" %}
读取未定义的对象属性且props数组也没有相应值时，返回 null
{% endhint %}

### 提示消息

页面组件支持调用WP风格的提示消息，用法是使用指定的方法设置消息，前端将逐条显示这些消息。以下将显示成功和失败的消息：

```php
public function view()
{
    $this->success('保存成功');
    $this->error('保存失败');
    // ...form
}
```

支持的消息类型：

|    类型   |  备注 |
| :-----: | :-: |
| success |  成功 |
|  error  |  错误 |
|   info  |  提示 |
| warning |  警告 |

# Settings

&#x20;Settings API 可以在新的页面或设置页面添加分区和表单字段。

#### 概要

1. 表单提交至 `wp-admin/options.php` ，用户需具备 `manage_options` 权限（多站点中需是超级管理员）
2. 界面风格与 WordPress 一致
3. 让 WordPress 处理并保存表单数据
4. 包含安全验证 - 如随机数验证
5. 可以使用内部相同的数据清理方法

### 函数参考

|                                                 设置                                                |                                                  分区/字段                                                  |   |
| :-----------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------: | - |
|   [register\_setting()](https://developer.wordpress.org/reference/functions/register\_setting/)   | [add\_settings\_section()](https://developer.wordpress.org/reference/functions/add\_settings\_section/) |   |
| [unregister\_setting()](https://developer.wordpress.org/reference/functions/unregister\_setting/) |   [add\_settings\_field()](https://developer.wordpress.org/reference/functions/add\_settings\_field/)   |   |

|                                                  渲染表单选项                                                 |                                                   错误                                                  |   |
| :-----------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------: | - |
|       [settings\_fields()](https://developer.wordpress.org/reference/functions/settings\_fields/)       |  [add\_settings\_error()](https://developer.wordpress.org/reference/functions/add\_settings\_error/)  |   |
| [do\_settings\_sections()](https://developer.wordpress.org/reference/functions/do\_settings\_sections/) | [get\_settings\_errors()](https://developer.wordpress.org/reference/functions/get\_settings\_errors/) |   |
|   [do\_settings\_fields()](https://developer.wordpress.org/reference/functions/do\_settings\_fields/)   |      [settings\_errors()](https://developer.wordpress.org/reference/functions/settings\_errors/)      |   |

### 用法

一般在 `admin_init` 钩子中执行以下方法&#x20;

1\. **注册设置** - 将创建一个 option 。默认选项组 `general`、`discussion`、`media`、 `reading`、`writing` 和 `options` 。

```php
register_setting(
    string $option_group,
    string $option_name,
    callable $sanitize_callback = ''
);
```

2\. **添加分区** - 包含标题和一组表单字段的容器。默认页面 `general`, `reading`, `writing`, `discussion`、`media` 。

回调函数可接收一个包含 title, id, callback 的数组参数

```php
add_settings_section(
    string $id,
    string $title,
    callable $callback,
    string $page
);
```

3\. 回调函数可接收 - `$args` 参数也提供给回调函数

```php
add_settings_field(
    string $id,
    string $title,
    callable $callback,
    string $page,
    string $section = 'default',
    array $args = []
);
```

4\. 渲染页面 - 下列函数不包含 `<form>` 标签

```php
// 输出安全验证相关隐藏字段
settings_fields($option_group);
// 输出分区，函数自动调用 do_settings_fields()
do_settings_sections($page);
// 保存按钮
submit_button('Save');
```

### Options API

允许创建、读取、更新和删除 WordPress 选项。选项值可以是单值或值数组，**大量相关选项储存为数组性能较好**。

* [add\_option()](https://developer.wordpress.org/reference/functions/add\_option/) - 添加
* [get\_option()](https://developer.wordpress.org/reference/functions/get\_option/) - 取值
* [update\_option()](https://developer.wordpress.org/reference/functions/update\_option/) - 更新
* [delete\_option()](https://developer.wordpress.org/reference/functions/delete\_option/) - 删除
* 多站点使用 `{action}_site_option`&#x20;

### 范例

使用 `Settings` 类来组织单个页面的选项或一组选项：

```php
<?php

namespace App····\Admin;

use App\Theme;

class TestSettings
{
    public $theme;

    public $page;

    public function __construct(Theme $theme, $page)
    {
        $this->theme = $theme;
        $this->page  = $page;

        \register_setting($this->page, $this->theme->prefix('disable_sidebar'));

        \add_settings_section($this->page . '-section', null, null, $this->page);

        \add_settings_field(
            $this->theme->prefix('disable_sidebar'),
            __('禁用侧边栏', 'test'),
            [$this, 'outputField'],
            $this->page,
            $this->page . '-section'
        );
    }

    public function outputField()
    {
        echo '<input type="checkbox" name="' . $this->theme->prefix('disable_sidebar') . '"'
        . checked(1, $this->theme->getOption('disable_sidebar'), false) . 'value="1">';
    }
}
```

接着**主类中挂载钩子**，钩子执行时传入主类对象并实例化 `Settings` 类的对象，下面是主类的方法：

```php
public function admin_init()
{
    new \App\Admin\TestSettings($this, 'test-page');
}
```

构建实例时需要提供主类对象，所以 `Settings` 类方法中可以调用主类的 `getOption()` 方法，用这个方法获取选项值不需写名称前缀：

```php
public function outputField()
{
    echo '<input type="checkbox" name="' . $this->theme->prefix('disable_sidebar') . '"'
    . checked(1, $this->theme->getOption('disable_sidebar'), false) . 'value="1">';
}
```

结合[表单组件](zu-jian-1.md)使用时，上面的方法可以这样写：

```php
public function outputField()
{
    echo Form::checkbox($this->theme->prefix('disable_sidebar'), 1, $this->theme->getOption('disable_sidebar'));
}
```

在自定义页面使用时，可用 [页面组件](cai-dan-ye-mian.md) 输出所有HTML内容。例如添加一个 `test` 页面，然后使用页面组件创建一个渲染 `Settings API` 表单的闭包：

```php
\add_menu_page('test', 'test', 'manage_options', 'test', MenuPage::factory('测试页面标题', 'test-page'));
```

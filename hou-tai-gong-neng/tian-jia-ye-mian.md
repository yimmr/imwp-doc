# 设置

### 前提

使用 [Settings API](https://developer.wordpress.org/apis/handbook/settings/) 扩展选项，此接口优点：

* 自动拼接风格一致的HTML表格
* 自动验证权限并保存数据
* 能扩展默认的设置页面，也能在自定义页面使用

{% hint style="info" %}
自动保存需要把表单提交到 `wp-admin/options.php` ，但用户必须拥有 `manage_options` 权限（并且多站点中必需是超级管理员）。
{% endhint %}

### 范例

使用 `Settings` 类来组织单个页面的选项或一组选项：

```php
<?php

namespace App\Admin;

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

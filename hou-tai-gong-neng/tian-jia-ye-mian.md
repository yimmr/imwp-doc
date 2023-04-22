# Settings

使用 `options` 时可能还要创建可视化编辑界面，这时拼凑零碎的函数就变得琐碎，希望实现一种简单快速构建表单的方式：

* 基于 Settings API 扩展选项
* 由一个类 `\Impack\WP\Base\Option` 的实例管理一个选项，不需额外创建类
* 一个选项保存一组选项，并由一个实例管理
* 一个实例包含选项值、前缀、组和菜单Slug等相关信息
* 初始化实例，后续从实例值相关操作，不用管选项名、默认值等零杂参数
* 调用一个方法就添加对应菜单页面

#### 概要

1. 表单提交至 `wp-admin/options.php` ，用户需具备 `manage_options` 权限（多站点中需是超级管理员）
2. 界面风格与 WordPress 一致
3. 让 WordPress 处理并保存表单数据
4. 包含安全验证 - 如随机数验证
5. 可以使用内部相同的数据清理方法

### 函数参考

<table><thead><tr><th align="center">设置</th><th align="center">分区/字段</th><th data-hidden></th></tr></thead><tbody><tr><td align="center"><a href="https://developer.wordpress.org/reference/functions/register_setting/">register_setting()</a></td><td align="center"><a href="https://developer.wordpress.org/reference/functions/add_settings_section/">add_settings_section()</a></td><td></td></tr><tr><td align="center"><a href="https://developer.wordpress.org/reference/functions/unregister_setting/">unregister_setting()</a></td><td align="center"><a href="https://developer.wordpress.org/reference/functions/add_settings_field/">add_settings_field()</a></td><td></td></tr></tbody></table>

<table><thead><tr><th align="center">渲染表单选项</th><th align="center">错误</th><th data-hidden></th></tr></thead><tbody><tr><td align="center"><a href="https://developer.wordpress.org/reference/functions/settings_fields/">settings_fields()</a></td><td align="center"><a href="https://developer.wordpress.org/reference/functions/add_settings_error/">add_settings_error()</a></td><td></td></tr><tr><td align="center"><a href="https://developer.wordpress.org/reference/functions/do_settings_sections/">do_settings_sections()</a></td><td align="center"><a href="https://developer.wordpress.org/reference/functions/get_settings_errors/">get_settings_errors()</a></td><td></td></tr><tr><td align="center"><a href="https://developer.wordpress.org/reference/functions/do_settings_fields/">do_settings_fields()</a></td><td align="center"><a href="https://developer.wordpress.org/reference/functions/settings_errors/">settings_errors()</a></td><td></td></tr></tbody></table>

### 用法

* 字段挂到指定的 `section` ，使用 `id` 标识每个分区
* 可以挂默认 `section` ，也可以创建新的，

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

3\. **添加字段** - `$args` 参数也提供给回调函数

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

4\. **渲染页面** - 下列函数不包含 `<form>` 标签

```php
// 输出安全验证相关隐藏字段
settings_fields($option_group);
// 输出分区，函数自动调用 do_settings_fields()
do_settings_sections($page);
// 保存按钮
submit_button('Save');
```

5\. **通知信息** - WordPress 将在 URL 中添加 `settings-updated` 参数，可在页面中判断显示消息。

```php
if (isset($_GET['settings-updated'])) {
    add_settings_error('setting', 'code', __('Settings Saved'), 'updated');
}

settings_errors('setting');
```

### Options API

允许创建、读取、更新和删除 WordPress 选项。选项值可以是单值或值数组，**大量相关选项储存为数组性能较好**。

* [add\_option()](https://developer.wordpress.org/reference/functions/add\_option/) - 添加
* [get\_option()](https://developer.wordpress.org/reference/functions/get\_option/) - 取值
* [update\_option()](https://developer.wordpress.org/reference/functions/update\_option/) - 更新
* [delete\_option()](https://developer.wordpress.org/reference/functions/delete\_option/) - 删除
* 多站点使用 `{action}_site_option`&#x20;

### 基于配置文件注册

使用 <mark style="color:orange;">`Impack\WP\Manager\Settings`</mark> 静态类的 <mark style="color:purple;">`register()`</mark> 方法可以快速注册设置项。此方法接受一个数组参数，数据结构如下：

```php
[
    '$page' => [
        'settings' => [
            '$option_name' => array $register_setting_args
        ],
        'sections' => [
            '$section_id' => [
                'title' => string 'Section title',
                'description' => string 'Section description',
                'callback' => callable
                'fields' => [
                    '$id' => [
                        'title'             => string 'Field title',
                        'callback'          => callable,
                        'label_for'         => 'field_name',
                        'class'             => 'custom-class-name',
                        'custom_data'       => 'custom',
                    ]
                ]
            ]
        ]
    ]
]
```

#### 关于此结构：

* 数组键名 <mark style="color:red;">`$page`</mark> 也用作 <mark style="color:purple;">`register_setting`</mark> 的 <mark style="color:red;">`$option_group`</mark> 参数
* **settings** 用于注册选项，<mark style="color:red;">`$args`</mark> 默认为空数组，可设置默认值和 POST 提交的过滤器等。
* **sections** 中不提供 `title` 则不调用 <mark style="color:purple;">`add_settings_section`</mark> 注册分区，`description` 和 callback 两项二选一，两者都用来渲染分区描述，callback 可用自定义函数输出描述。
* **fields** 即表单字段，归类到指定 _section_ 的配置下。 <mark style="color:red;">`$id,title,callback`</mark> 都用作 <mark style="color:purple;">`add_settings_field`</mark> 的参数， `callback` 以外的数组项都用作此函数的 <mark style="color:red;">`$args`</mark> 参数（即可以在回调函数内接收这些数组项）。

简单例子：

<pre class="language-php"><code class="lang-php"><strong>&#x3C;?php
</strong>
add_action('admin_init', function(){
    \Impack\WP\Manager::register([
        'imwp' => [
            'settings' => [
                'test'  => [
                    'default' => 'default value',
                    'sanitize_callback' => 'sanitize_text_field'
                ], 
            ],
            'sections' => [
                'test' => [
                    'title'       => 'Section Title',
                    'description' => 'Section description.',
                    'fields'      => [
                        'id'  => [
                            'title' => '输入字段',
                            'callback' => 'imwp_test_field_cb',
                            'label_for' => 'imwp_test_field',
                            'custom_data' => 'custom',
                        ],
                    ],
                ],
            ],
        ],
    ]);
});

function imwp_test_field_cb($args)
{
    $options = get_option('imwp_test');
    printf('&#x3C;input name="%1$s" id="%1$s" value="%2$s" class="regular-text" data-custom="%3$s">', esc_attr($args['label_for']), $options, esc_attr($args['custom_data']));
}
</code></pre>

注册完成后，自定义的页面需要调用内置函数输出表单HTML：

```php
echo '<form method="post" action="options.php">';
\settings_fields('imwp');
\do_settings_sections('imwp');
\submit_button();
echo '</form>';
```

一个快速渲染内置风格表单的方法是调用 `MenuPage` 组件：

```php
add_action('admin_menu',function (){
     add_menu_page(
          'IMWP',
          'IMWP Options',
          'manage_options',
          'imwp_slug',
          fn() => \Impack\WP\Components\MenuPage::settingsAPIPage('imwp')
     );
});
```

{% hint style="success" %}
添加菜单的 `$slug` 参数可以与注册 `$settings` 的不同，因为渲染 Settings API 表单的函数需要提供的 `$page` 参数不是 `$menu_slug` 而是注册 `$settings` 的 `$page` 。
{% endhint %}

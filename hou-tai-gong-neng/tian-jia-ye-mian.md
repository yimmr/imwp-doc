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

使用 <mark style="color:orange;">`Impack\WP\Manager\Settings`</mark> 静态类的 <mark style="color:purple;">`register()`</mark> 方法可以快速注册设置选项。此方法接受一个数组参数，数据结构如下：

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
}</code></pre>

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
添加菜单的 ￥
{% endhint %}

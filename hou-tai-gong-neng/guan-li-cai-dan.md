# 管理菜单

#### 添加顶级菜单

```php
add_menu_page(
    string $page_title,
    string $menu_title,
    string $capability,
    string $menu_slug,
    callable $function = '',
    string $icon_url = '',
    int $position = null
);
```

#### 添加子级菜单

```php
add_submenu_page(
	string $parent_slug,
	string $page_title,
	string $menu_title,
	string $capability,
	string $menu_slug,
	callable $function = ''
);
```

[查看内置菜单父级 Slug](guan-li-cai-dan.md#fu-ji-slug)

#### 处理表单数据

1. 使用 [Settings API](settings.md) 。
2. 添加菜单页面的函数均返回一个 `$hookname` ，钩子 `load-$hookname` 在输出任何 HTML 前执行，可以用来处理表单数据。需要手动执行必要的检查：
3. 判断是否有表单提交 (`'POST' === $_SERVER['REQUEST_METHOD']`).
4. CSRF 验证
5. 数据清理和验证

#### 相关信息

1. 注册菜单钩子 `admin_menu` 。
2. **插件**可以将模块文件路径传给 `$menu_slug` 参数，而 `$function` 参数设为 `null` 。
3. 移除菜单 `remove_menu_page(string $menu_slug)` ，仅从 UI 上移除，用户仍可以直接访问。
4. 获取菜单页面 URL `menu_page_url($menu_slug)` 。

#### 父级 Slug 和添加对应子菜单的函数：

* [add\_dashboard\_page()](https://developer.wordpress.org/reference/functions/add\_dashboard\_page/) – `index.php`
* [add\_posts\_page()](https://developer.wordpress.org/reference/functions/add\_posts\_page/) – `edit.php`
* [add\_media\_page()](https://developer.wordpress.org/reference/functions/add\_media\_page/) – `upload.php`
* [add\_pages\_page()](https://developer.wordpress.org/reference/functions/add\_pages\_page/) – `edit.php?post_type=page`
* [add\_comments\_page()](https://developer.wordpress.org/reference/functions/add\_comments\_page/) – `edit-comments.php`
* [add\_theme\_page()](https://developer.wordpress.org/reference/functions/add\_theme\_page/) – `themes.php`
* [add\_plugins\_page()](https://developer.wordpress.org/reference/functions/add\_plugins\_page/) – `plugins.php`
* [add\_users\_page()](https://developer.wordpress.org/reference/functions/add\_users\_page/) – `users.php`
* [add\_management\_page()](https://developer.wordpress.org/reference/functions/add\_management\_page/) – `tools.php`
* [add\_options\_page()](https://developer.wordpress.org/reference/functions/add\_options\_page/) – `options-general.php`
* [add\_options\_page()](https://developer.wordpress.org/reference/functions/add\_options\_page/) – `settings.php`
* [add\_links\_page()](https://developer.wordpress.org/reference/functions/add\_links\_page/) – `link-manager.php` – requires a plugin since WP 3.5+
* 自定义帖子类型 – `edit.php?post_type=wporg_post_type`
* 网络管理员 – `settings.php`
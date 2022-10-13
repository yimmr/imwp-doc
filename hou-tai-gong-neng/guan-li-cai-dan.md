# 管理菜单

注册菜单钩子 `admin_menu` 。

添加顶级菜单

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

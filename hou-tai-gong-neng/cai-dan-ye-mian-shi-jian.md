# 菜单页面实践

### 创建页面类

页面类**不直接添加与菜单相关的钩子**，主要负责引入模板文件、脚本样式、使用数据服务获取数据。另外，也可负责验证请求，验证通过后交给数据服务处理。

```php
<?php

namespace APP\Admin;

class AdminPage
{
    public function load()
    {
        // 实例化数据服务
    }

    public function render()
    {
        // 引入模板文件
    }

    public function enqueueScripts()
    {
        // 引入脚本样式
    }
}
```

### 延迟实例化页面类

页面类的方法将用于输出菜单页面和钩子的回调，此时要先进行实例化，若不想在未打开页面时创建实例，可使用以下办法：

1. 页面类使用静态方法
2. 使用注册管理类 `Impack\WP\Manager\AdminRegister`

#### 使用注册管理类

```php
AdminRegister::bindAdminPage(TestPage::class, \add_menu_page(
  '自定义',
  '自定义页面标题',
  'manage_options',
  'custom-page-slug',
  fn() => AdminRegister::currentAdminPageCall('render')
));
```

注册管理类提供的相关方法：

<mark style="color:purple;">**`bindAdminPage`**</mark>**  **  - **** 注册页面类

* <mark style="color:red;">`$abstract`</mark>  页面类名
* <mark style="color:red;">`$hookSuffix`</mark>  添加菜单页面函数返回的钩子
* <mark style="color:red;">`$withLoad = false`</mark>  是否添加 `load-{hook_suffix}` 钩子

<mark style="color:purple;">**`bindAdminPageFull`**</mark>**  **  -  注册页面类并完整添加钩子，如 `load-{hook}` 、入队脚本样式。

* 参数有同上个方法的前两个参数

<mark style="color:purple;">**`currentAdminPageCall`**</mark>**  **  -  调用当前页面对象的一个方法

* <mark style="color:red;">`$method`</mark>  方法名

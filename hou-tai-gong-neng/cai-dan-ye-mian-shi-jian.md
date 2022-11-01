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
2. 使用主类容器

#### 使用主类容器示例

```php
<?php

namespace APP;

use APP\Admin\TestPage;
use Impack\WP\Base\ContainerTrait;
use Impack\WP\Base\PathTrait;

class Main
{
    use PathTrait, ContainerTrait;

    public function boot()
    {
        static::$instance = $this;
        \add_action('admin_menu', [$this, 'admin_menu']);
        \add_action('admin_enqueue_scripts', [$this, 'admin_enqueue_scripts']);
    }

    public function admin_menu()
    {
        $hookSuffix = \add_menu_page(
            '自定义页面',
            '自定义',
            'manage_options',
            'custom-page-slug',
            fn() => $this->make(\get_current_screen()->id)->render()
        );

        $this->singleton($hookSuffix, TestPage::class);

        \add_action('load-' . $hookSuffix, fn() => $this->make($hookSuffix)->load());
    }

    public function admin_enqueue_scripts($hookSuffix)
    {
        if ($this->has($hookSuffix)) {
            $this->make($hookSuffix)->enqueueScripts();
        }
    }
}
```

示例中在主类中以单例形式注册页面类，并使用箭头函数做回调，回调函数被调用时获取页面的单例并执行指定方法。

在引入脚本样式时会判断当前页面 `$hookSuffix` (id) 是否注册过页面类，若有则获取页面单例并引入该页面的脚本样式。

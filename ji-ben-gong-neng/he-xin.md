# 核心

核心类提供了拼接路径、读取配置文件、绑定钩子及管理单例等基础功能。使用这些功能前主题的主类需要继承核心类 `Impack\WP\Base\Core` 并实现 `provider()` 方法：

```php
<?php
namespace App;

use Impack\WP\Base\Core;

class Theme extends Core
{
    public function provider()
    {
        $this->useConfig();
    }
}
```

实例化主类时将自动执行实例的 `provider()` 方法，你可以在此方法内**添加钩子**、**注册单例**或**扩展主类**等。

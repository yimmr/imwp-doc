---
description: 本节主要讲解开发思路
---

# 视图&数据

### 视图

优先考虑使用内置函数。如，使用 `get_template_part()` 加载模板文件。

### 数据

优先考虑使用内置函数。如，使用 `get_posts()` 查询文章，同时该函数也提供了内置的数据对象和缓存功能。

复杂的数据查询/处理不直接写在模板文件中，应该使用 `Service` 类统一接管这些功能。

**服务类的定义（仅参考）：**

* 专注于提供某种数据，不直接与模板对应；
* 类在 `Service` 目录下创建，或使用类似 `NewsService` 这样的类名；
* 不是单例或静态类；
* 如有需要可自定义代理 WP 数据对象的类；
* 可调用第三方服务。

一个简单的服务类如下，主要用于处理与新闻相关的数据：

```php
<?php

namespace App\Service;

class News
{
    // 获取最新的5条新闻
    public function latest()
    {
        return \get_posts(['post_type' => 'post', 'posts_per_page' => 5]);
    }
}
```

# 入门

### 介绍

impack/wordpress（简称 imwp）是用面向对象方式开发 WordPress 主题的小型库。它把零散的函数组织成面向不同功能的类，使用这些类可以快速实现主题功能，同时也让代码变得简洁，更容易维护，有效提升开发者的编码体验。

{% hint style="success" %}
使用前，你需要了解关于 [PHP命名空间](https://www.php.net/manual/zh/language.namespaces.rationale.php)、[Composer](https://www.phpcomposer.com) 的基础知识。imwp的功能均由类实现，并使用 Composer 自动加载，这也能有效避免命名冲突。
{% endhint %}

### 安装方式

此库仅适用主题，安装方式是在主题目录下使用 composer 引入包，若未初始化包管理，先执行 `composer init` 再引入包：

```bash
composer require impack/wordpress
```

### 目录结构

一款主题的基本目录结构：

|       名称      |   说明   |
| :-----------: | :----: |
|      app      |  类所在目录 |
|     config    | 配置文件目录 |
|     public    | 静态资源目录 |
|   style.css   | 主题描述文件 |
|   index.php   |   首页   |
| functions.php | 主题入口文件 |

{% hint style="success" %}
**app** 是定义类的目录，与 composer 自动加载的命名空间绑定，文档的命名空间都是 `App` ，示例中的类都使用这个命名空间
{% endhint %}

### 入口文件

主题视为 WordPress 的基础应用，目录下的 `functions.php` 文件被定义为应用入口文件，入口文件只创建应用实例和执行基本方法，不写功能的具体逻辑：

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

// 设置主题目录
$app = new \App\Application(__DIR__); 
// 设置目录对应网址
$app->setUrl(get_stylesheet_directory_uri()); 

// $app->web(\App\Web::class);
// $app->rest(\App\REST::class);
// $app->admin(\App\Admin\Admin::class);

// 启动应用
$app->boot(); 
```

入口文件的代码基本只有这些，但此时代码还无法运行，因为我们还没有创建应用容器 `App\Application` 和不同环境下执行的类。[应用容器](ji-ben-gong-neng/fu-wu-rong-qi.md)将在下一章讲解，下面简单介绍服务类型，也就是上面注释部分的代码。

服务类型实现了前后台代码分开加载。如果在应用启动前绑定服务类型，就会在启动后根据当前环境创建对应的服务。如下分别绑定提供前台和后台服务的类，访问前台时只创建 `Web` 服务，不创建 `Admin` ，而访问后台时相反：

```php
$app->web(\App\Web::class);
$app->admin(\App\Admin\Admin::class);
```

{% hint style="success" %}
任何类都可以作为服务类型，它们都使用容器创建，支持依赖注入
{% endhint %}

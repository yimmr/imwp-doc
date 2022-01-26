# 入门

### 介绍

impack/wordpress（简称 imwp）封装了一些 WordPress 二次开发中可能用到的通用功能。**目前这是本人自用的代码库。**

{% hint style="success" %}
使用前，你需要了解关于 [PHP命名空间](https://www.php.net/manual/zh/language.namespaces.rationale.php)、[Composer](https://www.phpcomposer.com) 的基础知识。imwp 的功能均是PHP类，并使用 Composer 自动加载，这也能预防命名冲突。
{% endhint %}

### 安装方式

打开命令行工具并切换到主题/插件目录，使用 composer 引入包：

```bash
composer require impack/wordpress
```

### 目录结构

主题/插件的目录结构可参考 [WordPress 开发者文档](https://developer.wordpress.org)，此外可参考下方表格中的信息来组织代码：

|   名称   |   说明   |
| :----: | :----: |
|   app  |  类所在目录 |
| config | 配置文件目录 |
| assets | 静态资源目录 |

### 开发思路

插件主文件或主题 functions.php 文件当做自身入口文件，入口文件遵守以下规则：

* 不定义任何功能，只执行简短代码。如：初始化主类实例。
* 允许添加插件激活、暂停或卸载等此类钩子，其他类型的钩子均在主类方法中绑定。
* 文件代码简洁，易修改，不写逻辑复杂的代码。

每个主题/插件均有一个主类，通常使用：`Plugin` 、 `Theme` 、`Service` 这类名称做类名。主类的职责是绑定主要钩子、提供全局通用功能以及管理单例等。

优先使用 WordPress 提供的 API，当它们无法满足功能的需求时再引入第三方库。

{% hint style="success" %}
本文档除了涵盖 imwp 的教程，还包含了 WordPress 部分核心功能的参考，你可以在开发中随时查阅这些内容。
{% endhint %}

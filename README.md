# 入门

### 介绍

impack/wordpress（简称 imwp）封装了一些 WordPress 二次开发中可能用到通用功能。**目前这是本人自用的代码库，如果你使用此库则需要自己承担问题。**

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
| public | 静态资源目录 |

{% hint style="success" %}
本文档除了涵盖 imwp 的教程，还包含了 WordPress 部分核心功能的参考，你可以在开发中随时查阅这些内容。
{% endhint %}

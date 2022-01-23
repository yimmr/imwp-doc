# 简介

模型是参考Laravel ORM设计的类，能在WordPress中使用简洁、统一的方式操作数据库。每个数据表都有对应的模型来与之交互，每条记录对应一个模型实例，而实例属性是这条记录的数据。但在WordPress中，每个默认表都关联模型是多余的，因为你可以使用WP函数与其交互，所以衍生了一种为默认表设计的核心模型。

核心模型底层基本使用 WP 函数实现，因此即使只关联一个表，也能操作其他表的数据。比如 `posts` 模型，不仅能够读取 `wp_posts` 表，也能读取 `wp_postmeta` 表。

目前只有四个核心模型：**posts、users、comments、terms**。

options 表结构简单，你可以使用WP函数与之交互，或使用 [`Config`](../ji-ben-gong-neng/pei-zhi.md) 类。

{% hint style="danger" %}
目前版本只实现了核心模型，若创建其他表的模型会提示未支持。
{% endhint %}

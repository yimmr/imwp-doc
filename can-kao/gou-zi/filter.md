---
description: 表格内容是方法中主要发生的钩子
---

# 构建主查询

#### $wp->main()

方法用于设置WP环境即构建主查询，并设置HTTP响应头。

|  名称 |     说明    |   参数   |
| :-: | :-------: | :----: |
|  wp | 设置WP环境后触发 | \[$wp] |

#### $wp->parse\_request()

|       名称       |           说明           |           参数          |
| :------------: | :--------------------: | :-------------------: |
|   query\_vars  |        添加公共查询变量        |  public\_query\_vars  |
|     request    |        过滤查询变量数组        |      query\_vars      |
| parse\_request | 解析完查询变量后触发的 **Action** |         \[$wp]        |

{% hint style="success" %}
**public\_query\_vars** 保存变量名。从 GET/POST 取变量值并设置查询变量。可配合路由重写规则使用。
{% endhint %}

#### $wp->send\_headers()

|       名称      |       说明       |      参数      |
| :-----------: | :------------: | :----------: |
|  wp\_headers  | 将发给浏览器的HTTP响应头 | headers, $wp |
| send\_headers |    发送响应头后触发    |    \[$wp]    |

#### $wp->query\_posts()

调用 `WP_Query::query` 查询数据，参数值为解析后的查询变量。

#### $wp\_query->get\_posts()

这里查询数据同时还会设置页面类型和状态信息。

|         名称        |       说明       |          参数         |
| :---------------: | :------------: | :-----------------: |
|  pre\_get\_posts  | 已创建查询变量，但查询前触发 |    \[$wp\_query]    |
| posts\_pre\_query |   查询前过滤 posts  | \[null, $wp\_query] |

{% hint style="success" %}
$wp\_query->posts 为 null 时才查询数据库，可在查询前设置值以跳过数据库。
{% endhint %}

#### $wp->handle\_404()

|        名称        |    说明    |         参数        |
| :--------------: | :------: | :---------------: |
| pre\_handle\_404 | 是否不处理404 | false, $wp\_query |

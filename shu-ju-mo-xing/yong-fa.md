---
description: 所有模型须继承 Impack\WP\Database\Fluent\Model
---

# 基本用法

## 名称与前缀

模型支持自动处理表前缀，设置表名时，不需要写名称的前缀。当 `table` 属性为空时，底层自动将类名（不含命名空间）+ s 的格式做它关联的表名，与数据库交互时自动加上 WP 表前缀。

## 属性参考

|     名称     |            默认值            |   类型   |     说明     |
| :--------: | :-----------------------: | :----: | :--------: |
|    table   |            null           | string | 不带前缀的数据表名称 |
| primaryKey | ID\|term\_id\|comment\_ID | string |    数据表主键   |
|   keyType  |            int            | string |   主键数据类型   |
| attributes |            \[]            |  array |   数据库字段和值  |
|  original  |            \[]            |  array |  原始数据库字段和值 |
|   exists   |           false           |  bool  | 模型(数据)是否存在 |
|   changes  |            \[]            |  array |   存放改过的数据  |

## 查询规则

核心模型调用这些方法时，将用作 WP\_Query 的查询参数，并不用于创建SQL语句。此外，核心模型还支持更多的方法，[查看详请](https://imwp.yimmr.com/shu-ju-mo-xing/he-xin-mo-xing)。

|         方法名        |        说明       |
| :----------------: | :-------------: |
|        where       |    添加where子句    |
| whereIn/whereNotIn |        -        |
|        limit       | 限制查询数量，-1可查全部数据 |
|       offset       |     从第n条开始查     |
|       orderBy      |       排序规则      |

## 创建模型

模型存放的目录没有限制，但推荐在Models文件夹创建模型，下方示例在app/Models文件夹创建一个操作文章数据的模型

```php
<?php

namespace App\Models;

use Impack\WP\Database\Fluent\Model;

class Post extends Model
{
    // protected $table = 'posts'; // 设置关联的表名称
}
```

## 新增数据

方法一：使用一组数据创建实例，并调用 `save` 保存到数据库

```php
$data = [
    'post_title'   => '文章标题',
    'post_content' => '简单内容...',
    'post_status'  => 'publish',   
];

$post = new App\Models\Post($data);

$post->save();
```

方法二：调用 `create` 方法设置数据并保存到数据库，它将返回模型实例

```php
$post = App\Models\Post::create(['post_title' => '文章标题']);
```

通过属性赋值的方式设置模型数据：

```php
$post->post_title = '文章';
```

调用 `fill` 方法填充模型数据，此方法不会保存数据：

```php
$post->fill(['post_name' => 'name']);
```

## 读取数据

模型可以充当查询构建器，例如调用 `get` 和 `first` 方法分别得到所有结果和第一条记录， `get` 始终得到一个模型或指定列数据数组。

```php
// 查询十条数据
$posts = App\Models\Post::limit(10)->get();

// 查询最后一次发布的文章
$first = App\Models\Post::orderBy('date', 'desc')->first();
```

使用主键作为参数调用 `find` 方法，将会得到匹配的模型：

```php
$post = App\Models\Post::find(1);

$posts = App\Models\Post::find([1, 2, 3]);
```

如果你想查询所有数据，可以使用 `all` 方法，它和 `get` 一样始终返回一个数组：

```php
$posts = App\Models\Post::all();
```

调用 `count` 方法获取数据总数：

```php
$count = App\Models\Post::where('post_type', 'post')->count();
```

## 更新数据

模型实例 `save` 方法可以把修改的属性更新到数据库，数据不存在时，它将新增一条记录：

```php
$post = App\Models\Post::find(1);

$post->post_title = '新的文章标题';

$post->save();
```

调用 `update` 方法支持按条件批量更新模型：

```php
App\Models\Post::where('post_type', 'post')
->update(['post_title' => '新的标题', 'post_content' => '新的内容...']);
```

## 删除数据

调用 `delete` 方法可以删除数据，下方先查出一条数据然后删除它：

```php
App\Models\Post::find(1)->delete();
```

按匹配条件删除文章：

```php
App\Models\Post::whereIn('ID', [1, 2, 3])->delete();
```

另外静态方法 `destroy` 支持使用主键批量删除：

```php
App\Models\Post::destroy(1);

App\Models\Post::destroy(1, 2, 3);

App\Models\Post::destroy([1, 2, 3]);
```

## 回收站

{% hint style="danger" %}
此版本仅有 `post | user` 模型支持回收站
{% endhint %}

调用 `trash` 方法时，不会删除数据库记录，只将它移入回收站：

```php
App\Models\Post::where('ID', 1)->trash();
```

按条件批量移入回收站：

```php
App\Models\Post::whereIn('ID', [1, 2, 3])->trash();
```

`restore` 方法与 `trash` 相反，用于恢复回收站数据：

```php
App\Models\Post::where('ID', 1)->restore();
```

仅要回收站数据，可以先调用 `onlyTrashed` 再执行查询：

```php
App\Models\Post::onlyTrashed()->get();
```

## 增删改查返回值约定

|    方法   |   返回类型   |           说明           |
| :-----: | :------: | :--------------------: |
|   Get   |   array  |          返回数组          |
|  Insert |   bool   |       布尔值表示成功或失败       |
|  Update |    int   | 单条返回主键ID，失败为0；批量返回成功条数 |
|  Delete | bool,int |    单条返回布尔值；批量返回成功条数    |
|  Trash  | bool,int |    单条返回布尔值；批量返回成功条数    |
| Restore | bool,int |    单条返回布尔值；批量返回成功条数    |

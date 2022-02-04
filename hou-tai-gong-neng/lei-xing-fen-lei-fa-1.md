# 类型&分类法

建议始终使用配置文件组织注册类型、分类法、元框等功能的参数，集中管理各项参数值以及需要多语言翻译的文字。

### 批量注册

静态类 `Impack\WP\Post\Register` 提供的方法可以批量注册类型、分类法以及相关的功能，这些功能需要提供以下约定的配置：

* 注册类型和分类法的配置是一个键值对数组，键名是类型/分类法名称，值是注册参数数组；
* 提供额外的参数实现同时注册与之相关的功能

**注册类型的额外参数：**

|      名称     |   类型  |                                      说明                                      |
| :---------: | :---: | :--------------------------------------------------------------------------: |
|  taxonomies | array |                                    分类法参数数组                                   |
| meta\_boxes | array |                                    元框参数数组                                    |
|     meta    | array | meta\_key 与 register\_post\_meta [参数](../can-kao/meta-can-shu.md#meta)的键值对数组 |

余下的可参考 [WP的PostType参数](../can-kao/zhu-ce-de-can-shu.md#zhu-ce-can-shu)。

**注册分类法的额外参数：**

|   名称   |       类型      |                说明                |
| :----: | :-----------: | :------------------------------: |
| fields | string\|array | [分类法自定义字段类](broken-reference)的类名 |

余下的可参考 [WP的Taxonomy参数](../can-kao/taxonomy-can-shu.md#zhu-ce-can-shu)。

**meta\_boxes 参数：**

参数值是多个 `add_meta_box` [参数](../can-kao/meta-can-shu.md#metabox)数组，但这些参数和原来的有些不同：

* 第三个参数仅支持符合 [元框类](broken-reference) 的类名（主用面向对象方式开发，所以暂时忽略其他类型）。
* 跳过 `screen` 参数，此参数值默认为 `null` 。

如下示例将在编辑文章的界面添加三个元框：

```php
[
    'post' => [
        'meta_boxes'   => [
            ['test-box1', 'News元框标题1', App\Admin\MetaBoxes\TestBox::class, 'side'],
            ['test-box2', 'News元框标题2', App\Admin\MetaBoxes\TestBox::class, 'side'],
            ['test-box3', 'News元框标题3', App\Admin\MetaBoxes\TestBox::class, 'side'],
        ],
    ]
];
```

**taxonomies 参数：**

参数值是注册分类法的参数数组，如果把分类法参数移到其他配置文件，则此项值可以是分类法名称数组，此时调用批量注册的方法时需要提供注册这些分类法的参数。

### 元框类

每个元框类用于组织某种元框的代码，使用批量注册时类需要实现以下方法：

* **render**: 渲染元框
* **save**: 保存元框数据

### 分类法字段类

每个分类法字段类用于组织某种分类法字段的代码，使用批量注册时类需要以下方法：

* **addFields**: 添加新增术语的表单字段
* **editFields**: 添加编辑术语的表单字段
* **save**: 保存字段数据。此方法不是必需的，方法不存在时不加到钩子

{% hint style="success" %}
批量注册时将使用分类法名称做为参数构建字段类的实例，因此可以在构造函数中添加额外的钩子
{% endhint %}

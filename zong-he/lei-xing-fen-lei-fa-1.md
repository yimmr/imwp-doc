# 类型&分类法

建议始终使用配置文件组织注册类型、分类法、元框等功能的参数，集中管理各项参数值以及需要多语言翻译的文字。

### 批量注册

静态类 `Impack\WP\Support\PostRegister` 提供的方法可以批量注册类型、分类法以及相关的功能，这些功能需要提供以下约定的配置：

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

|   名称   |   类型  |                                      说明                                      |
| :----: | :---: | :--------------------------------------------------------------------------: |
| fields | array |         [分类法字段类](lei-xing-fen-lei-fa-1.md#fen-lei-fa-zi-duan-lei)的类名         |
|  meta  | array | meta\_key 与 register\_term\_meta [参数](../can-kao/meta-can-shu.md#meta)的键值对数组 |

余下的可参考 [WP的Taxonomy参数](../can-kao/taxonomy-can-shu.md#zhu-ce-can-shu)。

**meta\_boxes 参数：**

参数值是多个 `add_meta_box` [参数](../can-kao/meta-can-shu.md#metabox)数组，但这些参数和原来的有些不同：

* 第三个参数仅支持符合 [元框类](broken-reference) 的类名（主用面向对象方式开发，所以暂时忽略其他类型）。
* 参数 `screen` 的值固定为 `null` 。

如下示例将在编辑文章的界面添加三个元框：

```php
[
    'post' => [
        'meta_boxes'   => [
            ['test-box1', 'News元框标题1', App\Admin\MetaBoxes\TestBox::class, null, 'side'],
            ['test-box2', 'News元框标题2', App\Admin\MetaBoxes\TestBox::class, null, 'side'],
            ['test-box3', 'News元框标题3', App\Admin\MetaBoxes\TestBox::class, null, 'side'],
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
* **save**: 保存字段数据。此方法可选，未定义方法时不加到钩子

{% hint style="success" %}
批量注册时将使用分类法名称做为参数构建字段类的实例，因此可以在构造函数中添加额外的钩子
{% endhint %}

### 批量注册类的方法

表格中均是静态方法，每个方法尽可能地精简，只进行简单的循环，因此有些功能并不一定用到这些方法，直接写简单的PHP循环即可。

|         名称        |                           参数                           |       说明      |
| :---------------: | :----------------------------------------------------: | :-----------: |
|      posttype     | <p>array $posttypes, </p><p>array $taxonomies = []</p> | 注册类型及分类法等相关功能 |
|      taxonomy     |              $posttype, array $taxonomies              |   注册分类法和相关功能  |
|     metaBoxes     |               $posttype, array $metaBoxes              |  注册元框（仅在后端执行） |
|    addTaxField    |             $taxonomy, array $fields = \[]             |   分类法添加自定义字段  |
| addGlobalTaxField |           array $fields, array $exclude = \[]          |  全部分类法添加自定义字段 |

### 例子

首先创建一个 `posttype.php` 的配置文件，文件内容如下：

```php
<?php

return [
    'post' => [
        'taxonomies' => [
            'post_popular' => ['label' => '热度'],
        ],
    ],
    'news' => [
        'label'      => '新闻',
        'public'     => true,
        'taxonomies' => [
            'news_tag' => [
                'label'  => '新闻标签',
                'fields' => [\App\Admin\TaxFields\SeoField::class],
            ],
            'news_cat' => [
                'label'        => '新闻分类',
                'hierarchical' => true,
            ],
        ],
        'meta'       => [
            'test_key' => [
                'type'    => 'string',
                'default' => '-',
                'single'  => false,
            ],
        ],
        'meta_boxes' => [
            ['test-box1', 'News元框标题1', \App\Admin\MetaBoxes\TestBox::class, null, 'side'],
            ['test-box2', 'News元框标题2', \App\Admin\MetaBoxes\TestBox::class, null, 'side'],
            ['test-box3', 'News元框标题3', \App\Admin\MetaBoxes\TestBox::class, null, 'side'],
        ],
    ],
];
```

创建元框类：

```php
<?php

namespace App\Admin\MetaBoxes;

class TestBox
{
    public function render($post)
    {
        $count = (int) \get_post_meta($post->ID, '_save_post_count', true);
        echo '此文章保存了' . $count . '次';
        echo '<input type="hidden" name="_save_post_count" value="' . $count . '">';
    }

    public function save($postid)
    {
        if (isset($_POST['_save_post_count'])) {
            \update_post_meta($postid, '_save_post_count', $_POST['_save_post_count'] + 1);
        }
    }
}
```

创建字段类：

```php
<?php

namespace App\Admin\TaxFields;

class SeoField
{
    public function addFields()
    {
        echo '<div class="form-field">';
        echo '<label>SEO关键词</label>';
        echo '<input type="text" name="_seo_key">';
        echo '</div>';
    }

    public function editFields($term)
    {
        echo '<tr class="form-field">';
        echo '<th scope="row">SEO关键词</th>';
        echo '<td><input type="text" name="_seo_key" value="' . \get_term_meta($term->term_id, '_seo_key', true) . '"></td>';
        echo '</tr>';
    }

    public function save($termid)
    {
        \update_term_meta($termid, '_seo_key', trim($_POST['_seo_key']));
    }
}
```

最后在主类 `init()` 方法中进行批量注册，主类的这个方法将用作 `init` 钩子回调：

```php
public function init()
{
    // 这里使用 \Impack\WP\Base\Config 类的实例读取配置文件
    \Impack\WP\Support\PostRegister::posttype($this->config->get('posttype'));
}
```

该示例将增加的功能：

* 一个新闻类型和关联的两个分类法：新闻标签、新闻分类
* 新闻标签的 SEO字段
* 编辑新闻时出现三个统计保存次数的元框
* 文章的热度分类法

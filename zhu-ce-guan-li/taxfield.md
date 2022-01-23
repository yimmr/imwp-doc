# TaxField

分类法自定义字段（TaxField）和元框类似，也是一个微型单元。

#### 创建字段

分类法字段类需要实现抽象类 `Impack\WP\Register\TaxField` 的 `addfield` 、`editField`、`save` 方法。例如，创建一个设置SEO关键词的字段：

```php
<?php
namespace App\Admin\TaxFields;

use Impack\WP\Post\TaxField;

class SeoField extends TaxField
{
    public function addField()
    {
        echo '<div class="form-field">';
        echo '<label>SEO关键词</label>';
        echo '<input type="text" name="_seo_key">';
        echo '</div>';
    }

    public function editField($term)
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

#### 添加字段

不需注册钩子，直接调用 `add` 方法给指定分类法添加字段：

```php
App\Admin\TaxFields\SeoField:add('post_tag');
```

#### 保存数据

WP自动调用保存数据的方法。但可以手动调用，用法是创建实例调用 `save` 方法并传入 `term_id` ：

```php
(new \App\Admin\TaxFields\SeoField)->save(1);
```

#### 自动化

参考 [类型&分类法](lei-xing-fen-lei-fa-1.md) 实现自动注册分类法的自定义字段。

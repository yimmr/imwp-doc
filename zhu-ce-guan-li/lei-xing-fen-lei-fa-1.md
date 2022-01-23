# 类型&分类法

将类型、分类法、元框等功能的注册参数关联起来，即可实现自动注册，后续维护代码也更轻松。

### 配置

类型配置是类型名称与选项数组对应的键值对数组，除了 `register_post_type` 函数的选项外，还支持相关功能的选项；分类法配置和类型遵循相同的规则。选项很多时可以放到单独文件中分开管理，例如 `posttype.php` 和 `taxonomy.php` 两文件返回各自的配置。

**posttype增加的配置项：**

|      名称     |   类型  |                                      说明                                     |
| :---------: | :---: | :-------------------------------------------------------------------------: |
|  taxonomies | array |                                   分类法名称数组                                   |
| meta\_boxes | array |                                     元框数组                                    |
|     meta    | array | meta\_key与 register\_post\_meta [参数](../can-kao/meta-can-shu.md#meta)的键值对数组 |

其他项可参考 \*\*\*\* [WP的PostType参数](../can-kao/zhu-ce-de-can-shu.md#zhu-ce-can-shu)。

**meta\_boxes** 选项是多个元框配置数组，元框配置数组首个值是 [元框类](metabox.md) 类名，剩下为传给类 `add` 方法的参数，但这里跳过 `screen` 参数。如给类型添加三个不同标题的元框：

```php
'meta_boxes'   => [
    [App\Admin\MetaBoxes\TestBox::class, 'test-box1', 'News元框标题1', 'side'],
    [App\Admin\MetaBoxes\TestBox::class, 'test-box2', 'News元框标题2', 'side'],
    [App\Admin\MetaBoxes\TestBox::class, 'test-box3', 'News元框标题3', 'side'],
],
```

**taxonomy增加的配置项：**

|   名称   |       类型      |              说明             |
| :----: | :-----------: | :-------------------------: |
| fields | string\|array | [分类法自定义字段类](taxfield.md)的类名 |

其他项可参考 \*\*\*\* [WP的Taxonomy参数](../can-kao/taxonomy-can-shu.md#zhu-ce-can-shu)。

### 自动注册

静态类 `Impack\WP\Post\Register` 用于自动注册前台或后台的所有功能。

调用 `posttype` 批量注册类型和分类法：

```php
\Impack\WP\Post\Register::posttype(['posttype' => []], ['taxonomy' => []]);
```

调用 `taxonomy` 批量注册指定类型的分类法：

```php
\Impack\WP\Post\Register::taxonomy(['taxonomy' => []], 'post');
```

调用 `addGlobalTaxField` 给所有分类法添加自定义字段，第一个参数是 `TaxField` 组件的类名，第二个参数排除的分类法名称数组：

```php
\Impack\WP\Post\Register::addGlobalTaxField(SeoField::class);
```

调用 `addTaxField` 给指定分类法添加自定义字段：

```php
\Impack\WP\Post\Register::addTaxField('post_tag', SeoField::class);
```

注册后可以通过类型对象获取新增的分类法：

```php
get_post_type_object('post')->taxonomies;
```

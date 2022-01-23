# MetaBox

元框（meta box）可以使用类创建，一个元框类相当于一个微型单元，可以创建多个不同属性的元框实例，每个实例管理自身视图和数据。

#### 创建元框

元框类需要实现抽象类 `Impack\WP\Support\Metabox` 的 `render` 、`save` 方法，例如创建记录文章保存次数的元框：

```php
<?php

namespace App\Admin\MetaBoxes;

use Impack\WP\Post\MetaBox;

class TestBox extends MetaBox
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

#### 添加元框

元框类可以创建多个不同属性的元框。调用静态方法 `add` 可以添加元框，方法和WP添加元框的函数类似，需要在指定钩子中执行，并且参数基本一致，但跳过输出元框的回调函数，用法如下：

```php
add_action('add_meta_boxes_post', function(){
    \App\Admin\MetaBoxes\TestBox:add('test-box1', '元框标题1');
    \App\Admin\MetaBoxes\TestBox::add('test-box2', '元框标题2', null, 'side');
});
```

[元框参数参考>>](../can-kao/meta-can-shu.md#metabox)

#### 保存数据

元框可以有不同的视图属性，但核心数据一致，保存数据的逻辑一致，直接创建实例并调用 `save` 方法即可：

```php
(new \App\Admin\MetaBoxes\TestBox)->save();
```

#### 自动化

参考 [类型&分类法](lei-xing-fen-lei-fa-1.md) 实现自动注册类型的元框。


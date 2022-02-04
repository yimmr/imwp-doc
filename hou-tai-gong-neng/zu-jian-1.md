---
description: 此组件是一个静态类，用于输出指定HTML
---

# 表单组件

### 普通表单组件

组件完整类名： `Impack\WP\Components\Form`&#x20;

### 方法

|    名称    |      说明      |
| :------: | :----------: |
|  enqueue | 页内加载所需的脚本和样式 |
| textarea |     多行文本域    |
|   input  |      文本框     |
| checkbox |      复选框     |
|   radio  |      单选框     |
|  select  |     下拉选择     |
|   image  |     图片上传     |

每个方法的首个必需参数是字段 `name` ，最后的可选参数是标签属性数组 `attrs` ， 首个参数可以是一个合并其他参数和 `attrs` 的数组，例如下方两种传参效果一致：

```php
Form::image('image_id', [1], '100-16:9', 3, ['class' => 'custom-image']);

Form::image([
    'name' => 'image_id',
    'value' => [1],
    'size' => '100-16:9',
    'count' => 3,
    'class' => 'custom-image'
]);
```

**图片上传组件**

默认支持单张上传，如果提供 `$count` 参数则支持上传多张图片，同时 `$value` 参数必需是数组。

参数 `$size` 可设置图片大小（单位 px），仅提供数值不带单位，支持以下几种值：

* 仅提供宽度：大小宽高一致
* 横线分隔宽-高：大小与提供的宽高一致
* 宽-宽高比：宽为提供的宽度，再使用宽高比计算出高度。如 `100-16:9` ，宽100px，高56.25px

使用图片上传需要引入脚本和样式：

```php
\add_action('admin_enqueue_scripts', [\Impack\WP\Components\Form::class, 'enqueue']);
```

**下拉组件**

参数 `$options` 是选项数组，每项数组值可以是字符串或者 `[value, label]` 数组：

```php
Form::select('fruit', '苹果',[
    '苹果', // 组件内部将自动转为 ['苹果', '苹果']
    ['橘子', '橘子']
    ['火龙果', '火龙果']
]);
```

### 分类法表单组件

类名 `Impack\WP\Components\TaxFrom`

|     名称    |     说明     |
| :-------: | :--------: |
|  addField | 输出新建标签时的字段 |
| editField | 输出编辑标签时的字段 |

####

# 表单组件

组件一般是构建HTML的静态类。

### 表单

类名 `Impack\WP\Components\Form` ，每个组件的根元素都支持自定义 attrs 属性。每个组件都只返回字符串，不直接输出。

|       名称      |      说明     |
| :-----------: | :---------: |
|     input     |     文本框     |
|    textarea   |    多行文本域    |
|     select    |     下拉选择    |
|    checkbox   |     复选框     |
| checkboxGroup |    多个复选框    |
|     radio     |     单选框     |
|   radioGroup  |    多个单选框    |
|     number    | Number类型文本框 |
|     image     |     图片上传    |
|   imageGroup  |     多图上传    |

图片组默认限制上传3张，配置 `count` 属性可以覆盖该值。[表单配置参考](broken-reference)

{% hint style="success" %}
**image** 组件基于WP的图片上传，使用时需要调用静态方法 `enqueue` 入队脚本样式。使用多图上传需要过滤表单提交的值，因为没有全部上传的id数组会带0值
{% endhint %}

### 分类法表单字段

类名 `Impack\WP\Components\TaxFrom`

|     名称    |      说明     |
| :-------: | :---------: |
|  addField | 输出新建标签的字段组件 |
| editField | 输出编辑标签的字段组件 |

### 配置项

类名 `Impack\WP\Components\Option` ，选项是表格 + 表单结构的WP后台配置项，基本直接输出html，不返回字符串。

|          名称         |            说明            |
| :-----------------: | :----------------------: |
|         main        |    按照数据完整输出WP后台选项风格的视图   |
|      wrapTable      | 只输出标题和表格，可在回调手动输出 **tr** |
|          tr         |      输出Option表格tr组件      |
|       trGroup       |        按照数据输出所有字段        |
|   trGroupChildren   | 输出表单配置 `children` 项的多个字段 |
| repeatGroupChildren |         重复拼接一组子字段        |

#### **main** 组件使用示例：

```php
public function render(){
    $theme = $this->app['themeOption'];
    Option::main($this->title, $theme->getFields(), $theme->getAll());
}
```

#### trGroup钩子

|              名称              |     说明    |                   参数                  |
| :--------------------------: | :-------: | :-----------------------------------: |
| imwp\_trgroup\_{name}\_field | 自定义输出表单字段 | $str, $params\[$field, $name, $value] |

{% hint style="success" %}
钩子的返回值是null时，不显示该字段
{% endhint %}

#### main和wrapTable支持的钩子

|                  名称                 |        说明       |
| :---------------------------------: | :-------------: |
| imwp\_option\_form\_\[before/after] | 两个钩子分别在form前后触发 |
|         imwp\_option\_table         |     输出表格后触发     |
|         imwp\_option\_submit        |     提交按钮后触发     |

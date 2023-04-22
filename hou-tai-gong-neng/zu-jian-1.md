---
description: 此组件是一个静态类，用于输出指定HTML
---

# 表单组件

### 普通表单组件

组件完整类名： `Impack\WP\Components\Form`&#x20;

基础组件支持PHP/JS实现，其他组件需要引入前端资产文件后使用。

### 字段方法

|        基础组件       |        额外属性       |         说明        |
| :---------------: | :---------------: | :---------------: |
|       input       |         -         |       通用输入控件      |
|        text       |         -         |       单行文本类型      |
|      textarea     |     rows, cols    |       多行文本框       |
|       select      | options, multiple |        下拉选择       |
|    checkControl   |       value       | checkbox/radio 控件 |
| checkControlGroup | options, multiple |       一组选择控件      |
|      checkbox     |         -         |         复选        |
|       radio       |         -         |         单选        |

#### 通用属性

* name ：字段名
* type ：类型
* label ：可读的名称
* default/checked ：指定默认值
* fields : 子级字段数组，存在此项时只渲染子字段，父级只是字段组的标题，不含字段
* description ：字段描述，支持HTML
* title ：表单表格的行标题，没有时使用 `label` 属性。一般不需要，为避免有的字段渲染 `label` 和标题重复时使用。

若有其他未列出的属性，将用做HTML表单元素或字段根元素的属性，或做为特定字段组件的参数。比如用 `'style' => ['objectFit' => 'cover']` 属性定义图片的样式。

{% hint style="info" %}
单个 checkbox 类型的字段支持一个 `value` 属性，用于 `input` 的值
{% endhint %}

#### 多选/下拉属性

* options ：一个 `label,value` 关联数组的数组
* multiple ：是否多选

### JS扩展字段

#### 媒体上传 image/video/audio

* placeholder ：替代默认上传图标的占位符
* ratio : 图片/视频显示的宽高比。基于设定的宽和高中最大那个值计算
* width/height : 媒体显示的大小。默认高100px，宽自由

### 分类法表单组件

类名 `Impack\WP\Components\TaxFrom`

|     名称    |     说明     |
| :-------: | :--------: |
|  addField | 输出新建标签时的字段 |
| editField | 输出编辑标签时的字段 |

####

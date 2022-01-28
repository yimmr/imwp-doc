# 表单配置

这里提供了表单字段的配置项的一些约定，以便结合 [内置表单](../hou-tai-gong-neng/zu-jian-1.md#main-zu-jian-shi-yong-shi-li) 、vue 等各种组件使用。

表单配置是字段名与属性关联的键值对数组，字段的属性如下：

|    选项    |   类型   |           说明          |
| :------: | :----: | :-------------------: |
|   label  | string |        字段的描述性文字       |
|   value  |  mixed |          默认值          |
|   type   | string |          字段类型         |
| children |  array |         子级字段数组        |
|  options |  array |          选项数组         |
|   attrs  |  array |        额外字段属性数组       |
|   size   | string | 仅用于图片字段，值是 `宽\|宽:高占比` |

{% hint style="success" %}
存在 `children` 项时，默认值将是一个数组（遍历子级获取每个值）；`options` 是字符串数组或一组数组 `['label'='','value'=>'']` 。
{% endhint %}

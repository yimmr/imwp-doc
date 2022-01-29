# Settings

### register\_setting

注册选项，将一些选项归为一组，只允许更新组内选项。

* `option_group {string}` 选项组组名。内置组： 'general', 'discussion', 'media', 'reading', 'writing', 'misc', 'options', and 'privacy'。
* `option_name {string}` 选项键名
* `[args] {array} []` 关于选项的参数：

|         名称         |      类型     |                                           描述                                          |   默认   |
| :----------------: | :---------: | :-----------------------------------------------------------------------------------: | :----: |
|        type        |    string   | 数据类型。Valid values are 'string', 'boolean', 'integer', 'number', 'array', and 'object' | string |
|     description    |    string   |                                           描述                                          |   ''   |
| sanitize\_callback |   callable  |                                       清理选项值的回调函数                                      |  null  |
|   show\_in\_rest   | bool\|array |                        是否出现在 REST API 中。参数还可以是 'schema' key 数组。                       |  false |
|       default      |    mixed    |                                          默认值                                          |        |

{% hint style="success" %}
一般只用到 `$args` 的 `sanitize_callback` 和 `default` 参数，其他的参数仅在 `REST API` 使用。
{% endhint %}

### add\_settings\_section

|    名称    |    类型    |       描述      |  默认 |
| :------: | :------: | :-----------: | :-: |
|    id    |  string  |     HTML标识    |     |
|  titile  |  string  |      标题述      |     |
| callback | callable |   标题和字段间输出内容  |     |
|   page   |  string  | 加到设置页面的 slug  |     |

### add\_settings\_field

|    名称    |    类型    |                                                                                描述                                                                                |    默认   |
| :------: | :------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-----: |
|    id    |  string  |                                                                              HTML标识                                                                              |         |
|  titile  |  string  |                                                                                标题述                                                                               |         |
| callback | callable |                                                                            标题和字段间输出内容                                                                            |         |
|   page   |  string  |                                                                           加到设置页面的 slug                                                                           |         |
|  section |  string  |                                                                           与 section 关联                                                                           | default |
|   args   |   array  | <p></p><p><strong>'label_for'</strong> <em>(string)</em> 提供时标题将外包 &#x3C;label> 元素。</p><p><strong>'class'</strong> <em>(string)</em> 添加到 &#x3C;tr> 元素的 CSS 类。</p> |   \[]   |

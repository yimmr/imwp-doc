# 钩子

除了模板钩子，前后台基本都会触发这些钩子

|             名称             |      说明      |     参数    |
| :------------------------: | :----------: | :-------: |
|       plugin\_loaded       | 每加载完一个插件执行一次 |  插件入口文件路径 |
|       plugins\_loaded      |    所有插件加载后   |     -     |
| sanitize\_comment\_cookies |  用于清理cookie  |     -     |
|        setup\_theme        |     加载主题前    |     -     |
|     after\_setup\_theme    |     加载主题后    |     -     |
|            init            |     初始化后     |     -     |
|         wp\_loaded         |  主题插件和核心加载完毕 |     -     |
|     template\_redirect     |    加载模板前触发   |     -     |
|      template\_include     |  过滤将引入的模板文件  | $template |

{% hint style="success" %}
每种页面都有对应的函数读取模板文件路径，没有找到模板文件时，默认会读取首页模板
{% endhint %}

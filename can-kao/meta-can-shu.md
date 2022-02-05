# Meta

## Meta

|         名称         |     类型     |   默认值  |       说明      |
| :----------------: | :--------: | :----: | :-----------: |
|   object\_subtype  |   string   |   ''   |   子类型，如post   |
|        type        |    strin   | string |      数据类型     |
|     description    |   string   |   ''   |      简短描述     |
|       default      |    mixed   |   ''   |      默认值      |
|       single       |    bool    |  false | meta键是单值还是一组值 |
| sanitize\_callback |  callable  |  null  |    过滤数据的回调    |
|   auth\_callback   |  callable  |  null  |    检查权限的回调    |
|   show\_in\_rest   | bool,array |  false |  是否支持REST API |

## MetaBox

|       名称       |            类型           |    默认值   |                       说明                       |
| :------------: | :---------------------: | :------: | :--------------------------------------------: |
|       id       |          string         |     -    |                    HTML的id属性                   |
|      title     |          string         |     -    |                       标题                       |
|    callback    |         callable        |     -    |                     元框显示的内容                    |
|     screen     | string,array,WP\_Screen |   null   |  <p>显示的位置，</p><p>post type, link, comment</p>  |
|     context    |          string         | advanced | <p>决定是否显示于边栏，</p><p>normal, side, advanced</p> |
|    priority    |          string         |  default |  <p>显示的优先级，</p><p>high, core, default, low</p> |
| callback\_args |          array          |   null   |                 此值传给渲染回调的第二个参数                 |

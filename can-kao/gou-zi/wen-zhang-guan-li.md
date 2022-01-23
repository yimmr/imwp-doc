# 文章管理

### 过滤钩子

|                     名称                     |      说明      |                参数               |
| :----------------------------------------: | :----------: | :-----------------------------: |
|             views\_{screen\_id}            | 过滤表格上方的tab列表 |              views              |
|    manage\_{post\_type}\_posts\_columns    |   过滤表格每列表头   |             $columns            |
| manage\_{$posttype}\_posts\_custom\_column |  返回文章列表每列的数据 |         $columns,$postid        |
|          default\_hidden\_columns          |   过滤默认隐藏的列   |         $hidden, $screen        |
|   manage\_{screen\_id}\_sortable\_columns  |   过滤支持排序的列   |             $columns            |
|        post\_column\_taxonomy\_links       |   过滤列的分类法链接  | $term\_links, $taxonomy, $terms |
|             page\_row\_actions             |   过滤页面行操作列表  |         $actions, $post         |
|             post\_row\_actions             |   过滤文章行操作列表  |         $actions, $post         |
|            display\_post\_states           |     文章状态     |          $status, $post         |

### 动作钩子

|                      名称                     |      说明     |          参数          |
| :-----------------------------------------: | :---------: | :------------------: |
|        manage\_pages\_custom\_column        |  页面自定义列的内容  | $columnName, $postID |
|        manage\_posts\_custom\_column        |   文章自定义列内容  | $columnName, $postID |
| manage\_{post\_type}\_posts\_custom\_column | 自定类型的自定义列内容 | $columnName, $postID |

### 编辑文章

|           名称           |     说明     |  参数 |
| :--------------------: | :--------: | :-: |
| post\_submitbox\_start | 文章保存按钮旁边执行 |     |

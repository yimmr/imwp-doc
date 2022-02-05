# Post Type

## 注册参数

|            名称           |       类型      |                                                      默认值                                                      |                                          备注                                         |
| :---------------------: | :-----------: | :-----------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------: |
|          label          |     string    |                                                $labels\['name']                                               |                                        后台菜单名称                                       |
|          labels         |     array     |                                                       -                                                       |                      详见 [#labels](zhu-ce-de-can-shu.md#labels)                      |
|       description       |     string    |                                                       ''                                                      |                                         简短描述                                        |
|       hierarchical      |      bool     |                                                     false                                                     |                                       是否支持父子级                                       |
|          public         |      bool     |                                                     false                                                     |                                       是否显示于后台                                       |
|   publicly\_queryable   |      bool     |                                                    $public                                                    |                                        是否支持查询                                       |
|   show\_in\_nav\_menus  |      bool     |                                                    $public                                                    |                                     设置菜单时可选择此类型                                     |
|  exclude\_from\_search  |      bool     |                                                    !$public                                                   |                                      从前端搜索结果中排除                                     |
|         show\_ui        |      bool     |                                                    $public                                                    |                                       是否显示管理界面                                      |
|      show\_in\_menu     |  bool\|string |                                                   $show\_ui                                                   |                                       是否显示管理菜单                                      |
|   show\_in\_admin\_bar  |      bool     |                                                $show\_in\_menu                                                |                                       显示于快捷管理栏                                      |
|      show\_in\_rest     |      bool     |                                                     false                                                     |                                REST API支持，true开启块编辑器                                |
|        rest\_base       |     string    |                                                  $post\_type                                                  |                                   REST API路由基础URL                                   |
| rest\_controller\_class |     string    | [WP\_REST\_Posts\_Controller](https://developer.wordpress.org/reference/classes/wp\_rest\_posts\_controller/) |                                    REST API控制器类名                                    |
|      menu\_position     |      int      |                                                      null                                                     |                                        后台菜单位置                                       |
|        menu\_icon       |     string    |                                              dashicons-admin-post                                             | 菜单图标URL/base64/[dashicons](https://developer.wordpress.org/resource/dashicons)/none |
|     capability\_type    |     string    |                                                      post                                                     |                                      创建权限的基础名称                                      |
|       capabilities      |     array     |                                                       -                                                       |                       <p>操作权限</p><p>默认用$capability_type生成</p>                       |
|      map\_meta\_cap     |      bool     |                                                     false                                                     |                                          -                                          |
|         supports        |     array     |                                              \['title', 'editor']                                             |                        [支持的功能](zhu-ce-de-can-shu.md#supports)                       |
| register\_meta\_box\_cb |    callable   |                                                      null                                                     |                                       注册元框的回调                                       |
|        taxonomies       |     array     |                                                      \[]                                                      |                                       分类法名称数组                                       |
|       has\_archive      |  bool\|string |                                                     false                                                     |                                       是否有归档页面                                       |
|         rewrite         |  bool\|array  |                                                       -                                                       |                   路由重写规则 [#rewrite](zhu-ce-de-can-shu.md#rewrite)                   |
|        query\_var       |  bool\|string |                                                  $post\_type                                                  |                                   帖子类型的query\_var键                                  |
|       can\_export       |      bool     |                                                      true                                                     |                                        是否允许导出                                       |
|    delete\_with\_user   |      bool     |                                                      null                                                     |                                     删除用户时删除此类型帖子                                    |
|         template        |     array     |                                                      \[]                                                      |                                       编辑器默认块数组                                      |
|      template\_lock     | string\|false |                                                     false                                                     |                                       是否锁定块模板                                       |
|        \_builtin        |      bool     |                                                     false                                                     |                                    （仅内部使用）是否是内置类型                                   |
|       \_edit\_link      |     string    |                                                post.php?post=%d                                               |                                        仅内部使用                                        |

### 示例

隐藏类型，不支持链接访问，但可手动查询和管理

```php
[
     'public'            => false,
     'show_ui'           => true,
     'show_in_admin_bar' => false,
];
```

## Labels

|             名称             |     备注    |
| :------------------------: | :-------: |
|            name            |     文章    |
|       singular\_name       |     文章    |
|          add\_new          |    写文章    |
|       add\_new\_item       |   撰写新文章   |
|         edit\_item         |    编辑文章   |
|          new\_item         |    写文章    |
|         view\_item         |    查看文章   |
|         view\_items        |    查看文章   |
|        search\_items       |    搜索文章   |
|         not\_found         |   未找到文章。  |
|    not\_found\_in\_trash   | 回收站中没有文章。 |
|   **parent\_item\_colon**  |    父页面：   |
|         all\_items         |    所有文章   |
|          archives          |    文章存档   |
|         attributes         |    页面属性   |
|     insert\_into\_item     |   插入至文章   |
|  uploaded\_to\_this\_item  |  上传到本文章的  |
|       featured\_image      |    特色图像   |
|    set\_featured\_image    |   设置特色图像  |
|   remove\_featured\_image  |   取消特色图像  |
|    use\_featured\_image    |  设置为特色图像  |
|     filter\_items\_list    |   过滤文章列表  |
|   items\_list\_navigation  |   文章列表导航  |
|         items\_list        |    文章列表   |
|       item\_published      |   文章已发布。  |
| item\_published\_privately |  文章已私密发布。 |
|  item\_reverted\_to\_draft | 文章已恢复为草稿。 |
|       item\_scheduled      |   文章已计划。  |
|        item\_updated       |   文章已更新。  |
|         menu\_name         |     文章    |
|      name\_admin\_bar      |     文章    |

## Supports

|        功能       |   备注  |
| :-------------: | :---: |
|      title      |   标题  |
|      editor     |  编辑器  |
|     comments    |   评论  |
|    revisions    |  历史版本 |
|    trackbacks   |   引用  |
|      author     |  修改作者 |
|     excerpt     |   摘要  |
| page-attributes |  页面属性 |
|    thumbnail    |  缩略图  |
|  custom-fields  | 自定义字段 |
|   post-formats  |  文章格式 |

## Rewrite

|      名称     |   类型   |      默认值      |             说明             |
| :---------: | :----: | :-----------: | :------------------------: |
|     slug    | string |  $post\_type  |          固定连接的slug         |
| with\_front |  bool  |      true     | 固定连接以WP\_Rewrite::$front开头 |
|    feeds    |  bool  | $has\_archive |         feed固定连接支持         |
|    pages    |  bool  |      true     |           是否提供分页           |
|   ep\_mask  |  const | EP\_PERMALINK |           分配的端点掩码          |

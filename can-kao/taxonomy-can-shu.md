# Taxonomy

## 注册参数

|            名称           |       类型      |                                                      默认值                                                      |                        备注                       |
| :---------------------: | :-----------: | :-----------------------------------------------------------------------------------------------------------: | :---------------------------------------------: |
|          label          |     string    |                                                       标签                                                      |                       菜单名称                      |
|          labels         |     object    |                                                       -                                                       |     详见 [#labels](taxonomy-can-shu.md#labels)    |
|       description       |     string    |                                                       -                                                       |                       简短描述                      |
|       hierarchical      |      bool     |                                                     false                                                     |                     是否支持父子级                     |
|          public         |      bool     |                                                      true                                                     |                      是否后台显示                     |
|   publicly\_queryable   |      bool     |                                                    $public                                                    |                     是否支持公共查询                    |
|   show\_in\_nav\_menus  |      bool     |                                                    $public                                                    |                     设置菜单时可选择                    |
|         show\_ui        |      bool     |                                                    $public                                                    |                     是否显示管理界面                    |
|      show\_in\_menu     |      bool     |                                                   $show\_ui                                                   |                     是否显示管理菜单                    |
|      show\_tagcloud     |      bool     |                                                   $show\_ui                                                   |                    显示于标签云小工具                    |
|  show\_in\_quick\_edit  |      bool     |                                                   $show\_ui                                                   |                     在快速编辑中显示                    |
|   show\_admin\_column   |      bool     |                                                     false                                                     |                   显示于Post管理列表                   |
| rest\_controller\_class |     string    | [WP\_REST\_Terms\_Controller](https://developer.wordpress.org/reference/classes/wp\_rest\_terms\_controller/) |                  REST API控制器类名                  |
|      meta\_box\_cb      | bool,callable |                                                      null                                                     |                   输出Post元框的回调                   |
| meta\_box\_sanitize\_cb |    callable   |                                                      null                                                     |                    过滤元框数据的回调                    |
|       capabilities      |     array     |                                                       -                                                       |                       权限数组                      |
|         rewrite         |   bool,array  |                                                       -                                                       | 路由重写规则 [#rewrite](zhu-ce-de-can-shu.md#rewrite) |
|        query\_var       |  string,bool  |                                                      tag                                                      |                    分类法查询的var键                   |
| update\_count\_callback |    callable   |                                                      null                                                     |                    更新计数时执行的钩子                   |
|      default\_term      | sttring,array |                                                       -                                                       |     [默认项](taxonomy-can-shu.md#default\_item)    |
|        \_builtin        |    boolean    |                                                     false                                                     |                      仅内部使用                      |

### 示例

隐藏类型，不支持链接访问，但可手动查询和管理

```php
[
    'public'            => false,
    'show_ui'           => true,
    'show_tagcloud'     => false,
];
```

## Labels

加粗表示仅标签有默认值

|                 名称                |        备注       |
| :-------------------------------: | :-------------: |
|                name               |       分类目录      |
|           singular\_name          |       分类目录      |
|           search\_items           |      搜索分类目录     |
|         **popular\_items**        |       热门标签      |
|             all\_items            |      所有分类目录     |
|            parent\_item           |      父级分类目录     |
|        parent\_item\_colon        |     父级分类目录：     |
|             edit\_item            |      编辑分类目录     |
|             view\_item            |      查看分类目录     |
|            update\_item           |      更新分类目录     |
|           add\_new\_item          |     添加新分类目录     |
|          new\_item\_name          |      新分类目录名     |
| **separate\_items\_with\_commas** | 多个标签请用英文逗号（,）分开 |
|     **add\_or\_remove\_items**    |     添加或删除标签     |
|    **choose\_from\_most\_used**   |     从常用标签中选择    |
|             not\_found            |      未找到分类。     |
|             no\_terms             |      没有分类目录     |
|      items\_list\_navigation      |      分类列表导航     |
|            items\_list            |       分类列表      |
|             most\_used            |       最多使用      |
|          back\_to\_items          |      ← 返回分类     |
|             menu\_name            |       分类目录      |
|          name\_admin\_bar         |     category    |
|              archives             |      所有分类目录     |

## Rewrite

|      名称      |   类型   |    默认值    |             说明             |
| :----------: | :----: | :-------: | :------------------------: |
|     slug     | string | $taxonomy |          固定连接的slug         |
|  with\_front |  bool  |    true   | 固定连接以WP\_Rewrite::$front开头 |
| hierarchical |  bool  |   false   |           是否分父子级           |
|   ep\_mask   |  const |  EP\_NONE |           分配的端点掩码          |

## Default\_item

|        名称       |   备注  |
| :-------------: | :---: |
|     **name**    | 标题/名称 |
|     **slug**    |   -   |
| **description** |   描述  |

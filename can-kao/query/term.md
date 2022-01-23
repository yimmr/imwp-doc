# term



|             名称            |       类型      |  默认值  |                                                                    备注                                                                    |
| :-----------------------: | :-----------: | :---: | :--------------------------------------------------------------------------------------------------------------------------------------: |
|          taxonomy         | string\|array |       |                                                                    分类法                                                                   |
|     term\_taxonomy\_id    | string\|array |       |                                                                  术语分类法ID                                                                 |
|        object\_ids        |   int\|array  |       |                                                                   文章ID                                                                   |
|        hide\_empty        |      bool     |  true |                                                               是否排除未分配给帖子的术语                                                              |
|           parent          |               |       |                                                                   父项ID                                                                   |
|         child\_of         |      int      |   0   |                                                                  单个子术语ID                                                                 |
|        hierarchical       |      bool     |  true |                                                                  包括术语子项                                                                  |
|         childless         |      bool     | false |                                                                 结果限制为没有子项                                                                |
|        pad\_counts        |      bool     | false |                                                                是否填充术语子项数量                                                                |
|            get            |               |       |                                                          all/null.是否返回术语而不管是否有父项                                                         |
|          orderby          |               |       | <p>name、slug、term_group、term_id、id、description、parent、term_order、</p><p> count(分类法计数)、include(依据include顺序)、slug__in、none(忽略ORDER BY)</p> |
|           order           |     string    |  ASC  |                                                                 ASC/DESC                                                                 |
|          include          | string\|array |       |                                                                要包含的术语ID的数组                                                               |
|          exclude          |               |       |                                                                                                                                          |
|       exclude\_tree       | string\|array |       |                                                                排除指定ID的子术语                                                                |
|           number          |               |       |                                                                 限制数量。0全部                                                                 |
|           offset          |               |       |                                                                                                                                          |
|           fields          |               |  all  |                                      all ids tt\_ids names slugs count id=>parent id=>name id=>slug                                      |
|           count           |      bool     | false |                                                                 是否返回术语计数                                                                 |
|           search          |               |       |                                                                                                                                          |
|            name           | string\|array |       |                                                                   术语名称                                                                   |
|            slug           | string\|array |       |                                                                  术语slug                                                                  |
|        name\_\_like       |               |       |                                                                                                                                          |
|    description\_\_like    |               |       |                                                                                                                                          |
|       cache\_domain       |     string    |  core |                                                                  对象缓存的键                                                                  |
| update\_term\_meta\_cache |      bool     |  true |                                                                  是否缓存查询                                                                  |
|        meta\_query        |               |       |                                                                                                                                          |
|         meta\_key         |               |       |                                                                                                                                          |
|        meta\_value        |               |       |                                                                                                                                          |
|         meta\_type        |               |       |                                                                                                                                          |
|       meta\_compare       |               |       |                                                                                                                                          |


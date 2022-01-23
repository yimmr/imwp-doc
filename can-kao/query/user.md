# user

|           名称          |       类型      | 默认值 |                                                                    备注                                                                   |
| :-------------------: | :-----------: | :-: | :-------------------------------------------------------------------------------------------------------------------------------------: |
|        blog\_id       |               |     |                                                                   站点ID                                                                  |
|          role         | string\|array |     |                                                                用户必须匹配某些角色                                                               |
|       role\_\_in      |     array     |     |                                                                找是某些角色的用户                                                                |
|    role\_\_not\_in    |     array     |     |                                                                   排除角色                                                                  |
|       meta\_key       |               |     |                                                                                                                                         |
|      meta\_value      |               |     |                                                                                                                                         |
|     meta\_compare     |     string    |     |                                                        REGEXP、NOT REGEXP、RLIKE...                                                       |
|        include        |     array     |     |                                                                 要包含的用户ID                                                                |
|        exclude        |     array     |     |                                                                 要排除的用户ID                                                                |
|         search        |               |     |                                                    搜索数据列。search\_columns为空时根据字符确定搜索的列                                                   |
|    search\_columns    |     array     |     |                                 要搜索的列名ID、user\_login、user\_email、user\_url、user\_nicename、display\_name                                 |
|        orderby        | string\|array |     | <p>对检索到的用户进行排序的字段</p><p>ID、name、include、login、loginin、 </p><p>nicename、nicenamein、email、</p><p>url、registered、post_count、meta_value</p> |
|         order         |     string    | ASC |                                                                 ASC/DESC                                                                |
|         offset        |               |     |                                                                    0                                                                    |
|         number        |      int      |  -1 |                                                                查询数量，-1全部                                                                |
|         paged         |               |     |                                                                    1                                                                    |
|      count\_total     |               |     |                                                          (bool)#true是否统计找到的用户总数                                                         |
|         fields        | string\|array | all |                      <p>all/ID/display_name/user_login/user_nicename</p><p>/user_email/user_url/user_registered</p>                     |
|          who          |               |     |                                                            要查询的用户类型，接受author                                                            |
| has\_published\_posts |  bool\|array  |     |                                                            是否发布过文章或发布了指定ID的文章                                                           |
|        nicename       |               |     |                                                                 nicename                                                                |
|     nicename\_\_in    |     array     |     |                                                                nicename数组                                                               |
|  nicename\_\_not\_in  |     array     |     |                                                               排除nicename数组                                                              |
|         login         |               |     |                                                                   登录名                                                                   |
|      login\_\_in      |     array     |     |                                                                  登录名数组                                                                  |
|    login\_\_not\_in   |     array     |     |                                                                 排除登录名数组                                                                 |

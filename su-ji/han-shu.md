# 函数

### 获取关联指定分类法术语的对象ID数组

`get_objects_in_term($term_ids, $taxonomies, $args = [])`

* _@param_ `int|array $term_ids` — 术语ID
* _@param_ `string|array $taxonomies` — 分类法
* _@param_ `array|string $args` — 设置对象ID顺序 ASC or DESC
* _@return_ `array|WP_Error`\
  成功返回对象ID数组，分类法不存在时 WP\_Error

### 菜单相关函数

|     需求     |            解决            |
| :--------: | :----------------------: |
|   菜单位置分类法  |         nav\_menu        |
| 菜单项的post类型 |      nav\_menu\_item     |
|  判断是否是菜单项  |    is\_nav\_menu\_item   |
|    删除菜单项   | wp\_delete\_post，删除前应先判断 |

|               名称              |                   说明                   |
| :---------------------------: | :------------------------------------: |
|   wp\_get\_nav\_menu\_object  |   使用ID、name、slug或WP\_Term获得菜单位置，即术语对象  |
|   wp\_get\_nav\_menu\_items   |             获取指定菜单（术语）的菜单项目            |
|   wp\_save\_nav\_menu\_items  |                 保存菜单项数据                |
|  wp\_update\_nav\_menu\_item  |                保存或新建菜单项                |
| wp\_update\_nav\_menu\_object | 添加菜单位置，简单参数：0, \['menu-name' =>'菜单名称'] |

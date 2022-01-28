# 全局变量

### wp-load

|           变量名          |     说明     |
| :--------------------: | :--------: |
|        timestart       | 开始加载WP时的时间 |
| wp\_theme\_directories |    主题目录    |
|     wp\_post\_types    | 已注册的post类型 |
|     wp\_taxonomies     |   已注册的分类法  |
|     wp\_meta\_keys     |  已注册的meta  |
|       wp\_filter       |    所有钩子    |

### 后台页面

|         变量名        |        说明       |
| :----------------: | :-------------: |
|       pagenow      |    当前访问的入口文件名   |
|    plugin\_page    | $\_GET\['page'] |
|   typenow/taxnow   |   当前查询的类型或分类法   |
|     page\_hook     |    当前页面的动作钩子名   |
|    hook\_suffix    |     当前页面钩子后缀    |
|        menu        |       顶级菜单      |
|       submenu      |       子菜单       |
| admin\_page\_hooks |    顶级菜单slug数组   |

### 后台文章相关

|         变量名        |  说明 |
| :----------------: | :-: |
|     post\_type     |     |
| post\_type\_object |     |

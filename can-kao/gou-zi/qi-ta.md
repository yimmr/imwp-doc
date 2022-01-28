# 其他

### 动作钩子

|            名称           |         说明        |  参数 |
| :---------------------: | :---------------: | :-: |
| core\_upgrade\_preamble |   核心、插件和主题更新表后执行  |     |
|    after\_db\_upgrade   |   升级数据库后下一页加载时执行  |     |
|        do\_robots       | 若是robots.txt请求则执行 |     |

### 过滤钩子

|                名称                |               说明              |               参数               |
| :------------------------------: | :---------------------------: | :----------------------------: |
|      upgrader\_pre\_download     |           是否返回自定义更新包          | false, $packlink, WP\_Upgrader |
|   site\_transient\_{$transient}  |            过滤站点瞬态值            |          $value, $key          |
| presite\_transient\_{$transient} | 过滤瞬态不存在时返回的默认值，false以外的值都提前返回 |           false, $key          |
|            robots\_txt           |         过滤robots.txt输出        |         $txt, $isPublic        |

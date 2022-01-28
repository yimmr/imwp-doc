# 数据库字段

## wp\_options

active\_plugins：已激活插件的数组（启动的插件都存个相对路径：plugins目录下入口文件）

```php
// 可以这样激活一个插件
$plugins = get_option('active_plugins', []);
array_push($plugins, 'plugin-name/index.php');
update_option('active_plugins', $plugins);
// 也可以调用自带函数
activate_plugin('plugin-name/index.php', null);
```

permalink\_structure：设置固定连接，如：update\_option('permalink\_structure', '/%post\_id%');

**一些选项：**

|        选项       |                         说明                         |
| :-------------: | :------------------------------------------------: |
| active\_plugins |                     已激活插件的相对路径                     |
|  rewrite\_rules | 路由规则数组。键名为匹配规则，值为实际的访问路径，用 `$matches` 数组获取括号匹配里的值。 |

## wp\_usermeta

|           字段名           | 值类型 |     说明    |
| :---------------------: | :-: | :-------: |
|       admin\_color      |  略  |   管理页色调   |
| show\_admin\_bar\_front |  略  | 前台是否显示工具栏 |

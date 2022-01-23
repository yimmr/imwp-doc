# 菜单页面

一个菜单页面分**菜单配置**、**页面组件**和**数据模型**三个部分，数据模型可能还依赖 [**表单配置**](../can-kao/biao-dan-pei-zhi.md)。

### 菜单配置

菜单配置是一个键值对数组，提供了单个菜单的信息。调用页面组件的方法并传入菜单配置即可添加后台管理的菜单页面，代码需要在 `admin_menu` 钩子执行。

#### 菜单页配置项：

|    键名    |   类型   |       默认值       |     说明     |
| :------: | :----: | :-------------: | :--------: |
|   title  | string |       Menu      |    页面标题    |
|   name   | string |       Menu      |    菜单名称    |
|    cap   | string | manage\_options |    访问权限    |
|   slug   | string |        ''       |   链接slug   |
|  render  | string |        ''       |  页面组件类名或回调 |
|   icon   | string |        ''       |  菜单图标(仅顶级) |
| position |   int  |       null      |    菜单位置    |
| children |  array |        -        | 子菜单数组(仅顶级) |

### 页面组件

页面组件提供菜单页面视图和添加菜单的方法。相关功能请查看组件部分的 [菜单页面](zu-jian-1/cai-dan-ye-mian.md) 。&#x20;

### 数据模型

数据模型用于菜单页面数据交互。你可以定义自己的模型或使用 [轻选项](../za-xiang/fu-wu.md#xuan-xiang-mo-xing)、 [选项模型](../shu-ju-mo-xing/xuan-xiang-mo-xing.md) 。

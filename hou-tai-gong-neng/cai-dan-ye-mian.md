---
description: 此组件是一个静态类，用于输出指定HTML
---

# 页面组件

### 类名

组件完整类名： `Impack\WP\Components\MenuPage`&#x20;

### 方法

|      名称      |                   说明                  |   |   |
| :----------: | :-----------------------------------: | - | - |
| settingsPage | 输出基于 `Settings API` 的表单，包含 `div.wra`p |   |   |
|   startWrap  |             开始 `div.wrap`             |   |   |
|    endWrap   |             结束 `div.wrap`             |   |   |
|   startForm  |   表单开始。默认提交至 `wp-admin/options.php`   |   |   |
|    endForm   |                  表单结束                 |   |   |
|       p      |                  一段描述                 |   |   |
|    factory   |        返回调用 `settingsPage` 的闭包        |   |   |

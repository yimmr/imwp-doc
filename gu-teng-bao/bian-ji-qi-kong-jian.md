# 编辑器控件

### 工具栏和侧边栏

* 基础包 <mark style="color:blue;">`@wordpress/block-editor`</mark>&#x20;
* 工具栏控件位于 `<BlockControls>` 组件内
* 侧边栏位于 `<InspectorControls>` 组件内
* 侧边栏仅用于块级设置，不应只应用于块内特定内容
* 注册块的 `supports` 属性可添加内置的设置
* 组件包 <mark style="color:blue;">`@wordpress/components`</mark> 提供 `Panel`, `PanelBody` 等内置组件，保持控件风格一致




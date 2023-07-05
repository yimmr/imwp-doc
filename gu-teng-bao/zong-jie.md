# 总结

### 包

* [@wordpress/scripts](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/scripts) WP 开发架子，提供可复用的脚步集合
* [@wordpress/data](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/data) 基于 [Redux](https://redux.js.org/) 的模块化数据管理中心
* [@wordpress/core-data](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/core-data) 用于对 WP 核心数据的访问，自动化管理 REST API 数据
* [@wordpress/element](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/element) 用于访问底层 React 的一个抽象层接口
* [@wordpress/components](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/components) 与 WP 仪表盘共享的通用组件库
* [@wordpress/blocks](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/blocks) 块的相关接口，如注册块、样式、变体等
* [@wordpress/block-editor](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/block-editor) 用于构建块编辑器的组件库
* [@wordpress/editor](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/editor) 帖子编辑器的组件库，如富文本、工具栏等
* [@wordpress/icons](https://wordpress.github.io/gutenberg/?path=/docs/icons-icon--default) WP 图标库
* [@wordpress/compose](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/compose) 一些 Hooks 和创建高阶组件的工具库
* [@wordpress/hooks](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/hooks) 类似PHP钩子的JS事件管理器
* [@wordpress/media-utils](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/media-utils) 提供 WP 媒体库上传等操作的工具包
* [@wordpress/i18n](https://github.com/WordPress/gutenberg/tree/f3da2676af29b3b672cfb3f49317193538d72d54/packages/i18n) 客户端国际化翻译工具包

### 工具栏和侧边栏

* 基础包 <mark style="color:blue;">`@wordpress/block-editor`</mark>&#x20;
* 工具栏控件位于 `<BlockControls>` 组件内
* 侧边栏位于 `<InspectorControls>` 组件内
* 侧边栏仅用于块级设置，不应只应用于块内特定内容
* 注册块的 `supports` 属性可添加内置的设置
* 组件包 <mark style="color:blue;">`@wordpress/components`</mark> 提供 `Panel`, `PanelBody` 等内置组件，保持控件风格一致

### 扩展核心块

* 自定义 CSS 类
* 块样式
* 块变体
* 过滤块
* 创建新块

### 块钩子

* 使用 <mark style="color:blue;">`@wordpress/hooks`</mark> 包，全局 `wp.hooks` 与之对应
* 添加钩子等方法需要 `vendor/plugin/function` 格式的唯一命名空间做第二个参数
* 添加/删除钩子时触发 `hookAdded` / `hookRemoved` 动作
* 开发环境可通过 `all` 钩子调试

#### 过滤块

* 后端有 [`block_type_metadata`](https://developer.wordpress.org/block-editor/reference-guides/filters/block-filters/#block\_type\_metadata) 和 [`block_type_metadata_settings`](https://developer.wordpress.org/block-editor/reference-guides/filters/block-filters/#block\_type\_metadata\_settings) 用于过滤块的原始配置和已确定的配置。
* 块配置 [`blocks.registerBlockType`](https://developer.wordpress.org/block-editor/reference-guides/filters/block-filters/#blocks-registerblocktype) ：接收 `settings, name` 参数
* 块 `edit` 组件 [`blocks.BlockEdit`](https://developer.wordpress.org/block-editor/reference-guides/filters/block-filters/#editor-blockedit) ：接收一个组件且需返回一个组件，因此要结合 [`createHigherOrderComponent`](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-compose/#createHigherOrderComponent) （React 高阶组件模式）使用。
* 编辑块和工具栏组件 [`editor.BlockListBlock`](https://developer.wordpress.org/block-editor/reference-guides/filters/block-filters/#editor-blocklistblock) ：用法与 BlockEdit 相同，新属性可添加到 `wrapperProps` 对象属性中
* 块 `save` 组件的根元素属性 [`blocks.getSaveContent.extraProps`](https://developer.wordpress.org/block-editor/reference-guides/filters/block-filters/#blocks-getsavecontent-extraprops) ：接收 `props, blockType, attributes` 参数。
* 块 `save` 组件返回的元素 [`blocks.getSaveElement`](https://developer.wordpress.org/block-editor/reference-guides/filters/block-filters/#blocks-getsaveelement) ：接收 `props, blockType, attributes` 参数。
* [移除块](https://developer.wordpress.org/block-editor/reference-guides/filters/block-filters/#removing-blocks)

{% hint style="success" %}
帖子储存的内容需和 `save` 函数输出一致，因此修改现有内容要用后端 `render_block` 钩子。例如，过滤块的前端 HTML。
{% endhint %}

### 块变体

常用于自定义核心块的属性、内部块、或类名样式等。

### 块模式

可用 [`register_block_pattern`](https://developer.wordpress.org/reference/functions/register\_block\_pattern/) 函数注册，也可在主题 `patterns` 文件夹中创建

* 以文件创建模式：将模式的注册参数写到 `php` 文件顶部注释中
  * 参数名：单词首字母大写且以空格分隔
  * 数组值：以英文逗号分隔

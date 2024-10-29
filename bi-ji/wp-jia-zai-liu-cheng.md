# WP加载流程

### 加载核心文件 `wp-load.php`&#x20;

* 设置错误报告等级
* 引入 `wp-config.php` 文件，若根目录下没有还会尝试从父目录找
* 没有配置文件，尝试重定向安装页面，否则报错
* 后台个页面入口也较早地先引入此文件
* 配置文件会引入 `wp-settings.php` 文件：
  * 引入 `WPINC/version.php` 文件：定义版本信息全局变量
  * 引入 `WPINC/load.php` 文件：基础核心函数
  * 检查数据库版本
  * 引入初始化所需的文件
  * 完整初始化各个常量，注册致命错误处理者、设置时区
  * 调用 `wp_fix_server_vars()` 修复 `$_SERVER` 全局变量，适配不同服务器环境
  * 检查是否在维护、设置开始加载计时、检查是否是调试模式
  * 过滤是否启用 `advanced-cache.php` 插件的加载
    * 位于 `WP_CONTENT_DIR/advanced-cache.php`
    * 引入文件后重新初始化由缓存加的任何钩子
  * 先引入一些核心文件、初始化 `wpdb` 实例、启用对象缓存
  * 注册默认钩子
  * 若是多站模式，引入相关文件
  * 注册 PHP 结束后回调，可用钩子 `shutdown`
  * 判断常量`SHORTINIT`是否只需要基础部件，若是则提前返回
  * 加载国际化库、判断是否未安装 wordpress
  * 引入更多核心文件
  * 初始化 `embed`、文本域注册表、引入多站特定文件、定义插件目录常量
  * 若需要则加载多站点插件，设置多站 cookie 常量
  * 设置 cookie 常量、SSL 常量
  * 为后续流程创建通用全局变量：当前界面的文件名、当前用户浏览器、当前 WP 环境
  * 初始化分类法、文章类型
  * 开始捕获文件编辑中的 PHP 错误
  * 注册主题目录，非多站模式初始化恢复模式
  * 加载插件和相关文件，设置内部编码、按需加对象缓存 post(需定义 `wp_cache_postload` 函数)
  * 执行 `plugins_loaded` 动作
  * 定义影响功能的常量、转义输入的全局变量（合并 `$_GET+$_POST`）
  * 清理评论 cookie 动作 `sanitize_comment_cookies`
  * 创建 `WP_Query`、`WP_Rewrite`、`WP`、`WP_Widget_Factory`、`WP_Roles`
  * 执行 `setup_theme` 动作
  * 设置模板常量、加载默认本地化文本、创建 `WP_Locale`、`WP_Locale_Switcher`
  * 引入当前主题的 `functions.php` 文件
  * 执行 `after_setup_theme` 动作
  * 创建站点健康检查 `WP_Site_Health` 实例，以便让 Cron 事件可以触发
  * 提前初始化当前用户实例
  * 执行 `init` 动作
  * 若是多站模式，引入状态检查文件，可用过滤钩子 `ms_site_check`
  * 执行 `wp_loaded` 钩子，已加载所有必要部件

### 执行 `WP::main` 方法

* 初始化：创建当前用户
* `do_parse_request` 返回 `bool` 决定 WP 是否解析请求，若进行解析：
  * 解析时：
    * `query_vars` 过滤允许的查询参数名数组
    * `request` 过滤最终的查询参数键值对
    * `parse_request` 解析完成后执行
  * 解析后：进行主查询、404 处理和全局变量设置等
* 发送响应头：
  * 判定`query_vars['error']`的错误码：非 `404` 在发送头后退出程序
  * 过滤钩子 `wp_headers`，接收 `headers` 和 `WP` 实例
  * 发送后钩子 `send_headers`，接收 `WP` 实例。(提前退出则不执行)
* 执行 `wp`钩子，接收 `WP` 实例
* 如果 `$wp_the_query`不存在 则指向 `$wp_query`

### 模板加载

* `template_redirect` 在加载模板前执行
* `exit_on_http_head` 过滤是否退出 `HEAD` 请求
* 特殊请求提前返回：
  * `do_robots` 和 `do_favicon` 执行机器人和图标动作
  * `pre_trackback_post` 对 trackback 数据进行预处理和验证
  * `trackback_post` 执行 trackback 后动作
* `template_include` 过滤将引入的模板文件
  * 过滤前没有模板文件会找首页模板
  * 过滤后若是假值，且当前用户可切换主题则显示错误
  * 最后返回
* 注意：`wp_using_themes()` 函数返回真才进行模板处理

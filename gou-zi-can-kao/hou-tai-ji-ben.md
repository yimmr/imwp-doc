# 后台基础

### 初始化时 <a href="#hou-tai-ji-ben-gou-zi" id="hou-tai-ji-ben-gou-zi"></a>

| 名称                          | 说明                    |
| --------------------------- | --------------------- |
| admin\_menu                 | 加载管理员菜单前触发            |
| admin\_page\_access\_denied | 显示无访问权限前触发            |
| admin\_init                 | 注册完菜单后-渲染页面前触发        |
| admin\_action\_{$action}    | 请求参数含action但没有page时触发 |

### 渲染页面前 <a href="#hou-tai-ye-mian-gou-zi" id="hou-tai-ye-mian-gou-zi"></a>

| 名称                          | 说明             |
| --------------------------- | -------------- |
| load\_{$page\_hook}         | 加载自定义页面前触发     |
| $page\_hook                 | 加载页面头部后触发      |
| load\_{$pagenow}            | 加载核心页面前触发      |
| admin\_action\_{$action}    | 后台请求含action时执行 |
| wp\_ajax\_{$action}         | 处理已登录用户的AJAX请求 |
| wp\_ajax\_nopriv\_{$action} | 处理未登录用户的AJAX请求 |

### 渲染页面时 <a href="#hou-tai-ye-mian-bu-fen-gou-zi" id="hou-tai-ye-mian-bu-fen-gou-zi"></a>

| 名称                              | 说明                                               | 参数            |
| ------------------------------- | ------------------------------------------------ | ------------- |
| admin\_enqueue\_scripts         | 引入管理页脚本                                          | $hook\_suffix |
| admin\_print\_\[styles/scripts] | <p>页内脚本样式，特定页面可用</p><p>动态后缀-{$hook_suffix}钩子</p> | -             |
| admin\_head                     | <p>管理页head，</p><p>也有-{$hook_suffix}后缀的钩子</p>     | -             |
| adminmenu                       | 输出菜单后触发                                          | -             |
| in\_admin\_header               | 后台页面的wpbody前触发                                   | -             |
| admin\_notices                  | 输出管理页通知                                          |               |
| all\_admin\_notices             | 所有通知消息                                           | -             |
| in\_admin\_footer               | 页面wpbody后触发                                      | -             |
| admin\_footer\_text             | 过滤感谢使用WP的文本                                      | $text         |
| admin\_footer                   | 跟head类似，此外还有输出页内脚本的钩子                            | -             |

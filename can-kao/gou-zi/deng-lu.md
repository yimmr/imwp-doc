# 登录注册

### 动作钩子

|            名称           |        说明        |         参数         |
| :---------------------: | :--------------: | :----------------: |
|   register\_new\_user   |     新用户注册后执行     |       $userid      |
|       login\_init       |    登录表单初始化时执行    |                    |
|  login\_form\_{$action} |  根据表单action动态执行  |                    |
| login\_enqueue\_scripts |    登录页引入脚本和样式    |                    |
|       login\_head       |      引入脚本后执行     |                    |
|      login\_header      |   登录页body标签后执行   |                    |
|      login\_footer      |   登录页body标签前执行   |                    |
| retrieve\_password\_key | 生成密码重置key但在保存前执行 | $user\_login, $key |

### 过滤钩子

|              名称             |         说明         |                           参数                           |
| :-------------------------: | :----------------: | :----------------------------------------------------: |
|         login\_title        |        登录页标题       |                   $title, $raw\_title                  |
|       login\_headerurl      |       登录页图标链接      |                          $url                          |
|      login\_headertext      |     登录页图标链接的文本     |                          $text                         |
|      login\_body\_class     |  登录页body标签的class属性 |                     $class, $action                    |
|        login\_message       |     登录表单上的消息文本     |                          $msg                          |
|    allow\_password\_reset   |     是否允许用户重置密码     |                       true, $uid                       |
|  retrieve\_password\_title  |      重置密码的邮件标题     |         $title, $user\_login, $user\_data(obj)         |
| retrieve\_password\_message | 重置密码的邮件正文，为空则不发送邮件 | <p>$msg, $key, </p><p>$user_login, $user_data(obj)</p> |

### 邮件过滤

|            名称           |      说明      |             参数            |
| :---------------------: | :----------: | :-----------------------: |
|        is\_email        |  是否是有效email  | $result, $email, $context |
|      wp\_mail\_from     |    邮件发送者地址   |           $email          |
|   wp\_mail\_from\_name  |     发件人名称    |           $name           |
| wp\_mail\_content\_type | wp\_mail内容类型 |           $type           |
|    wp\_mail\_charset    |  wp\_mail字符集 |          $charset         |
|     wp\_mail\_failed    |    发送异常时执行   |         WP\_Error         |

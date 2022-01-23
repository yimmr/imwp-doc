# 函数

### HTTP

|              名称              |      参数     |   返回值  |       说明       |
| :--------------------------: | :---------: | :----: | :------------: |
|   wp\_safe\_remote\_{$type}  | $url, $args |        |     安全发送请求     |
| wp\_remote\_retrieve\_header | $res, $name | string | 从响应中提取单个header |
|  wp\_remote\_retrieve\_body  |     $res    | string |     提取响应正文     |

### 文件功能

admin\_init 钩子之后可用，文件位置 `wp-admin/includes/file.php`

|            名称           |             参数            |        返回值        |                                说明                               |
| :---------------------: | :-----------------------: | :---------------: | :-------------------------------------------------------------: |
|      WP\_Filesystem     |   $args, $context, $bool  |     bool\|null    |                     初始化并连接WP Filesystem 抽象类                     |
|      download\_url      | $url, $timeout, $verisign | string\|WP\_Error | <p>下载到本地临时文件，并在验证签名前判断</p><p>Header:x-content-signature是否为真</p> |
|       unzip\_file       |         $file, $to        |  bool\|WP\_Error  |                           zip文件解压至某个位置                          |
|       wp\_tempnam       |     $filename, $tmpdir    |       string      |                             返回临时文件路径                            |
|    verify\_file\_md5    |      $filename, $md5      |  bool\|WP\_Error  |                             验证文件md5                             |
| verify\_file\_signature |    $file, $signs, $bool   |  bool\|WP\_Error  |                         根据ED25519签名校验文件                         |

### 字符串

|             名称            |          参数         |   返回值  |                                    说明                                    |
| :-----------------------: | :-----------------: | :----: | :----------------------------------------------------------------------: |
|   sanitize\_text\_field   |         $str        | string | <p>清理字符串(移除无效UTF-8、八位字节、</p><p>html标签、换行符、制表符和空格，</p><p>&#x3C;字符转实体)</p> |
| sanitize\_textarea\_field |                     |        |            <p>清理字符串(类sanitize_text_field，</p><p>但保留换行和空格)</p>            |
|       sanitize\_user      |     $str, $bool     |        |     <p>返回过滤后的用户名，</p><p>移除标签/八位字节/实体，</p><p>严格模式仅保留字母/数字/_/空格/./-</p>    |
|        wp\_unslash        |                     |        |                                 移除字符串中斜杠                                 |
|          wp\_kses         | $str, $allows, $arr |        |                              过滤文本并去除不允许的HTML                             |
|      wp\_kses\_split      |   $str, $arr, $arr  |        |                                 过滤HTML标记                                 |
|  excerpt\_remove\_blocks  |                     |        |                              解析块，然后呈现适合摘录的内容                             |
|     strip\_shortcodes     |                     |        |                                 删除所有短代码标签                                |
|    wp\_strip\_all\_tags   |     $str, $bool     |        |             <p>移除所有HTML标签，包括脚本和样式，</p><p>参数二可选删除剩余的换行和空格</p>             |
|      wp\_trim\_words      |  $str, $len, $more  |        |                               将文本修剪为一定数量的单词                              |
|     wp\_kses\_no\_null    |      $str, $opt     |        |                              删除字符串中所有无效控制字符                              |
|  wp\_check\_invalid\_utf8 |     $str, $bool     |        |                               检查字符串中无效的UTF8                              |
|   wp\_sanitize\_redirect  |                     |        |                                 清理重定向URL                                 |

### 文章

|            名称            |            参数            | 返回值 |      说明      |
| :----------------------: | :----------------------: | :-: | :----------: |
| add\_post\_type\_support | $posttype, $array, $args |     | 给指定类型的文章增加功能 |
|      paginate\_links     |                          |     |    生成分页链接    |

### 用户

|             名称            |         参数         |       返回值      |         说明        |
| :-----------------------: | :----------------: | :------------: | :---------------: |
|    register\_new\_user    |    login, email    | uid\|WP\_Error |      处理新用户注册      |
|      wp\_create\_user     | login, pass, email | uid\|WP\_Error |       新建一个用户      |
|     validate\_username    |        sting       |                |     检查是否有效登录名     |
|      username\_exists     |       string       |                |      检查登录名已存在     |
|         is\_email         |       string       |                |    验证是否是有效email   |
|       email\_exists       |       string       |                |    用户email是否已注册   |
| get\_password\_reset\_key |      WP\_User      |                | 创建，存储并返回用户的密码重置密钥 |
|   wp\_generate\_password  | $len, $bool, $bool |                |       生成随机密码      |

### 其他

|         名称         |                 参数                | 返回值 |     说明    |
| :----------------: | :-------------------------------: | :-: | :-------: |
|      wp\_mail      | to, subject, msg, headers, attach |     |    发邮件    |
|    wp\_redirect    |       $url, $status=302, $by      |     |    重定向    |
| wp\_safe\_redirect |                                   |     |  安全过滤重定向  |
|   wp\_parse\_args  |                                   |     |  合并字符串或数组 |
|   add\_query\_arg  |                                   |     | 添加URL请求参数 |
|   get\_transient   |                $key               |     |   读取瞬态值   |
|   set\_transient   |         $key, $val, $expir        |     |  设置可过期的内容 |

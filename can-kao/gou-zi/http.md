# HTTP

### 过滤钩子

|            名称            |         说明         |           参数           |
| :----------------------: | :----------------: | :--------------------: |
|      http\_response      |    返回HTTP API响应    | $response, $args, $url |
|       http\_origin       |      过滤HTTP请求源     |     $currentOrigin     |
|   allowed\_http\_origin  |      过滤允许的请求源      |    $allowed, $origin   |
|  allowed\_http\_origins  |    过滤允许的origin列表   |        $origins        |
| allowed\_redirect\_hosts |    过滤重定向host白名单    |      $hosts, $host     |
|   wp\_signature\_hosts   |   过滤需要进行签名的host数组  |         $hosts         |
|    wp\_signature\_url    |     文件签名对应的url     |  $sign\_url, $req\_url |
|        nonce\_life       | nonce过期时间，默认86400秒 |          $time         |

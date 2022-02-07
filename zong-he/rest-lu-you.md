# REST路由

### 概括

如果感觉注册路由比较繁琐时，可以使用 `Impack\WP\REST\Router` 类的实例来注册路由，这可以让传参变得简单，也不用多重复提供名称空间。

例如，注册一条获取最新新闻地路由：

```php
public function rest_api_init()
{
    $router = new Router('mytheme/v1');

    $router->get('news', function (\WP_REST_Request $request) {
        return \get_posts(['posts_per_page' => $request->get_param('per_page')]);;
    });
}
```

这个代码同样要在 `rest_api_init` 钩子运行。用法是先创建注册路由的实例并提供路由命名空间，使用该实例注册的路由都在这个命名空间下。

常用的请求方法与类的方法对应，比如调用 `get()` 方法添加一个映射到 GET 方法的路由。这些方法的参数与 `register_rest_route()` 类似，只不过 `$args` 同时支持 `array|callable` 两种类型。上面的写法相当于：

```php
\register_rest_route('mytheme/v1', 'news', [
    'methods'             => 'GET',
    'callback'            => function (\WP_REST_Request $request) {
        return \get_posts(['posts_per_page' => $request->get_param('per_page')]);;
    },
    'permission_callback' => '__return_true',
], false);
```

{% hint style="success" %}
如果提供给 `$args` 的参数是 `callable` ，则自动添加回调返回值为真的 `permission_callback` 参数。
{% endhint %}

注册路由后试着用浏览器访问接口 `/wp-json/mytheme/v1/news?per_page=5` ，没其它问题的话将看到一些文章数据，还可使用参数来修改查询的文章个数。

### 方法

路由类提供的方法如下：

|    名称    |                       参数                       |                   说明                  |
| :------: | :--------------------------------------------: | :-----------------------------------: |
| addRoute | $methods, $uri, $args = \[], $override = false |                  注册路由                 |
|    all   |      $uri, $args = \[], $override = false      | 注册一个 `\WP_REST_Server::ALLMETHODS` 路由 |
|    get   |      $uri, $args = \[], $override = false      |              注册一个 GET 路由              |
|   post   |      $uri, $args = \[], $override = false      |              注册一个 POST 路由             |
|    put   |      $uri, $args = \[], $override = false      |              注册一个 PUT 路由              |
|   patch  |      $uri, $args = \[], $override = false      |             注册一个 PATCH 路由             |
|  delete  |      $uri, $args = \[], $override = false      |             注册一个 DELETE 路由            |
|  options |      $uri, $args = \[], $override = false      |            注册一个 OPTIONS 路由            |

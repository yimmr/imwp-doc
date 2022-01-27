# 脚本队列

`Impack\WP\Support\Script` \*\*\*\* 类可以快速引入页面的脚本和样式，不需手动填写版本、文件地址等参数。

### 使用方式

1\. 脚本入队前先静态调用 `uri` 方法设置文件所在的基础URL和目录，远程文件可以不设置目录。

```php
public function wpEnqueueScripts(){
    $script = Impack\WP\Support\Script::uri($this->app->publicUrl(), $this->app->publicPath());
}
```

{% hint style="success" %}
脚本队列和 `wp_enqueue_script` 函数一样，都是在 `wp_enqueue_scripts` 钩子执行
{% endhint %}

如果文件是按照类型放在不同目录，可以先调用 `setSubDir` 方法设置每个类型对应的二级目录，参数只接受一个键值对数组：

```php
$script->setSubDir(['js' => 'scripts', 'css' => 'styles']);
```

2\. 设置基础信息后会返回一个实例，可以用它引入目录下的文件：

```php
$script->css('style')->js('script.min');
```

入队时一般只填文件名即可，默认会自动用文件修改时间做版本号，也可以手动设置版本或设为 `null` 禁止添加版本，如下引入的文件将没有版本号：

```php
$script->css('style', [], 'all', '', null);
```

3\. 若需要引入页内的脚本可以调用 `inline` 方法：

```php
$script->inline('alert("你好！")', 'script-min');
```

{% hint style="success" %}
css、js、inline 方法的参数接近于 WP函数，但调换了传参顺序。
{% endhint %}

4\. 如果需要引入其他URL的文件，建议按照第一步创建新实例，而不是修改旧实例的URL。

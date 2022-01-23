# 核心模型

当前版本只支持创建 **User/Post/Term/Comment** 表的模型，这也是WordPress主要的数据表，只有这些模型支持额外的功能，但也有一定的使用限制。

## 基本功能

当你调用不存在的方法时，将会添加一个与方法同名的查询参数：

```php
// ['paged' => 2, 'tag_id' => 1]
$args = App\Models\Post::paged(2)->tagId(1)->getQueryVars();
```

{% hint style="success" %}
方法名支持驼峰写法，自动转为下划线写法的参数名
{% endhint %}

### get/first/find/all

默认应支持取部分列的数据，但核心模型中，传入的值仅用做查询参数 `fields` 的值：

```php
$postIds = App\Models\Post::all('id'); // ['fields' => 'ids']
```

{% hint style="success" %}
名称支持自动转为正确的参数值：id -> ids、\* -> all
{% endhint %}

### where

此方法不用于添加 SQL Where 子句，只是简单设置查询参数，主键名会自动转成参数名称。下方示例设置查询参数，并用此参数执行 `get_posts()` 来查询数据：

```php
$posts = App\Models\Post::where('post_type', 'post')->where('p', '1')->get();
```

以上代码会得到一个模型数组，但底层查询是：

```php
$posts = get_posts(['post_type' => 'post', 'p' => 1]);
```

调用 `getQueryVars` 获取当前的查询参数：

```php
$args = App\Models\Post::where('ID', 1)->where('post_type', 'post')
->getQueryVars();

// 主键被自动转换，因此打印: ['post__in' => [1], 'post_type' => 'post']
print_r($args);
```

### whereIn/whereNotIn

同样不用于添加 Where 子句，默认执行时只添加 `{$column}__in | {$column}__not_in` 格式的查询参数，但 `$column` 是数据库主键时，会设置 `include | exclude | post__in` 等类型的参数：

```php
App\Models\Post::whereIn('post_parent', 1)->get(); // ['post_parent__in' => [1]]

App\Models\User::whereNotIn('ID', 1)->get();  // ['exclude' => [1]]
```

### **whereMeta/whereMetaWith**

`whereMeta` 用于快速设置 **meta\_query** 参数，每执行一次就追加一个子数组条件。若想设置多条件的关系，可以使用 `whereMetaWith` 方法，此方法还支持第二个参数，用于设置下次执行的索引，实现更复杂的查询：

```php
Post::whereMeta('key', 'value')

// 设置关系为或，后续条件追加到 meta_query[1] 数组
->whereMetaWith('OR', 1)->whereMeta('1_key', '1_value')

// 仅将后续条件推送到 meta_query[2][0] 数组
->whereMetaWith(false, '2.0')->whereMeta('2_0_key', '2_0_value')->whereMeta('2_0_key1', '2_0_value1')

// 设置关系为且，后续条件追加到 meta_query 数组
->whereMetaWith('AND', null)->whereMeta('end', 'end')

->getQueryVars()
```

{% hint style="success" %}
类似的方法还有：whereTax/whereTaxWith，用于快速设置tax\_query
{% endhint %}

## 快速对meta增删改查

核心模型实例上 `getMeta` 、 `addMeta` 、 `updateMeta` 、 `deleteMeta` 方法可以实现元数据的增删改查，除了不需传入ID，其余参数与 WordPress 内置函数相似：

```php
App\Models\User::first()->getMeta('nickname');
```

{% hint style="info" %}
执行读操作时，默认只取单个数据，与 get\_post\_meta 相反
{% endhint %}

## 支持的过滤钩子

|             名称            |       参数       |    说明   |
| :-----------------------: | :------------: | :-----: |
| imwp\_query\_insert\_data | $values, $from | 新增前过滤数据 |
| imwp\_query\_update\_data | $values, $from | 更新前过滤数据 |

# Backbone

### 引入

直接加入队列：

```php
	wp_enqueue_script( 'wp-api' );
```

或者用作指定脚本依赖项：

```php
\wp_enqueue_script('main', $this->publicURL('js/main.js'), ['wp-api']);
```

### 自定义类型

此库支持自定义类型，使用前先扩展对象并且类型的注册参数 `show_in_rest` 为真，示例：

```javascript
const CustomPost = wp.api.models.Post.extend({
  urlRoot: wpApiSettings.root + 'wp/v2/custom_post_type',
  defaults: {
    type: 'custom_post_type',
  },
});

const CustomPosts = wp.api.collections.Posts.extend({
  url: wpApiSettings.root + 'wp/v2/custom_post_type',
});

new CustomPosts().fetch().then((posts) => {
  // do something with the custom posts
});
```

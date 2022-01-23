# 文件系统

Filesystem 是基于 `$wp_filesystem` 的文件交互类，功能基本一致，但方法名支持驼峰式写法，并还增加了解压、复制文件的功能。

### 基本用法

使用前需要绑定到应用容器中：

```php
 $app->singleton('filesystem', \Impack\WP\Base\Filesystem::class);
```

获取文件大小：

```php
$app->make('filesystem')->size();
```

复制目录：

```php
$app->make('filesystem')->copyDir($from, $to);
```

获得实例的所有方法：

```php
$methods = $app->make('filesystem')->getMethods();
```

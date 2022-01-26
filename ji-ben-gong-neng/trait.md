# 扩展

本节介绍的均为 PHP Trait ，主类 `use TraitName` 就可以扩展对应的功能，当然也可以扩展其他类。

### 选项

开发主题时可能会使用 `option` （选项），为了避免冲突，选项名称需要带上主题自己的前缀，但这也为开发带来不便，而 `OptionTrait` 可以解决这个问题。

Trait全名： `Impack\WP\Base\OptionTrait` 。

**扩展了以下方法：**

* addOption
* getOption
* updateOption
* deleteOption

功能和 WordPress 的函数一致，只是选项名称不需带上前缀，操作不带名称前缀的选项仍需使用内置函数。例如简单获取选项 `imwp_text` 的值：

```php
$this->getOption('text');
```

### 单例容器

让主类支持简单的应用容器功能，目前只限于单例。可以注册闭包，不支持类型反射，但会检查对象是否有 `setApp` 方法，如有将自动使用主类对象做实参调用此方法。

Trait全名： `Impack\WP\Base\ContainerTrait` 。

**扩展了以下方法：**

* instance: 添加单例&#x20;
* singleton: 注册单例，但不构建实例
* make: 获取或构建单例
* forgetInstance: 移除单例
* useConfig: 构建并添加 `config` 单例

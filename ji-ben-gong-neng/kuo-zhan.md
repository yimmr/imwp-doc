# 扩展

本节介绍的均为 PHP Trait ，主类 `use TraitName` 就可以扩展对应的功能，当然也可以扩展其他类。

### 选项

开发主题时可能会使用 `option` （选项），为了避免冲突，选项名称需要带上主题自己的前缀，但这也为开发带来不便，而 `OptionTrait` 可以解决这个问题。

扩展了以下方法：

* **addOption**
* **getOption**
* **updateOption**
* **deleteOption**

功能和 WordPress 的函数一致，只是选项名称不需带上前缀，操作不带名称前缀的选项仍需使用内置函数。例如简单获取选项 `imwp_text` 的值：

```php
$this->getOption('text');
```

# 选项模型

选项模型实现**表单配置**与**option数据**关联。主要功能是选项数据的增删改查，或读取表单配置。

选项模型需要继承抽象类 `Impack\WP\Config\Option` 。如下创建管理主题选项数据的模型：

```php
namespace App\Options;

use Impack\WP\Config\Option;

class Theme extends Option
{
    // 配置文件名
    protected $name = 'theme';
}
```

模型需要定义 `name` 属性，值为不带扩展的表单配置文件名。默认情况下，模型的这些方法依赖应用容器，因此创建模型时需要注入应用容器。

|     方法名    |                  功能                  |
| :--------: | :----------------------------------: |
| loadFields |               返回表单配置数组               |
| getFullKey |               返回带前缀的字段名              |
|  getConfig | 返回 `\Impack\WP\Config\Repository` 实例 |

你覆盖上面的方法实现自己的逻辑。

**基本的交互方法**

|         方法名        |         说明        |
| :----------------: | :---------------: |
| get/set/has/delete | 读取/设置/判断/删除option |
|  getAll/deleteAll  |     读取/删除所有选项     |
|     getDefault     |    读取所有或指定的默认值    |
|      getFields     |      读取表单字段配置     |

{% hint style="success" %}
读取数据时有临时缓存，不需担心重复查询数据库
{% endhint %}

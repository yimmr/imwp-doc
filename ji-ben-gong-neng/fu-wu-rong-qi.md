# 应用容器

### 容器

本文只简单说明容器在主题开发中的使用，关于容器的具体概念可以通过网上的资料了解。

容器是应用运行时的单例，可以先绑定类到容器中，在实际用到时创建已绑定的实例，创建实例时还支持依赖注入，即自动给构造函数传参，参数值通过反射参数类型创建。此外，容器可以管理单例，你不需要明确地写一个类为单例，只需以单例的形式绑定到容器。

### 应用容器

容器提供了管理类和实例的功能，而应用容器是在容器的基础上创建，不仅具备容器的功能还提供应用所需的功能。**imwp** 只提供了抽象的应用容器类 `Impack\WP\Base\Application` ，你可以基于抽象类创建自己的应用容器类，类需要实现抽象方法 `provider` ，例如：

```php
<?php
namespace App;

use Impack\WP\Base\Application as BaseApplication;

class Application extends BaseApplication
{
    public function provider()
    {
    }
}
```

{% hint style="success" %}
如果对这里的命名空间有疑惑可以先了解[目录结构](../#mu-lu-jie-gou)
{% endhint %}

### 名称前缀

主题保存数据到数据库时应注意名称冲突，给字段名添加名称前缀可以避免冲突。但没处都写前缀将变得难以管理，因此统一将前缀设置在应用容器并调用 `prefix` 方法获得带前缀的字段名。

名称前缀默认值是 `imwp` ，可以重写这个属性：

```php
protected $prefix = 'custom';
```

调用 `prefix` 方法给字段名加前缀：

```php
echo $this->prefix('name'); // custom_name
```

### 服务提供者

应用容器 `provider` 方法是简版服务提供者，用于给应用绑定服务，例如绑定与主题选项数据交互的类：

```php
public function provider()
{
    $this->singleton('themeOption', \App\Options\Theme::class);
}
```

当需要读取主题选项数据时，可以通过应用容器创建实例 `$this->make('themeOption')` 。

### 核心钩子

应用启动时会自动添加WP钩子，只需实现与钩子名对应的方法（方法名需驼峰式写法）即可，不需手动调用 `add_action` 函数，这个方法中的代码将在 `init` 钩子执行：

```php
public function init()
{
    echo '到达init阶段';
}
```

仅这些钩子支持与容器方法对应及自动添加：

|        名称       |    说明   |
| :-------------: | :-----: |
|   bootstrapped  | 启动引导程序后 |
| afterSetupTheme |  加载主题后  |
|       init      | WP核心已加载 |

### 容器参考

**应用容器常用：**

|       名称       |           说明          |
| :------------: | :-------------------: |
|     setPath    |       设置应用目录（主题）      |
|     setUrl     |     设置应用目录的URL（主题）    |
|      path      |         返回应用目录        |
|   storagePath  |        返回文件储存目录       |
|   configPath   |        返回配置文件目录       |
|   publicPath   |        返回公共资源目录       |
|       url      |       返回主题目录的URL      |
|    publicUrl   |      返回公共资源目录的URL     |
|   isDebugging  |        是否已开启调试        |
|    timestart   |       WP开始加载时的时间      |
|     prefix     |       给指定键名设置前缀       |
|      boot      |          启动应用         |
| web/admin/rest | 分别是注册前台/后台/REST API服务 |

**类容器常用：**

|       名称       |                  说明                  |
| :------------: | :----------------------------------: |
|    make/get    |                 创建实例                 |
|       has      |                是否已绑定实例               |
|    resolved    |                是否已解析实例               |
|    isShared    |                 是否是单例                |
|      bind      |                 注册实例                 |
|     bindIf     |               未注册时注册实例               |
|    singleton   |                单例形式绑定                |
|    instance    |               绑定已实例化的单例              |
|      alias     | <p>设置别名，</p><p>对应还有get,is,remove</p> |
| forgetInstance |                 移除单例                 |
|      flush     |                 重置容器                 |
|    resolving   |                添加解析事件                |
|   getBindings  |              返回容器已绑定的实例              |
|      call      |            执行一个方法（支持依赖注入）            |

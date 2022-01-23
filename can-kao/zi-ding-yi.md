# 自定义

### 面板

|         name         |        type       | description |        default       |
| :------------------: | :---------------: | :---------: | :------------------: |
|     **priority**     |        int        |   显示位置的优先级  |          160         |
|    **capability**    |       string      |    所需的权限    | edit\_theme\_options |
|  **theme\_supports** | string\|string\[] |   所需的主题功能   |                      |
|       **title**      |       string      |      标题     |                      |
|    **description**   |       string      |      描述     |                      |
|       **type**       |       string      |      类型     |                      |
| **active\_callback** |      callable     |     显示条件    |                      |

### 区域

|           name          |        type       | description |        default       |
| :---------------------: | :---------------: | :---------: | :------------------: |
|       **priority**      |        int        |   显示位置的优先级  |          160         |
|        **panel**        |       string      |    所属的面板    |                      |
|      **capability**     |       string      |    所需的权限    | edit\_theme\_options |
|   **theme\_supports**   | string\|string\[] |   所需的主题功能   |                      |
|        **title**        |       string      |      标题     |                      |
|     **description**     |       string      |      描述     |                      |
|         **type**        |       string      |      类型     |                      |
|   **active\_callback**  |      callable     |     显示条件    |                      |
| **description\_hidden** |        bool       |  隐藏帮助图标后的描述 |         false        |

### 控件

|         name         |          type          |             description             |        default       |
| :------------------: | :--------------------: | :---------------------------------: | :------------------: |
| **instance\_number** |           int          |             相对于其他实例的创建顺序            |                      |
|      **manager**     | WP\_Customize\_Manager |               定制器引导实例               |                      |
|        **id**        |         string         |              控件HTML ID              |                      |
|     **settings**     |          array         |        相关的所有设置。未定义时使用 **$id**       |                      |
|      **setting**     |         string         |                 主要设置                |        default       |
|    **capability**    |         string         |                所需的权限                | edit\_theme\_options |
|     **priority**     |           int          |               显示位置的优先级              |          160         |
|      **section**     |         string         |                所属的区域                |                      |
|       **label**      |         string         |                  标识                 |                      |
|    **description**   |         string         |                  描述                 |                      |
|      **choices**     |          array         | 'radio' 或 'select' 控件的选项列表，键为值，值为标识 |                      |
|   **input\_attrs**   |          array         |               输入框的属性数组              |                      |
|  **allow\_addition** |          bool          |        允许用户添加新页面，目前仅用于下拉页面控件        |         false        |
|       **type**       |         string         |                  类型                 |                      |
| **active\_callback** |        callable        |                 显示条件                |                      |

### 设置

|            name            |        type       |                description                |        default       |
| :------------------------: | :---------------: | :---------------------------------------: | :------------------: |
|          **type**          |       string      |                     类型                    |      theme\_mod      |
|       **capability**       |       string      |                   所需的权限                   | edit\_theme\_options |
|     **theme\_supports**    | string\|string\[] |                  所需的主题功能                  |                      |
|         **default**        |       string      |                    默认值                    |          ''          |
|        **transport**       |       string      | 数据传输方式。'postMessage'选择性更新预览，'refresh'重载预览 |        refresh       |
|   **validate\_callback**   |      callable     |                   验证值的回调                  |                      |
|   **sanitize\_callback**   |      callable     |                   清理值的回调                  |                      |
| **sanitize\_js\_callback** |      callable     |                 转为JSON值的回调                |                      |
|          **dirty**         |        bool       |                创建时初始设置是否已变更               |                      |

### 选择性刷新

|           name           |   type   |            description           |   default   |
| :----------------------: | :------: | :------------------------------: | :---------: |
|         **type**         |  string  |                类型                |   default   |
|       **selector**       |  string  |            jQuery 选择器            |             |
|       **settings**       |  string  |    setting id，未定义时使用控件 **$id**   |             |
|   **primary\_setting**   |  string  |        负责渲染该区域的setting id        |   $setting  |
|      **capability**      |  string  |            编辑此区域所需的权限            | 继承自 setting |
|   **render\_callback**   | callable | 渲染回调。回调可输出或返回需要呈现的内容，异常可返回 false |             |
| **container\_inclusive** |   bool   |          替换整个容器还是仅替换子节点          |    false    |
|   **fallback\_refresh**  |   bool   | 区域无法刷新时是否重载预览，回调返回 false 时视为渲染失败 |     true    |

### 内置自定义控件

|                    class                   |               description               | value type |
| :----------------------------------------: | :-------------------------------------: | :--------: |
|      **WP\_Customize\_Media\_Control**     | WordPress 媒体管理器，限制上传类型用 'mime\_type' 参数 |    附件 ID   |
| **WP\_Customize\_Cropped\_Image\_Control** |                选择图像后可裁剪图像               |   附件 URL   |
|     **WP\_Customize\_Upload\_Control**     |                   附件上传                  |   附件 URL   |
|      **WP\_Customize\_Color\_Control**     |                  颜色选择器                  |   string   |


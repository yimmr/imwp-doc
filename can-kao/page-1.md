# Page 1

### 面板 <a href="#mian-ban" id="mian-ban"></a>

nametypedescriptiondefault**priority**int显示位置的优先级160**capability**string所需的权限edit\_theme\_options**theme\_supports**string|string\[]所需的主题功能​**title**string标题​**description**string描述​**type**string类型​**active\_callback**callable显示条件​

### 区域 <a href="#qu-yu" id="qu-yu"></a>

nametypedescriptiondefault**priority**int显示位置的优先级160**panel**string所属的面板​**capability**string所需的权限edit\_theme\_options**theme\_supports**string|string\[]所需的主题功能​**title**string标题​**description**string描述​**type**string类型​**active\_callback**callable显示条件​**description\_hidden**bool隐藏帮助图标后的描述false

### 控件 <a href="#kong-jian" id="kong-jian"></a>

nametypedescriptiondefault**instance\_number**​相对于其他实例的创建顺序​**manager**WP\_Customize\_Manager定制器引导实例​**id**string控件HTML ID​**settings**array相关的所有设置。未定义时使用 **$id**​**setting**string主要设置default**capability**string所需的权限edit\_theme\_options**priority**int显示位置的优先级160**section**string所属的区域​**label**string标识​**description**string描述​**choices**array'radio' 或 'select' 控件的选项列表，键为值，值为标识​**input\_attrs**array输入框的属性数组​**allow\_addition**bool显示新增内容的 UI，目前仅用于下拉页面控件false**type**string类型​**active\_callback**callable显示条件​

### 设置 <a href="#she-zhi" id="she-zhi"></a>

nametypedescriptiondefault**type**string类型theme\_mod**capability**string所需的权限edit\_theme\_options**theme\_supports**string|string\[]所需的主题功能​**default**string默认值''**transport**string数据传输方式。'postMessage'选择性更新预览，'refresh'重载预览refresh**validate\_callback**callable验证值的回调​**sanitize\_callback**callable清理值的回调​**sanitize\_js\_callback**callable转为JSON值的回调​**dirty**bool创建时初始设置是否已变更​

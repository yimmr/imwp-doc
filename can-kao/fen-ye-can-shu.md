# 分页参数

|           名称          |   类型   |              默认值             |            说明            |
| :-------------------: | :----: | :--------------------------: | :----------------------: |
|          base         | string |               -              |           基础链接           |
|         format        | string |           ?page=%#%          |          分页链接格式          |
|          total        |   int  |  $wp\_query->max\_num\_pages |            总页数           |
|         current       |   int  |            $paged            |           当前页码           |
|      aria\_current    | string |             page             |      aria-current属性值     |
|        show\_all      |  bool  |             false            |          是否显示所有页         |
|        end\_size      |   int  |               1              |        上页后和下页前显示几个       |
|        mid\_size      |   int  |               2              |         当前页两边显示几个        |
|       prev\_next      |  bool  |             true             |         是否包括上下页按钮        |
|     **** prev\_text   | string |          « Previous          |           上一页文字          |
|       next\_text      | string |            Next »            |           下一页文字          |
|          type         | string |             plain            | 返回的格式：plain, array, list |
|        add\_args      |  array |              \[]             |          URL参数数组         |
|      add\_fragment    | string |              ''              |        附加到每个链接的字符        |
|  before\_page\_number | string |              ''              |          页码前的文字          |
|   after\_page\_number | string |              ''              |          页码后的文字          |

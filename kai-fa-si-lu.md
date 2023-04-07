# 开发思路

## 后台设置

* 表单默认层次为 <mark style="color:orange;">section > table > tr > td</mark> ，td 可以是单个或一组字段
* 当 section 对应一个 option 时，选项数组规划两层合适
* 一个页面对应 option 时，选项划三层 `opts[section][tds][td]` 。

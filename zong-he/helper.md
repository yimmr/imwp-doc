# Helper

开发中经常用到一些实用的 `utility` 函数，为避免命名冲突，代码逻辑分散各处，一般使用静态类的方法实现这些功能。

例如主题使用静态类 `App\Helper` 提供一些实用的功能：

```php
<?php

namespace Impack\WP\Support;

class Helper
{
    /**
     * 获取指定模板的页面链接
     *
     * @param string $template
     * @return string
     */
    public static function getPageLinkByTemplate($template)
    {
        return (string) \get_permalink(self::getPageIDByTemplate($template, 1));
    }

    /**
     * 查询使用指定模板的页面ID
     *
     * @param string $template
     * @param int $number
     * @return int|int[]
     */
    public static function getPageIdByTemplate($template, $number = 1)
    {
        $pages = \get_posts([
            'post_type'      => 'page',
            'fields'         => 'ids',
            'meta_key'       => '_wp_page_template',
            'meta_value'     => $template,
            'posts_per_page' => $number,
        ]);

        return $number == 1 ? intval(current($pages)) : $pages;
    }

    /**
     * 截取内容摘要
     *
     * @param \WP_Post $post
     * @param int $length
     * @param string $more
     * @return string
     */
    public static function getExcerpt($post, $length = 55, $more = '&hellip;')
    {
        $content = \strip_shortcodes($post->post_content);
        $content = \excerpt_remove_blocks($content);
        $content = \apply_filters('the_content', $content);
        $content = str_replace(']]>', ']]&gt;', $content);

        return \wp_trim_words($content, $length, $more);
    }
}
```

---
layout: post
title: explode拆分字符串big5版
wordpress_id: 12
date: 2005-02-24 17:32:00.000000000 +08:00
category: tech
tags: php explode big5 chinese
---
{% highlight php %}
<?php
$s='帆船|火車';
var_dump(explode('|',$s));
?> 
{% endhighlight %}

這一段程式用來拆分一個字符串
本來預期的結果應該是拆成 帆船 跟 火車
可是由於big5的 帆 這個字big5内碼是啊a67c，跟分割符|的内碼7c相沖突，實際結果是 ? ?船 火車
可以用下面的函數來解決
145
{% highlight php %}
<?php
/**
* 用$sep來拆分$str
*
* @param   $sep    分隔符
* @param   $str    需要處理的string
*
* @return  array
*
* @author  David Jiang (smartynaoki@gmail.com)
*/
function big5_explode($sep,$str)
{
    if (function_exists('mb_convert_encoding'))
    {
        $str=mb_convert_encoding($str,'utf-8','big5');
        $r=explode($sep,$str);
        foreach($r as $k=>$v)
        {
            $r[$k]=mb_convert_encoding($v,'big5','utf-8');
        }
    }
    elseif (function_exists('iconv'))
    {
        $str=iconv('big5','utf-8',$str);
        $r=explode($sep,$str);
        foreach($r as $k=>$v)
        {
            $r[$k]=iconv('utf-8','big5',$v);
        }
    }
    else
    {
        $r=explode($sep,$str);
    }
    return $r;
}
?> 
{% endhighlight %}

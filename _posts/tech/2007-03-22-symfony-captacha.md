---
layout: post
title: symfony中实现验证码的方法(利用jpgraph)
wordpress_id: 22
date: 2007-03-22 05:26:17.000000000 +08:00
tags: symfony plugin
category: tech
---
今天刚好用到验证码，在[symfony的wiki](http://trac.symfony-project.com/trac/wiki/HowToCaptcha)找到一个解决的方法，记录下来。
这个方法其实使用的是大名鼎鼎的jpgraph的库，由于有[jpgraph的插件](http://trac.symfony-project.com/trac/wiki/sfJpGraphPlugin)，安装很容易:
首先到你的项目根目录执行下面的命令，利用svn命令行工具下载这个插件
	svn co http://svn.symfony-project.com/plugins/sfJpGraphPlugin ./plugins/sfJpGraphPlugin
如果你的项目使用svn管理，请设定sfJpGraphPlugin目录的属性:
	svn propedit svn:externals plugins
输入:
	sfJpGraphPlugin http://svn.symfony-project.com/plugins/sfJpGraphPlugin
ok,jpgraph插件安装完了。

##验证码部分

动作:
{% highlight php %}
<?php
class profileActions extends sfActions

{
    
    
    
    public function executeRegister()
    
    {
        
        if ($this->getRequest()->getMethod() == sfRequest::POST) {
            
            // process form
            
        }
        
        require_once 'jpgraph/jpgraph_antispam.php';
        
        $antispam = new AntiSpam();
        
        $antispam_string = $antispam->rand(5);
        
        $this->getUser()->setAttribute('antispam', $antispam_string);
        
    }
    
    
    
    public function executeAntispam() {
        
        if (!$string = $this->getUser()->getAttribute('antispam')) {
            
            return sfView::NONE;
            
        }
        
        
        
        $this->getResponse()->setContentType('image/jpeg');
        
        $antispam = new AntiSpam($string);
        
        echo $antispam->stroke();
        
        return sfView::NONE;
        
    }
    
}

{% endhighlight %}
模版:


验证类:myCharactersValidator.class.php

{% highlight php %}
<?php
class myCharactersValidator extends sfValidator
{
    public function execute(&$value, &$error)
    {
        $string = sfContext::getInstance()->getUser()->getAttribute('antispam');
        if ($value != $string) {
            // generate a new image....
            $antispam = new AntiSpam();
            $antispam_string = $antispam->rand(5);
            sfContext::getInstance()->getUser()->setAttribute('antispam', $antispam_string);
            $error = $this->getParameter('characters_error');
            return false;
        }
        return true;
    }
}
{% endhighlight %}

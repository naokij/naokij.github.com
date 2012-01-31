---
layout: post
title: symfony中实现验证码的方法(利用jpgraph)
wordpress_id: 22
date: 2007-03-22 05:26:17.000000000 +08:00
tags: symfony plugin
---
今天刚好用到验证码，在<a href="http://trac.symfony-project.com/trac/wiki/HowToCaptcha">symfony的wiki</a>找到一个解决的方法，记录下来。
这个方法其实使用的是大名鼎鼎的jpgraph的库，由于有<a href="http://trac.symfony-project.com/trac/wiki/sfJpGraphPlugin">jpgraph的插件</a>，安装很容易:
首先到你的项目根目录执行下面的命令，利用svn命令行工具下载这个插件
<blockquote>svn co http://svn.symfony-project.com/plugins/sfJpGraphPlugin ./plugins/sfJpGraphPlugin</blockquote>
如果你的项目使用svn管理，请设定sfJpGraphPlugin目录的属性:
<blockquote>svn propedit svn:externals plugins</blockquote>
输入:
<blockquote>sfJpGraphPlugin http://svn.symfony-project.com/plugins/sfJpGraphPlugin</blockquote>
ok,jpgraph插件安装完了。

<strong>验证码部分</strong>

动作:
<pre line="1" lang="php">
class profileActions extends sfActions
{

public function executeRegister()
{
if ($this-&gt;getRequest()-&gt;getMethod() == sfRequest::POST) {
// process form
}
require_once 'jpgraph/jpgraph_antispam.php';
$antispam = new AntiSpam();
$antispam_string = $antispam-&gt;rand(5);
$this-&gt;getUser()-&gt;setAttribute('antispam', $antispam_string);
}

public function executeAntispam() {
if (!$string = $this-&gt;getUser()-&gt;getAttribute('antispam')) {
return sfView::NONE;
}

$this-&gt;getResponse()-&gt;setContentType('image/jpeg');
$antispam = new AntiSpam($string);
echo $antispam-&gt;stroke();
return sfView::NONE;
}
}</pre>
模版:
<pre line="1" lang="php">
<label>Picture</label>
<img src="http://www.jiangle.name/wp-admin/%3C?php%20echo%20url_for%28%27profile/antispam%27%29;%20?%3E" /></pre>
验证类:myCharactersValidator.class.php

<pre line="1" lang="php"> class myCharactersValidator extends sfValidator
{
public function execute (&amp;$value, &amp;$error)
{
$string = sfContext::getInstance()-&gt;getUser()-&gt;getAttribute('antispam');
if ($value != $string) {// generate a new image....
$antispam = new AntiSpam();
$antispam_string = $antispam-&gt;rand(5);
sfContext::getInstance()-&gt;getUser()-&gt;setAttribute('antispam', $antispam_string);$error = $this-&gt;getParameter('characters_error');
return false;
}
return true;
}
}
</pre>

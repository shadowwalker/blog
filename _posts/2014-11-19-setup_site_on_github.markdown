---
layout:     post
published: true
title:      "Github静态博客搭建完全方案"
subtitle:   "讨论如何Github上搭建静态站点，不仅如此，还给出一个零成本且高质量的完全解决方案。"
date:       2014-11-19 17:29:00
author:     "ShadowWalker"
header-img: "img/sledgehammer_peak.jpeg"
location:	"Shanghai"
---
##Abstract 摘要
This article would talk about how to establish a static website on [<i class="fa fa-github"> github.com</i>](https://github.com/). Not only cover the technology and tools used, but also provide a solution to establish a blog site with zero cost and high visiting quality. I try to cover all the information that readers need in this one article, topics would related to git, jekyll, markdown, bootstrap, 3rd party comment plugins, open CDN, free online storage service, etc. In addition this site you are watching is on github.

##Quick Start 快速入门

假设你对Git已经有所了解，并对网页有最基本的认识。那么我相信在接下来的几分钟里，我们可以一起搭建出你的第一个github page，你可以用它来做你的个人宣传页，产品页，静态博客等等所有的静态页面。OK，我们开始！

* 准备好你的[github.com](https://www.github.com/)账号
* 在本地和github上配置你的SSH Key用于代码的上传，或者下载安装github桌面客户端，用客户端进行下载、上传、分支等操作。以下我使用命令行操作，客户端操作类同且简单，不再赘述。
* 建立你的github目录，给两种方案：

  方案一：<span class="text-muted">推荐</span> 在github.com网页上创建目录，再clone到本地
  
  <pre class="code-container"><code class="bash">$ git clone https://github.com/[username]/[repository_name].git
$ git checkout -b gh-pages</code></pre>

  方案二：在本地建立目录，再push到github仓库

  <pre class="code-container"><code class="bash">$ mkdir website
$ cd website    #创建，进入目录
$ git init      #git初始化
$ git checkout -b gh-pages    #创建gh-pages分支，github.com规定在这个分支上才有效</code></pre>

* 创建配置文件 _config.yml

创建文件，添加内容。注意baseurl:后一定要有空格。

  <pre class="code-container"><code class="bash">baseurl: /home</code></pre>

* 创建模板，文章，首页

创建如下目录结构和文件

  <pre class="code-container"><code class="bash">website/
  |- _config.yml
  |- _layouts/
      |- default.html
  |-  _posts/
      |- 2014-11-21-hellowold.html
  |- index.html</code></pre>

default.html

<pre class="code-container"><code class="html">&lt;!DOCTYPE html&gt;
&lt;!--请将以下[]都改成{}--&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;meta http-equiv="content-type" content="text/html; charset=utf-8" /&gt;
    &lt;title&gt;[[ page.title ]]&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    [[ content ]]
&lt;/body&gt;
&lt;html&gt;</code></pre>

2014-11-21-hellowold.html

<pre class="code-container"><code class="html">&lt;!--请将以下[]都改成{}--&gt;
    ---
    layout: default
    title: 你好，世界
    ---
    &lt;h2&gt;[[ page.title ]]&lt;/h2&gt;
    &lt;p&gt;我的第一篇文章&lt;/p&gt;
    &lt;p&gt;[[ page.date | date_to_string ]]&lt;/p&gt;</code></pre>

index.html

<pre class="code-container"><code class="html">&lt;!--请将以下[]都改成{}--&gt;
    ---
    layout: default
    title: 我的Blog
    ---
    &lt;h2&gt;[[ page.title ]]&lt;/h2&gt;
    &lt;p&gt;最新文章&lt;/p&gt;
    &lt;ul&gt;
        [% for post in site.posts %]
        &lt;li&gt;[[ post.date | date_to_string ]] &lt;a href="[[ site.baseurl ]][[ post.url ]]"&gt;[[ post.title ]]&lt;/a&gt;&lt;/li&gt;
        [% endfor %]
　　&lt;/ul&gt;</code></pre>

* 还差一步，发布

<pre class="code-container"><code class="bash">$ git add --all
$ git commit -m "first post"
$ git remote add origin https://github.com/username/website.git #如果你是clone下来的目录，这步可忽略
$ git push origin gh-pages</code></pre>

  发布之后需要等待十多分钟，你的页面就可以访问了。页面地址：http://[username].github.com/home/ 。这里的home就是刚才在_config.yml里配置的baseurl参数。

* 可选，域名绑定

  在git目录下建一个CNAME文件，里面写上你的域名。然后再你的域名服务商的DNS里添加一条记录。


##为什么要建独立博客

在开始讨论细节之前，我想先闲扯一下为什么我要花这些功夫建立自己的博客。一般来讲，我们去新浪博客，CSDN，网易或者其他一些博客平台上去创建自己的博客是非常容易的事情，并且那有设计好的很棒的主题和插件。倘若只是一般的需求，当然是首选。但是往往我们会在那些博客上看到很多的广告，还有杂七杂八的热门推荐，页面模块偏多。我希望能有一个干净整洁的页面作为我的个人博客，并在上面积累知识和信息，这是我这么做的第一个原因。

其次，这么做使得代码完全可控。这样我就有几乎无限的空间去拓展自己需要的功能，模块。在这个过程中，会不断的遇到问题，这会是一个很好地学习的机会。我能够在实践中学到很多的前端的内容，接触很多开放友好的开源程序。

再次，github的开源管理已经做得相当出色，并且利用他的免费资源，加之网络上其他的资源。完全可以建立零成本，也不用担心流量问题的出色的小站。这是让人非常兴奋的事情。

##Git 和 Github

说起Git大家肯定都不陌生，博主也不想多介绍。作为一个优秀的VCS，它能够帮你管理博客的不同版本，这样不用担心文章丢失，也可以方便的做历史回溯。github是一个国外的代码托管网站，国外还有[Bitbucket](https://bitbucket.org/)，类似的国内有[开源中国git托管平台](http://git.oschina.net/)，[GitCafe](https://gitcafe.com/)。从用途上讲，他们大同小异，只是在提供的服务上有差异，后面几个大多提供更大的空间，免费的私有目录等，但目前看还无法撼动github的地位。

如果读者不熟悉git，我推荐[廖雪峰的git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)，讲的比较浅显易懂，花两小时就可以学完，超值。

##Jekyll与静态网页

Github上的静态页面引擎用的是[Jekyll](http://jekyllrb.com/)。所谓静态页面，就是页面上的内容全部是事先写好的，而不是通过ajax等技术加载生成的。在WEB出现的早期，所有网页都是静态的，需要要用html语言写内容，浏览器讲内容下载之后解析渲染成一个网页。但是直接写html不是很直观，而且会有很多重复性的内容需要书写。Jekyll提供了一种静态页面自动生成的方案，让用户能够只关注内容本身，其他的html等又程序去生成，且它支持markdown语法，因此对没有web经验的人也更为友好。Jekyll使用ruby开发的，ruby我也没有学过。就使用来说也很简单，我目前在本地安装了jekyll，可以进行本地测试。详情还是请参考[Jekyll官网](http://jekyllrb.com/)，可以关注一下参数配置，模板等内容。

##Markdown

Markdown语法是一种兼容HTML的简单又容易书写的文本格式，它允许没有WEB经验的人像写普通博客一样书写内容（只要遵循一点规则）。就可以生成HTML以便页面显示。

[Markdown语法说明](http://wowubuntu.com/markdown/index.html)

推荐一款基于web的markdown编辑器[stackedit.io](https://stackedit.io/)

##CDN云存储方案

一句话概括CDN和它的作用就是，通过把你的文件存储在很多加速节点服务器上来提高访问加载速度，服务器会对最佳的路由和链路进行选择。

我使用的是[七牛云存储](http://www.qiniu.com/)，原因很简单，它提供一定的免费空间和流量支持。目前我把图片文件存储在上面，加载效果还不错。当然你也可以存自己的js和css。使用也很简单:

* 注册、实名认证
* 创建空间容器
* 上传文件（根据东哥的建议，可以按日期或者方便管理的形式添加前缀，如blog/img/xxx.jpg）
* 修改页面上的url为http://spacename.qiniudn.com/blog/img/xxx.jpg
* 完成!

##Open CDN 公共库

Open CDN公共库能够免费提供CDN节点加速服务，能有效提高加载速度和效果。我们如果能够找到自己所使用的js和css第三方库的CDN链接，就可以将网页中的src和href改到上面去。例如：

<pre class="code-container"><code class="html">&lt;!-- 新 Bootstrap 核心 CSS 文件 --&gt;
&lt;link rel="stylesheet" href="http://cdn.bootcss.com/bootstrap/3.3.0/css/bootstrap.min.css"&gt;
&lt;!-- 可选的Bootstrap主题文件（一般不用引入） --&gt;
&lt;link rel="stylesheet" href="http://cdn.bootcss.com/bootstrap/3.3.0/css/bootstrap-theme.min.css"&gt;
&lt;!-- jQuery文件。务必在bootstrap.min.js 之前引入 --&gt;
&lt;script src="http://cdn.bootcss.com/jquery/1.11.1/jquery.min.js"&gt;&lt;/script&gt;
&lt;!-- 最新的 Bootstrap 核心 JavaScript 文件 --&gt;
&lt;script src="http://cdn.bootcss.com/bootstrap/3.3.0/js/bootstrap.min.js"&gt;&lt;/script&gt;</code></pre>

推荐国内的open cdn:

* [bootcdn](http://www.bootcdn.cn/)
* [360网站卫士前端公共库](http://libs.useso.com/)

其中第二个非常有用的一点是将googleapis的公共库做了代理，这样只要把googleapis换成useso就可以轻松加载google的在线资源。避免了众所周知的无法加载的问题。

##第三方评论插件 Disqus

第三方的评论插件应该有很多，我这里用Disqus为例。读者也可以在本页面下面自己体会Disqus的效果。我觉得这种插件最有意思的地方是让你的静态页面看上去像个动态交互网页一样，看上去更像一个真正的博客。要在自己的网页上部署Disqus，只需要几个步骤:

* 注册[Disqus](https://www.disqus.com/)。
* 填写博客名称，填写url。这也是他们平台对你页面的一个标识。
* 然后复制代码到页面合适的位置，js建议放在body尾部，div放在你想让他展示出来的位置。它提供了评论区和评论计数两部分的代码可供选用。
* 完成!

对此有疑问？看看本页的源码就明白了！

##参考资料

> [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

听说[openshift](https://www.openshift.com/)也提供免费的代码空间可以创建wordpress，webapp等，有兴趣可以了解一下。
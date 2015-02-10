---
layout: post
title: "在GitHub上搭建Octopress"
date: 2015-02-10 18:38:16 +0800
comments: true
categories: octopress

---

#前言
github果真是一个神奇的地方.以前在使用Baidu空间和CSDN来写博客的时候,就听前辈们说过GitHub可以写博客.经过一番Google后终于弄明白了原理.
首先是github中有一个功能叫做[Pages](https://pages.github.com/),这个功能具体的作用不详,但是其中的一项是能够把你上传的html文件显示为一个网页.其次是有好事者(别的博主都这么称呼)做了一个基于github的管理工具:[Octopress](http://octopress.org/)).然后,我们就可以使用Octopress这个开源的框架加上Github Pages服务来搭建自己的Blog.

到这里可能你会觉得非常兴奋,恨不得立马就要下手开搞.引用一句Octopress官方网站的原文:  
Octopress  
A blogging framework for hackers.  
所以,如果没有一定的GitHub基础,html知识和Linux命令行知识的话,最好不要轻易的下决定.

另外,Octopress框架下的Blog系统,需要使用[Markdown](http://zh.wikipedia.org/zh/Markdown)语言来写博客,如果之前没有使用过的话,需要简单的学习一下,废话少说,开始介绍我的搭建之旅.

<!--more-->
---

#搭建Octopress
我是在iMac上面成功搭建的Octopress,具体的环境如下:

* Mac OS X 10.9.5 (这个版本号下安装Ruby1.9.3p125是非常头疼的)



安装Octopress基本上要按照以下几个步骤: 

 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. 注册GitHub账号  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. 安装Ruby1.9.3p125  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. 安装Octopress  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. 部署到GitHub上  

[这里](http://octopress.org/docs/setup/)是Octopress的官方指南，各位可以按照其中的步骤进行安装.需要注意一点,官方的教程中的第2步:Install Ruby 1.9.3 or greater using either rbenv or RVM.提到可以使用更高的Ruby版本,这一点,我在2.0.0下做过验证,没有成功,强烈建议使用版本1.9.3p125或者1.9.3

Octopress的官方指南推荐使用的是RVM和rbenv。我在安装的过程中使用的是RVM,这里同时给出两种方式来安装Ruby1.9.3


git呢我们的mac默认安装,[GitHub](https://github.com/)账号注册,直接跳过.


##安装低版本的Ruby

###1.通过Ruby安装Ruby1.9.3p125

####安装Homeview
这里我使用[Homebrew](http://brew.sh/)来安装rbenv，如果你没有Homebrew，打开终端，使用以下命令安装吧。

```
$ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

有了Homebrew就可以安装rbenv了

```
$ brew update
$ brew install rbenv
$ brew install ruby-build
```

####安装Ruby
使用rbenv安装1.9.3p125版本的ruby

```
$ rbenv install 1.9.3-p125
$ rbenv local 1.9.3-p125
$ rbenv rehash
$ ruby --version #ruby 1.9.3p125 (2012-02-16 revision 34643) [x86_64-darwin13.4.0]
```

安装完成后可以用ruby --version进行验证
###2.通过Rvm安装Ruby1.9.3p125

rvm 全称Ruby Version Manager, 是一个非常好用的ruby版本管理以及安装工具.
####安装rvm
```
$ curl -L get.rvm.io | bash -s stable
$ source ~/.bashrc
$ source ~/.bash_profile
```
修改 RVM 的 Ruby 安装源到国内的 淘宝镜像服务器，这样能提高安装速度

```
$ sed -i -e 's/ftp\.ruby-lang\.org\/pub\/ruby/ruby\.taobao\.org\/mirrors\/ruby/g' ~/.rvm/config/db
```
####Ruby的安装与切换

 * 列出已知的ruby版本
 
 ```
   $ rvm list known
 ```
  * 安装一个ruby版本
 
 ```
   $ rvm install 1.9.3p125
 ```
  * 使用一个ruby版本
 
 ```
   $ rvm use 1.9.3p125
 ```
 * 如果想设置为默认版本，可以这样
 
 ```
   $ rvm use 1.9.3p125 --default 
 ```
 * 查询已经安装的ruby
 
 ```
   $ rvm list
 ```
  * 卸载一个已安装版本
 
 ```
    $ rvm remove 1.9.3p125
 ```

##安装Octopress
安装Ruby完成后就按照官方指南安装Octpress

```
#clone octopress
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress

#安装依赖
$ gem install bundler
$ rbenv rehash
$ bundle install

#安装octopress默认主题
$ rake install
```
---------

#部署
接下来需要把Blog部署到github上去，第一步要做的是去[github](https://github.com/new)创建一个`username.github.com`的repo，比如我的就叫`douxinchun.github.com`。

然后运行以下命令，并依照提示建立github和Octopress的关联

```
$ rake setup_github_pages
```
---------

#创建博客

###生成博客
```
$ rake generate
$ rake deploy
```

把生成后的代码上传到github

```
$ git add .
$ git commit -m 'create blog'
$ git push origin source
```
完成后等待一段时间后就能访问`http://username.github.com`看到自己的博客了


###简单的修改配置
配置文件路径为`~/octopress/_config.yml`

```
url:                # For rewriting urls for RSS, etc
title:              # Used in the header and title tags
subtitle:           # A description used in the header
author:             # Your name, for RSS, Copyright, Metadata
simple_search:      # Search engine for simple site search
description:        # A default meta description for your site
date_format:        # Format dates using Ruby's date strftime syntax
subscribe_rss:      # Url for your blog's feed, defauts to /atom.xml
subscribe_email:    # Url to subscribe by email (service required)
category_feeds:     # Enable per category RSS feeds (defaults to false in 2.1)
email:              # Email address for the RSS feed if you want it.
```
编辑完成后,执行这个操作部署到GitHub中

```
$ rake generate
$ rake deploy
```
将修改保存到GitHub的远程库中

```
$ git add .
$ git commit -m "simple settings" 
$ git push origin source
```

###支持中文标签
目前版本的Octopress会在`/source/blog/categories`下创建一个`index.markdown`来作为分类的首页，但这个首页在标签有中文时会出现无法跳转的情况，原因是因为在出现中文标签时Octopress会把文件的路径中的中文转换成拼音，而在Category跳转时是直接写了中文路径，结果自然是404。解决方法是自己实现一个分类首页并处理中文。

首先按照[这里](https://kaworu.ch/blog/2013/09/23/categories-page-with-octopress/)的方法实现`index.html`,给Blog添加分类

将`plugins/category_list_tag.rb`中的

```
category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)
```

替换成

```
category_url = File.join(category_dir, category.to_url.downcase)
```
这样你的博客就可以支持中文标签的跳转了。

---------

#写博客

经过上面几部后，博客已经成功搭建，现在就可以开始写博文了。

###创建博文
```
#使用终端
$ rake new_post['title']

```
生成的文件在`~/source/_posts`目录下


###编辑博文

```
#...markdown写博文

$ rake preview #localhost:4000

$ rake generate

$ git add .
$ git commit -m "comment" 
$ git push origin source

$ rake deploy
```

---------
#参考资料

* http://octopress.org/
* http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/
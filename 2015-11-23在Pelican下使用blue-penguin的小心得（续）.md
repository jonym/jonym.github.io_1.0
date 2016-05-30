Title: 在Pelican下使用blue-penguin的小心得（续）
Date: 2015-11-23 21:20
Category: 学技能
Tags: 编程

本来是要换掉blue-penguin这个主题换成pelican-clean-blog或者pure，但事实上每一个主题都有自己的缺陷，当我实在没有办法的时候，终于还是决定继续使用这个主题，并且打开Google搜索“如何制作pelican theme”，然后决定自己修改这个主题，后来发现其实也没有太难，两天的时间基本上都搞定了，下面来说一下我都修改了些什么内容。
本来是要换掉blue-penguin这个主题换成pelican-clean-blog或者pure，但事实上每一个主题都有自己的缺陷，当我实在没有办法的时候，终于还是决定继续使用这个主题，并且打开Google搜索“如何制作pelican theme”，然后决定自己修改这个主题，后来发现其实也没有太难，两天的时间基本上都搞定了，下面来说一下我都修改了些什么内容。
本来是要换掉blue-penguin这个主题换成pelican-clean-blog或者pure，但事实上每一个主题都有自己的缺陷，当我实在没有办法的时候，终于还是决定继续使用这个主题，并且打开Google搜索“如何制作pelican theme”，然后决定自己修改这个主题，后来发现其实也没有太难，两天的时间基本上都搞定了，下面来说一下我都修改了些什么内容。

那么我们先来看一下，theme的文档结构是怎样的。

```linux
├── static
│   ├── css
│   └── images
└── templates
    ├── archives.html         // 显示归类
    ├── period_archives.html  // 按照日期归类
    ├── article.html          // 文章处理
    ├── author.html           // 作者处理
    ├── authors.html          // 列出所有文章的作者
    ├── categories.html       // 分类汇总
    ├── category.html         // 分类处理
    ├── index.html            // 首页显示所有文章
    ├── page.html             // 页面处理
    ├── tag.html              // 标签处理
    └── tags.html             // 标签汇总，生成云标签
```
>内容引自<http://pelican-docs-zh-cn.readthedocs.org/en/latest/themes.html>，与主题不完全符合，意思理解即可。
####在主页中使用摘要
blue-penguin的主界面是显示全部文章内容，有人说不能使用Pelican不能使用摘要功能，只能截取固定字数的文字作为摘要，其实完全不是，我们先来看一下`templates`文件夹下的`index.html`是怎么写的。
```html
{% extends "base.html" %}

{% block content %}
{% set date = None %}
{% for article in articles_page.object_list %}
{%   if date != article.date.date() %}
{%     set first_article_of_day = True %}
{%   else %}
{%     set first_article_of_day = False %}
{%   endif %}
{%   set date = article.date.date() %}
{%   include "article_stub.html" %}
{% endfor %}

{% include "pagination.html" %}
{% endblock %}
```
注意`{%   include "article_stub.html" %}`,内容显示他直接引用的博客正文的内容，也太懒点了吧，看了看别人都是怎么写的最后做了如下的设计，不能直接改动`article_stub.html`,不然博客文章也都出问题，首先复制`templates`文件夹下的`article_stub.html`，并将其改名为`index_stub.html`，然后将`templates`文件夹下的`index.html`中
```html
{%   include "article_stub.html" %}
```
改为
```html
{%   include "index_stub.html" %}
```
然后去修改`index_stub.html`文件，将
```html
{{ article.content }}
```
改为
```html
{% if article.summary %}
{{ article.summary|truncate(200) }}
{% endif %}
```
如果用markdown里面加入summary，那么主页将显示summary内容，如果没有，将会取文章前200个字符，当然200时通过`truncate(200)`定义的，可以改，这里要注意的是如果食用摘要功能，在正文中需要再写一次，进入正文后无法看到摘要。
除此之外，还有一个重要的点就是在`index_stub.html`中需要删除倒数第二行代码
```html
{% include "disqus.html" %}
```
否则如果添加disqus的评论功能，主页会出现评论项目。
####修复网站标题与home按钮点击无法正常回到主页
上篇文章说过这个问题，我做了一个假的Home，但是假的就是假的，还是想弄成真的，其实真正做起来也没有太难，关键是找对地方。打开打开`templates`文件夹下的`base.html`，找到如下两段代码：
```html
{% if DISPLAY_HOME or DISPLAY_HOME is not defined %}
	<li{% if output_file == "index.html" %} class="selected"{% endif %}><a href="{{ SITEURL }}">Home</a></li>
 {% endif %}
```
以及
```html
<div class="header_box">
	<h1><a href="{{ SITEURL }}">{{ SITENAME }}</a></h1>
		{% if SITESUBTITLE %}
	<h2>{{ SITESUBTITLE }}</h2>
	{% endif %}
</div>
```
第一段对应是Home，第二段对应的是博客的主标题，两个的改法相同，将
```html
href="{{ SITEURL }}"
```
改成
```html
href="../.."
```
就可以了。
在第二段代码中我们还发现了添加副标题的功能，可以在配置文件`pelicanconf.py`中添加一段比如`SITESUBTITLE = u"JonyM's Blog"`效果相当漂亮，谁用谁知道。
####修改底部文字
官方给到到底部文字始终是`Blue Penguin Theme · Powered by Pelican · Atom Feed`如果想添加一些关于自己的标记，版权声明，甚至备案如何做呢？方法是，打开`templates`文件夹下的`base.html`，找到如下代码：
```html
<footer>
	<p>
	<a href="https://github.com/jody-frankowski/blue-penguin">Blue Penguin</a> Theme
	&middot;
	Powered by <a href="http://getpelican.com">Pelican</a>
	{% if FEED_ALL_ATOM %}
	&middot;
	<a href="{{ SITEURL }}/{{ FEED_ALL_ATOM }}" rel="alternate">Atom Feed</a>
	{% endif %}
	{% if FEED_ALL_RSS %}
	&middot;
	<a href="{{ SITEURL }}/{{ FEED_ALL_RSS }}" rel="alternate">Rss Feed</a>
	{% endif %}
</footer>
```
在footer标签中间添加一个p标签，例如：
```html
	<p>
	Copyright © <a href="../pages/about-me">JunMa</a>
	&middot;
	This work is licensed under a
	<a href="http://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>
	</p>
```
至于具体要加什么内容就看你自己喽。

将一个跟自己的想象差很远的主题，做成自己喜欢的样子是一件特别有成就感的事情，添加了Disqus的评论系统和Google的分析系统后，果然变的越来越好用了，不过依然有三个问题没有解决。
1.就是国内访问的速度问题，一直很慢而且经常崩溃。
2.页面无法被baidu抓取到，这不是个技术博客，对我来讲这很重要。
3.字体很丑，有大神用云字体解决这个问题，我也想试试，不过目前还没有成功。

估计接下来可能会选择不在Github而直接转为亚马逊的云主机，慢慢来吧，一切都会顺利的。


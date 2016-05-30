Title: 在Pelican下使用blue-penguin的小心得
Date: 2015-11-21 22:10
Category: 学技能
Tags: 编程

####选择Pelican编写博客都原因
如果你是程序员，看到此文肯定会喷我，我不是程序员，所以里面有大量超级low的操作，你们就忍忍吧，不希望我误导大家，就请在下面的回复里补充，小马感激不尽。

在博客已经算是落后产物的当下，选择做一个博客是个有趣的决定，一方面是因为秉承“做中学”的原则，总觉得既然想学点编程，总不能天天只看教程什么都不做，另一方面也确实发现随着工作年限的积累，需要开始给自己积累些文字性的东西了，没有用像新浪博客这种东西的原因，纯是因为感觉自己编的博客比较diao而已。

选择Pelican的原因是，我只学了点Python，当时小勾推荐用的是Jekyll，即便他说不用学Ruby也能用，但事实上我的电脑死活装不上去，而且完全不知道为什么。我觉得小白们应该和我有一样的感觉，大神轻易部署的环境，到自己的电脑里死活装不上，而且根本不知道为什么。博客部署还是用的Github pages，事实上对于一个非程序员而言，如果不是这个博客，可能一辈子都不知道Github究竟是什么鬼。
####使用的环境
先整体介绍一下我的环境：
#####系统环境：OS X 10.10.5
Win好还是Mac好，虽然乔老爷子仙逝之后，苹果的创造力有所下降，而且微软也出现了Surface这种大牛货，但是不要和Mac用户讨论谁好，Mac的好是弥漫式的，也许仅仅体现在一个字体的现实效果上，而且用Mac，你可以体验到各种神器。当然，为了避免某些尴尬，建议大家安装一个Parallels Desktop。
#####IDE:Xcode
什么是IDE对于小白来说，这就是个问题，引用百度百科的解释：
>IDE集成开发环境（简称IDE）软件是用于程序开发环境的应用程序，一般包括代码编辑器、编译器、调试器和图形用户界面工具。

虽然大牛们会说一句，就这点东西，用文本编辑还不够么，确实不够，越是大神，越能脱离工具的控制，越是小白，越需要工具帮助。Xcode是苹果程序员必备开发软件，本以为这个东西会很大，但是打开单个的html或者markdown文档速度相当的快。
#####markdown文档工具：马克飞象
这是一款专门为印象笔记设计的markdown工具，为什么要用它，真比其他的好用多少么？当然没有，只是在一次会议上，打开马克飞象做记录，周围的人都来问这是什么东西，好吧，虚荣心让我已经离不开他了。
#####GitHub工具：GitHub Desktop for Mac
不是程序员就别老学人家用Terminal操作了，你确定你都知道自己干了些什么么，GitHub Desktop是官方给的工具，直接改文件里的内容，用他更新，棒棒哒。
#####翻墙软件：VPNPP
别问为什么了，不翻下载某些东西的时候就是报错，某些网站就是上不去，某些教程就是查不到，别找免费的了，省点钱不够闹心的。
#####其他的环境：Virtualenv工具、Python 2.7
使用Virtualenv的原因很简单，就是Pelican也出现了死活安不上的问题，用Virtualenv就都解决了，Python2.7是自带的，还有其他的乱七八糟也不知道安了多少插件，教程实在太多了，我这里就不写了，这篇文章重要是写关于blue-penguin的设置问题。
####blue-penguin的设置与使用
先上一张图，这是写这片文章之前的图：
![aimage](/images/1448118648087.png)
有可能大家看到这篇文章的时候，这个博客已经不用这个主题了，为什么我后面会说。

选择这个主题的主要原因是，这个主题很简单，元素少，内容文章显示内容多，符合我的审美情趣，网址是：<https://github.com/jody-frankowski/blue-penguin>
我现在这个阶段用Pelican非常简单，写好文章，存储称为.md格式，放在content文件夹下，然后用Terminal做如下操作
```terminal
cd pelican ＃打开pelican文件夹 
source bin/activate ＃打开Virtualenv
cd jonymblog ＃打开项目文件夹（jonymblog是生成的文件夹名）
make html 
make publish
make serve ＃各种生成，生成完以后就可以登陆localhost:8000登陆
```
如果你复制以上代码，并输入到terminal当中，点击回车，没有报错的话就在output文件夹里面生成了最新的html文件，全部复制并粘贴到与github链接的文件夹中，全部替换，并利用GitHub Desktop更新就好，所以关于Pelican的设置完全就取决于pelicanconf.py这个文件到底是怎么设置的，那我先输出一下我的设置文件，然后一点点说里面这么设置的原因：
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*- #
from __future__ import unicode_literals

AUTHOR = u'Jonym'
SITENAME = u"小马的杂货铺"
SITEURL = 'http://jonym.com'
TIMEZONE = 'Asia/Shanghai'

PATH = 'content'
DATE_FORMATS = {
    'zh_CN': '%Y-%m-%d %H:%M:%S',
}
DEFAULT_DATE_FORMAT = '%Y-%m-%d %H:%M:%S'
DEFAULT_LANG = u'zh_CN'
LOCALE = ('en_US')

# Feed generation is usually not desired when developing
FEED_ALL_ATOM = None
CATEGORY_FEED_ATOM = None
TRANSLATION_FEED_ATOM = None
AUTHOR_FEED_ATOM = None
AUTHOR_FEED_RSS = None
THEME = 'blue-penguin'

DISQUS_SITENAME = u"jonym"

# all defaults to True.
DISPLAY_HEADER = True
DISPLAY_FOOTER = True
DISPLAY_HOME   = False
DISPLAY_MENU   = True

# provided as examples, they make ‘clean’ urls. used by MENU_INTERNAL_PAGES.
#TAGS_URL           = 'tags'
#TAGS_SAVE_AS       = 'tags/index.html'
HOME_URL            = 'index'
HOME_SAVE_AS        = 'index.html'
ABOUT_URL           = 'pages/about-me'
ABOUT_SAVE_AS       = 'pages/about-me.html'
#AUTHORS_URL        = 'authors'
#AUTHORS_SAVE_AS    = 'authors/index.html'
#CATEGORIES_URL     = 'categories'
#CATEGORIES_SAVE_AS = 'categories/index.html'
ARCHIVES_URL        = 'archives'
ARCHIVES_SAVE_AS    = 'archives/index.html'

# use those if you want pelican standard pages to appear in your menu
MENU_INTERNAL_PAGES = (
    #('Tags', TAGS_URL, TAGS_SAVE_AS),
    #('Authors', AUTHORS_URL, AUTHORS_SAVE_AS),
    #('Categories', CATEGORIES_URL, CATEGORIES_SAVE_AS),
    ('Home',HOME_URL,HOME_SAVE_AS),
    ('Archives', ARCHIVES_URL, ARCHIVES_SAVE_AS),
    ('About Me', ABOUT_URL, ABOUT_SAVE_AS),
)
# additional menu items
#MENUITEMS = (
#    ('GitHub', 'https://github.com/'),
#    ('Linux Kernel', 'https://www.kernel.org/'),
#)

# Blogroll
LINKS = (('Pelican', 'http://getpelican.com/'),
         ('Python.org', 'http://python.org/'),
         ('Jinja2', 'http://jinja.pocoo.org/'),
         ('You can modify those links in your config file', '#'),)

# Social widget
SOCIAL = (('You can add links in your config file', '#'),
          ('Another social link', '#'),)

DEFAULT_PAGINATION = 6

# Uncomment following line if you want document-relative URLs when developing
#RELATIVE_URLS = True


```

大家看到#号就知道，这是被我备注掉的代码，正常来讲应该是删除掉，但是难免有一天你又突然发现有些东西需要，所以我基本上不删除代码，作为资深菜鸟，要把用过的代码都放着，万一想再用的时候找不到，就悲剧了。下面一点点说里面重要的设置内容。
#####将blun-penguin设置为主题
从官方下载主题文件到文件夹中，并在pelicanconf.py添加一行代码：
```python
THEME = 'blue-penguin'
```
这样就可以了，主题到文件说明中要求复制如下代码到pelicanconf.py中，先照着做，然后改：
```python
# all the following settings are *optional*

# all defaults to True.
DISPLAY_HEADER = True
DISPLAY_FOOTER = True
DISPLAY_HOME   = True
DISPLAY_MENU   = True

# provided as examples, they make ‘clean’ urls. used by MENU_INTERNAL_PAGES.
TAGS_URL           = 'tags'
TAGS_SAVE_AS       = 'tags/index.html'
AUTHORS_URL        = 'authors'
AUTHORS_SAVE_AS    = 'authors/index.html'
CATEGORIES_URL     = 'categories'
CATEGORIES_SAVE_AS = 'categories/index.html'
ARCHIVES_URL       = 'archives'
ARCHIVES_SAVE_AS   = 'archives/index.html'

# use those if you want pelican standard pages to appear in your menu
MENU_INTERNAL_PAGES = (
    ('Tags', TAGS_URL, TAGS_SAVE_AS),
    ('Authors', AUTHORS_URL, AUTHORS_SAVE_AS),
    ('Categories', CATEGORIES_URL, CATEGORIES_SAVE_AS),
    ('Archives', ARCHIVES_URL, ARCHIVES_SAVE_AS),
)
# additional menu items
MENUITEMS = (
    ('GitHub', 'https://github.com/'),
    ('Linux Kernel', 'https://www.kernel.org/'),
)
```
#####设置日期格式
当大家放入一篇测试文章的时候，发现日期格式不太对啊，月份并没有以英文简写的方式出现，而是以阿拉伯数字的形式出现，不要太丑好吧，虽然也加了很多有用没用的代码在里面，但最后发现，最关键的是在pelicanconf.py添加这行代码。
```python
LOCALE = ('en_US')
```
其实就算括号里什么都不添加，文件找不到后默认还是英文的月份，但没这行就是达不到要的效果。
#####无效的Home解决方案
正常来讲，点击Home或者点击博客名称都应该回到首页，但是自动生成的文件，并没有，网页给到的反馈是反应了一下，但是没有变化，对比官方的博客发现有如下问题，官方的相关html标签内容中是这样设置的：
```html
<h1><a href="../..">Computer Stuff</a></h1>
```
而我们生成的是这么设置的：
```html
<h1><a href="">小马的杂货铺</a></h1>
```
这个href标签的设置也太坑了吧，我也不能产生的所有html页面全都打开改一遍吧，当然据说有人可以对某个文件夹中所有html相同的字段进行修改，如果有这种神器当然好，不过我没找到能用的，于是做了如下的设置：
首先将官方要求的DISPLAY_HOME设置成False
```python
DISPLAY_HOME   = False
```
然后又添加了一个假的Home
```python
HOME_URL            = 'index'
HOME_SAVE_AS        = 'index.html'
```
以及在MENU_INTERNAL_PAGES中添加('Home',HOME_URL,HOME_SAVE_AS),
这样虽然点击博客名称虽然仍然没有解决，但是Home按钮正常了。
>此处肯定有更好的解决方案，只是我不会
#####设置About Me
默认官方生成出来的菜单中有很多选项，但是都没什么用，最关键的About Me竟然没有，我除了把Archives留下了，其他都删掉了。

设置About Me的方法是，首先在content文件夹下新建一个文件夹，该名称为pages，将你要写的内容，制作成.md格式的内容，注意文件开头格式是
```markdown
Title: About Me
Date: 2015-11-15 
Category: About Me
```
并且添加代码：
```python
ABOUT_URL           = 'pages/about-me'
ABOUT_SAVE_AS       = 'pages/about-me.html'
```
以及在MENU_INTERNAL_PAGES中添加 ('About Me', ABOUT_URL, ABOUT_SAVE_AS),
#####添加评论功能
使用DISQUS注册个账号，然后要生成个shortname，并且在pelicanconf.py中添加如下代码就好了，注意jonym是我的shortname：
```python
DISQUS_SITENAME = u"jonym"
```
但是首页的第一篇文章也出现了评论框，目前没解决。
#####部署在顶级域名上
这个事情那其实没什么必要，但是既然做就做彻底：
- 第一步：需要购买个域名，很多文章都推荐使用国外的域名注册网站，我用的万网，阿里云旗下网站，也挺好的啊，也不用备案啊，英文实力不行或者墙总是翻不过去的或者没有国际信用卡的，用这个挺好；
- 第二步：在github里项目的根目录下面添加一个名称为CNAME的文件，注意这个文件连后缀都不能有，里面也只能有一条就是你注册的域名，http和www都不用写；
- 第三步：域名解析，在域名管理里面点击解析，要求输入IP，由于各种教程比较受到时间的影响，这哥们IP换过一次，我解析的时候用的是192.30.252.153，大家感觉有问题就上官网再查一下，有可能又有变化。
#####添加favicon图标
在本机测试的时候，总会告诉我们缺少两个名为favicon的文件.png的和.ico的，这是什么东西呢，其实就是在网址显示的旁边的极小的那个图标，做两个图标，放到根目录下面就好了，.ico的文件貌似需要在Win环境下完成，貌似先要生成.bmp文件，然后改后缀名。
>当然在Mac环境下肯定也有解决方案
#####添加404页面
404页面就是找不到了到那个页面，做一个名称为404.html 的文件然后放到根目录下面就可以了，如果你想要一个和整体主题一样，那就把生成的About Me的html文档复制一下改改就好了。

目前能记得住的就这些坑了，大坑小坑一大堆，慢慢弄，这片文章对我来说纯是写给自己的，技术大哥们，求指导。
####写这篇文章是的原因是不准备使用blue-penguin主题了
blue-penguin到目前我还没真正下定决心换掉，毕竟我很喜欢他的风格，而且花了不少心思的，但是这个blue-penguin主题自身非常不成熟，很多东西确实不知道怎么设置，比如设置在主页中文章的文字显示的长度，一直报错，更重要的是，这个主题并没有采用响应式的交互设计，虽然在手机上登录也没有问题，也挺好看的，接下来有可能换主题，希望喜欢用这个主题的朋友，本文能给到大家一点点的参考。
本文所写时，所对应的文件github网址：<https://github.com/jonym/Pelican-blue-penguin-jonymblog.git>
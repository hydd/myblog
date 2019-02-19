---
title: Mac平台利用Hexo+GitHub搭建个人博客
date: 2019-02-17 20:07:35
tags:
  - Mac
  - Hexo
  - Github Pages
  - 博客搭建
categories:
  - [简单博客搭建]
  - [Hexo使用攻略]

  
---



## **前言**
拥有自己的个人博客，并对自己的日常生活、学习进行记录是一件很酷的事情，因此不久前萌生了搭一个个人博客的想法。搭建博客的话，必不可少的需要一个托管博客的服务器，一个域名，以及一个快速blog生成工具。

托管服务器可以选择自己从阿里云等云服务商购买的诸如ECS或者轻量服务器等虚假服务器，也可以是一些免费或者付费的虚拟空间，或者托管于GitHub Pages或coding等。其中使用第三方服务商提供的托管服务较为便捷，本博客正是托管于GitHub Pages。选择GitHub Pages托管个人博客，顺便附带了GitHub级别的防ddos服务，虽然个人小博客也基本不会遭遇ddos。

域名的话可以选择使用GitHub提供的*.github.io二级域名，当然如果感觉不够酷、不够个性的话可以选择到[freenom](https://www.freenom.com/zh/index.html)注册.tk等免费域名，或者到[阿里云](https://wanwang.aliyun.com/domain/)等域名注册站购买付费域名。关于域名的注册解析将在另一篇文章{% post_link 域名注册、解析、blog绑定 %}展开。

快速blog生成工具中比较火的有Wordpress，Hexo等，其中Wordpress可以实现更多更专业的功能，更多的用在个人服务器上建站来用；Hexo较为轻量简洁，本博客正是基于hexo框架。Hexo可以通过简单的命令实现本地预览功能，并直接发布到WEB容器中实现同步。

## **准备**
### 	托管：[GitHub Pages](https://pages.github.com/)
###  	域名：[freedom](https://www.freenom.com/)
### 	框架：[Hexo](https://hexo.io/zh-cn/)
### 	主题：[NexT](https://theme-next.iissnan.com/)

## **创建GitHub博客**
要使用 GitHub Pages，首先你要拥有自己的 [GitHub](https://github.com/) 账号，若未注册GitHub账号点击链接注册即可。
### **新建托管博客的仓库**
新建仓库名称必须为user.github.io，如hydd.github.io。

![](./1.png)

创建完成之后可以直接进行第3部配置本地博客环境，当然若不需要的话也可以在Settings中进行一些设置，相关内容将在之后进行介绍。

![](./2.png)

## **搭建本地博客环境**
### **安装Node.js和Git**

Mac上安装可以选择图形化方式或终端安装，此处使用终端安装。通过终端安装需要mac已经安装包管理工具[homebrew](https://www.jianshu.com/p/de6f1d2d37bf)，若不确定是否安装的话可以通过在终端执行命令`brew -v `来查看。若结果如下所示，即homebrew已经安装。

![](./3.png)

若未安装homebrew的话，通过在终端执行以下命令进行安装，完成后再次使用`brew -v`命令查看。

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Brew安装完成之后，通过以下命令分别安装Node.js和Git

```shell
brew install node
brew install git
```

安装完成后，通过在终端执行如下命令测试Node.js和npm
```shell
node -v
v11.9.0

npm -v
6.8.0
```

测试git

```shell
git --version
git version 2.20.1
```

### **安装hexo**
Node.js和Git都安装成功后安装Hexo。安装时加上sudo防止出现权限问题，其中-g表示全局安装。

```shell
sudo npm install -g hexo
```

若安装过程出错，使用以下命令安装：

```shell
sudo npm install --unsafe-perm --verbose -g hexo
```

安装完成后使用`hexo -v`或者`hexo version`可以成功看到版本号即安装完成。

### **博客初始化**
通过folder或者终端mkdir命令创建存储博客的文件夹，如通过终端在终端默认目录通过mkdir命令创建test文件夹(`~/test`，文件夹名称自定义即可，后续所有test均指我所创建的博客文件夹，替换成自己的文件夹名称即可)，创建完成后进入test文件夹。

```shell
mkdir myblog
cd myblog
```

初始化本地博客：

```shell
hexo init
```

安装npm：

```shell
sudo npm install
```

配置完成后需要通过`hexo g`和`hexo s`(`hexo s --debug`可以开启调试)命令来处理markdown文件生成html文件并开启本地服务器，之后后经常用到这两个命令。成功之后根据终端提示访问<http://localhost:4000/>即可看到默认界面。
```shell
#常用命令解释
hexo g  #生成缓存和静态文件
hexo s  #开启本地服务器
hexo d  #重新部署到远程服务器
hexo clean	#清除之前生成的缓存和静态内容，即缓存文件(db.json)和站点根目录的静态文件夹(public)
```

![](./4.png)

## **将本地博客与GitHub进行关联**
使用第2步已经创建好的存储仓货，复制对应的web url，如图：

![](./5.png)

复制完成之后，修改本地的配置文件。有两种途径，一种是通过终端进入test文件夹，通过vim命令打开并编辑_config.yml（记为**站点配置文件**，后续所有**站点配置文件**均指代该配置文件）文件；另一种方法是在本地找到test文件夹，然后通过文本编辑器进行编辑，个人推荐通过vs code进行编辑。
配置文件文件打开之后，直接跳转到文件末尾，修改deploy如下，其中repository属性值使用刚才复制的web url，注意冒号后边需要加空格。

```yml
deploy:
  type: git
  repository: https://github.com/hydd/hydd.github.io.git
  branch: master
```

编辑完成并保存之后，在终端打开站点根目录（指~/test），并先后运行`hexo g`和`hexo d`生成静态html文件并同步到GitHub仓库。

若执行`hexo g`或`hexo d`命令出错，则执行以下对应命令即可解决。

```shell
#hexo g
npm install hexo --save

#hexo d
npm install hexo-deployer-git --save
```

若未绑定GitHub，则需要在执行`hexo d`时输入提示的GitHub账户相关信息。

上传完成之后即可通过访问*.github.io看到默认的博客，和之前本地服务器看到的内容一样（后期如果出现同步之后服务器端未显示新内容的话，可以在执行`hexo g`命令之前执行`hexo clean`清除本地的缓存文件）。

根据一些教程，可以通过添加ssh key到GitHub避免每次更新博客再输入用户名和密码，本人暂未遇到相关情况，因此暂不介绍。

如果只是搭建一个简单的个人博客，那么到这里基本就可以了。之后的主题更换等内容可以后续再关注即可。

## **更换并配置hexo主题**
Hexo默认的主题相对不够简洁吸引人，因此我选择使用人数比较多的Hexo-theme-next主题，该主题简洁而不失丰富。next有四种主题选择，分别为Muse、Mist、Pisces、Gemini四种，个人选择的是Pisces主题通过修改_config.yml配置文件（位于 `～/test/themes/next/_config.yml`，记为**主题配置文件**，后续所有**主题配置文件**均指代该文件，切勿与**站点配置文件**混淆）即可。可以为主题增加标签、分类、归档、喜欢、文章阅读统计、访问人数统计、评论等功能，本人暂时只开启了标签，分类，归档，喜欢，博客界面如下所示。

![](./6.png)

### **安装主题**
终端通过cd命令进入博客根目录（即~/test目录），然后执行以下命令：

```shell
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

完成后修改**站点配置文件**中的theme字段的名称landscape更改为next。

修改完成之后执行`hexo g`和`hexo s`即可在本地预览更新，或直接执行`hexo g`和`hexo d`命令直接生成并部署到服务器上。之后每次修改更新都需要使用其中一组命令来进行操作。

### **启用标签、分类、归档页**

首先取消注释**主题配置文件**中menu模块中`tags`、`catagories`、`archive`字段。接下来以增加标签页功能为例。

首先通过如下命令创建tags标签界面；
```shell
hexo new page 'tags'
```

创建完成后会发现test/sources中生成了一个tages文件夹，打开该文件夹，修改index.md内容(可使用vs code进行修改，也可以使用专业markdown编辑器进行修改，我使用的是typora，所见即所得)，新增type字段如下：
```markdown
title: 标签
type: tags
date: 2019-02-16 19:21:13
```

修改完成后使用`hexo g`和`hexo s`命令即可以本地预览，发现多出了标签界面，之后即可以同步到服务器端。catagories（分类）及archive（归档）同理操作即可。

### **增加喜欢**栏

喜欢界面用于展现自己喜欢的书籍和电影，通过图片流的形式进行展示。为了实现该标签，首先要在**主题配置文件**中menu模块新增favorites字段，如下：
```yml
menu:
  home: / || home
  #about: /about/ || user
  favorites: /favorites/ || heart
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
```

然后从从 [GitHub](https://github.com/hydd/myblog/tree/master/themes/next/scripts) 中下载image-stream.js，放到对应的 next/scripts目录中。

之后即可以修改source/favorites/index.md文件，使用插件自定义的两个模版来生成页面，格式如下：

```markdown
{% stream %}
{% figure  [https://img3.doubanio.com/lpic/s6509536.jpg](https://img3.doubanio.com/lpic/s6509536.jpg)  《悟空传》%}
{% endstream %}
```
![](./7.png)
保存并使用生成命令之后就可以在本地看到效果图：

![](./8.png)

### **其他功能**

诸如数据统计与分析，评论系统，阅读统计，内容分享服务等功能暂未添加，有待后续实现，也将在系列博客进行介绍。

## **个人域名绑定**
当前博客域名为GitHub提供的二级域名，如想要绑定自己的域名，按照本节操作即可，若无该方面需求可以跳过。本文默认读者已经拥有一个自己的域名，并且可以正常解析。如果尚未拥有自己的域名，阅读{% post_link 域名注册、解析、blog绑定 %}即可。

### **创建CNAME文件**

终端cd到next主题中的source文件夹，然后使用touch命令创建无后缀的CNAME文件，之后通过vim操作将自己的域名添加进去即可。不过要注意如果是中文域名的话，要先将中文域名转换为[**Punycode标准编码的字符串**](http://www.webmasterhome.cn/tool/punycode.asp)，然后使用转换之后的域名即可。
![](./9.png)

### **域名解析**

使用ping命令获取当前博客对应域名所处的IP，如图：

![](./10.png)

获取到对应IP之后修改域名解析到对应IP即可。

## **搜索引擎优化**
通过搜索引擎优化可以提高博客内容被搜索引擎的爬取次数。如果对此无需求的话可以跳过本部分，希望自己的博客更容易被搜索到的话可以进行相关操作。由于Github博客屏蔽百度爬虫（似乎，挺好的？），所以本文以Google为例，bing等搜索引擎优化类似。

### **确认收录情况**

在Google中进行如下搜索，当然域名需要替换成你自己的，如果结果和我类似，那么就是该域名暂未被收录。

![](./11.png)

### **网站归属验证**

进入谷歌站长平台中的搜索引擎提交入口，添加域名，选择验证方式，个人选择的是HTML标记验证，如图：

![](./12.png)

编辑**主题配置文件**，将google_site_verification字段的属性值使用获取到的content属性值替换即可。

```
google_site_verification: XXXXXXXXXXXXXXXXXXXXXXX
```

完成保存之后使用`hexo g`和`hexo d`更新博客内容，点击上图的验证按钮即可完成域名归属验证。必应搜索操作类似。

### **其他设置**

完整的Google收录还需要添加站点地图，本博客站点地图暂时有问题，暂未添加，后续更新。添加成功之后等待1天后通过7.1操作接可以搜索到博客内容。

## **后续更新**
本篇博客简单的介绍了Mac平台利用hexo和GitHub pages搭建属于自己的个人博客，至于如何进行博客的创作并发布以及更多的hexo基础及进阶玩法将在后续系列博客更新。当然，更重要的是通过blog记录日常的学习生活。









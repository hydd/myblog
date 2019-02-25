---
title: ss配置文件转换-生成clash配置文件
date: 2019-02-24 14:07:49
tags:
	- ss
	- clash
categories:
	- 资源分享
---

用过surge的朋友都知道规则匹配的魅力,可以设置特定的网站走特定的节点，以及其他种种，这是ss（ssr）客户端的pac规则匹配所无法媲美的。但是surge只支持ios以及mac，同时高昂的售价让很多人止步。幸运的是，现在已经有了新的开源项目——clash，可以理解为开源版的surge，同样基于规则匹配，并且支持[Mac](https://github.com/yichengchen/clashX)和[win](https://docs.cfw.lbyczf.com/)双平台，win平台也终于有一款好看、好用的代理客户端。但是clash当前并不面向小白用户，一些新手在设置配置文件的时候一筹莫展，不知道如何将当前的ss节点信息按照clash的规则进行配置。本文正式为了解决这个问题，分别介绍三种方法。

**注意：clash只支持ss和v2ray，不支持ssr**

所需脚本文件均来自个人[GitHub](https://github.com/JRQLS/ToClash)，当前属于beta版，会持续更新，请关注并收藏本文获取后续最新消息！

## 通过ss订阅生成clash配置文件

首先进入[**SS_clash(from ss subscription)**](https://github.com/JRQLS/ToClash/tree/master/) 文件夹，下载**SS_clash.py**文件，该脚本文件实现通过ss订阅链接生成clash配置文件。
下载该文件之后，通过文本编辑器（如VS code）打开该文件，将文件末尾处如下地址替换为自己的订阅链接，然后执行脚本便可在脚本文件夹得到clash配置文件（如何执行python脚本请自行Google）。

![](./1.png)

## ss本地配置文件转换为clash配置文件

个别小机场并没有提供ss订阅，就需要通过ss本地文件转换的方式得到clash配置文件。
进入[**SS_clash(from ss Local configuration file)**](https://github.com/JRQLS/ToClash/tree/master) 下载整个文件夹，然后将自己的ss配置文件（可以通过ss客户端导出，为json格式）重命名为export.json，替换文件夹中默认的export.json

然后运行**SS_clash.py**脚本即可在当前文件夹得到clash配置文件。
如果本地配置文件包含了多个机场，但是只想得到某个机场的clash配置文件的话，将脚本最后的` nodes = getallNodes(file)`注释，同时将`# nodes = getGroupNodes("MySSR", file)`取消注释，并修改MySSR为所需group名称，运行脚本即可。

## 将surge本地配置文件转换为clash配置文件

如果机场提供了surge托管的话，可以使用F大提供的转换链接即可（将复制下边链接中并将其中的http://example.com 替换为自己的托管地址，然后将新链接复制到clash订阅填写更新即可)

<https://tgbot.lbyczf.com/surge2clash?url=http://example.com>

但是部分机场（如老板娘）只提供了surge 的配置文件，但是并未提供surge的托管地址，通过本脚本可以得到clash配置文件。
和ss本地文件转换类似，下载[Surge_clash(local) ](https://github.com/JRQLS/ToClash/tree/master/) 文件夹，将surge配置文件放入该文件夹并重命名为surge.conf，然后运行**surge_clash.py**脚本即可。




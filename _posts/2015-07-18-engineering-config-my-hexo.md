---
layout: post
categories: Engineering
title: "配置我的HEXO"
tags: [Engineering, Blog Platform]
date-string: July 18, 2015
---

## 参考
 - http://hexo.io/
 - http://ibruce.info/2013/11/22/hexo-your-blog/

## 注意
执行hexo命令出现错误：
 
```
/usr/bin/env: node: No such file or directory
```
 
解决方法：

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

另外，对于为了绑定个人主页域名（例如我的liweiwu.com）,
需要修改_config.yml中的url字段。否则可能会出现从自己的域名无法访问的情况。

## 主题

主题选择：知乎家顺张给出了一个简单的star排行榜。目前来看最好的主题是next。next主题需要手动添加categories， tags。

参考：

 - http://www.zhihu.com/question/24422335
 - http://blog.idhyt.xyz/2015/05/11/4-android-reverse-clickbutton/
 - https://github.com/iissnan/hexo-theme-next/wiki/%E5%88%9B%E5%BB%BA%E5%88%86%E7%B1%BB%E9%A1%B5%E9%9D%A2

## 托管
托管到gitcafe： 在_config.yml添加

```
deploy:
  	type: github
  	repository: git@gitcafe.com:wlwchina/wlwchina.git
  	branch: gitcafe-pages
```

version 3 的问题：

```
github --> git 
npm install hexo-deployer-git --save
```

## markdown编辑器
Linux下推荐: Haroopad 是一款覆盖三大主流桌面系统的编辑器，支持 Windows、Mac OS X 和 Linux。 主题样式丰富，语法标亮支持 54 种编程语言。该工具重点推荐 Ubuntu/Linux 用户使用，从此可以告别 gedit 加 Markdown 插件这种工作方式了。 
本文使用Haroopad编写。Mac上的Markdown也很好用。

参考：
 
- http://pad.haroopress.com/user.html
- http://code.csdn.net/news/2819623
 

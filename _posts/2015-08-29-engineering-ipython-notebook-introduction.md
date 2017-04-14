---
layout: post
categories: Engineering
title: "iPython Notebook Introcduction"
tags: [Engineering, Coding]
date-string: May 29, 2015
---

今天才知道ipython及其notebook这个强大的东西，相见恨晚啊! 这里简单介绍ipython及其notebook的安装和使用。个人觉得ipython使得python的命令行交互更加方便，而其强大的notebook更是强大到直接把浏览器变成一个mathmatica！ipython notebook设计目的是让数据分析更容易分享和再生，目前用它来给科研做详细记录、设计教学模型以及与他人合作、其科学家用户已越来越多。ipython notebook保存的文件后缀名是.ipynb，这是一种文本文件，所以可以方便进行版本控制(例如git)。

<center>
    <img src="http://7xkiab.com1.z0.glb.clouddn.com/ipython_notebook_show.jpg" width="500">
</center>

## install ipython
```
sudo apt-get install ipython
```

```
wuliwei@wulw:~$ 
wuliwei@wulw:~$ ipython
Python 2.7.6 (default, Jun 22 2015, 17:58:13) 
Type "copyright", "credits" or "license" for more information.

IPython 1.2.1 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: a = 10

In [2]: a
Out[2]: 10

In [3]: exit
wuliwei@wulw:~$ 
```

## run *.py
```
wuliwei@wulw:~/code/python$ 
wuliwei@wulw:~/code/python$ ls
hello.py
wuliwei@wulw:~/code/python$ ipython
Python 2.7.6 (default, Jun 22 2015, 17:58:13) 
Type "copyright", "credits" or "license" for more information.

IPython 1.2.1 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: run hello.py
hello

In [2]: exit
wuliwei@wulw:~/code/python$ 
```

## ipython的Tab completion
```
In [1]: from sys import std
stderr  stdin   stdout 
```

## IPython Notebook
IPython Notebook是web based IPython封装，但是可以展现富文本，使得整个工作可以以笔记的形式展现、存储，对于交互编程、学习非常方便。

安装:
```
sudo apt-get install ipython-notebook
```

打开命令行，切换到某个目录下，输入ipython notebook。它会启动服务器，并打开浏览器。它会自动读取该目录下面的.ipynb文件.并显示。如果要新建一个文件的话 点按钮New notebook就好了。

用过Mathmatic的人应该一下子就明白了，这就是把浏览器编程Mathmatic了呀！

这里需要明白一个简单的概念--cell, 显示的页面除了菜单栏和快捷按钮, 下面的部分都是由各个cell组成, 默认的cell是code, 所以可以在In[1]后面写python代码, 然后按shift+Enter来执行。还可以设置cell为其他类型: Markdown, raw text, Head... 。同样的, 例如选中一个cell, 设置为Markdown之后, 按shift+Enter就可以渲染处markdown的效果。

<center>
    <img src="http://7xkiab.com1.z0.glb.clouddn.com/ipython_notebook.jpg" width="500">
</center>

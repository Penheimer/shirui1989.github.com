---
layout: post
title: "Octopress在Archlinux中代码高亮问题"
date: 2013-02-01 19:08
comments: true
categories: [博客相关, octopress, python]
keywords: python , octopress , archlinux , 代码高亮 , rake generate , 错误 ,pygments
---
<!--more-->
关于自带代码高亮的问题
----------------------
自带代码高亮是用
``` sh
    ``` [lang] [filename]
    ```
```
的形式来解决的，但是在Arch下`rake generate`总会报错，经查是Python版本的问题，代码高亮使用的是python2但是arch下默认的python是python3，经过一番折腾终于解决了问题。          
首先安装[python-virtualenvwrapper](https://wiki.archlinux.org/index.php/Python_VirtualEnv)：
``` sh
$ sudo pacman -S python-virtualenvwrapper
```
然后进行如下配置（本人zsh）：
``` sh zsh.sh
$ export WORKON_HOME=$HOME/.virtualenvs >> ~/.zshenv
$ mkdir -p $WORKON_HOME
$ source /usr/bin/virtualenvwrapper.sh
$ mkvirtualenv -p python2.7 --distribute blog_env
```
之后把`source /usr/bin/virtualenvwrapper.sh`加入到`~/.zshrc`里面确保每次zsh启动都能运行。      
使用方法：
``` sh
$ cd <path-to-your-blog-dir>
$ workon blog_env
$ bundle exec rake generate
```

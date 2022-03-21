---
layout: post
title:  "github搭建博客"
category: web
tags: [github, hexo, 博客]
---
# github搭建博客
## github搭建博客指南参考链接：
[参考链接: https://zhuanlan.zhihu.com/p/35668237](https://zhuanlan.zhihu.com/p/35668237)

## github访问链接
[链接：https://github.com/liiuzhanchen/xiaomangguo.github.io/settings/pages](https://github.com/liiuzhanchen/xiaomangguo.github.io/settings/pages)
## 注意事项：
1. 下载安装的nodejs版本要大于12.3
2. 生成密钥SSH key 的时候要注意生成的文件的路径
会让你指定生成的文件名，如果不指定，会生成到默认的文件夹中**(C:\Users\Zhanchen.Liu\.ssh)** id_rsa.pub，可以用everything这个软件查找。
3. 在github上生成ssh时要将本地的id_rsa中的密钥拷贝进去。长的类似这样 *ssh-rsa AAAAB3NzaC1yc2EAAAAD.......*

## 相关工具以及安装包参考链接：
1. [nodejs官网](http://nodejs.cn/) 一般不下载最新的。点击下载，找到所有下载选项，找到想要的版本。一般windows上下载 **.msi** 文件安装。
2. [Atom](https://atom.io/) 下载markdown编辑器。这个编辑器不仅可以写markdown语法。还可以写js+h5

<!-- more -->

## 常用的命令行
安装hexo(博客的模板框架)
``` bash
$ npm install hexo-cli -g
```
***
创建博客
``` bash
$ hexo init blog
```
***
安装博客依赖项
``` bash
$ ncd blog
$ npm install
```
***
生成静态页面
``` bash
$ hexo g
```
***
启动博客服务
``` bash
$ hexo s
```
***
提交代码
``` bash
$ hexo d
$ hexo d -m "更新内容"
```
***
创建博客
``` bash
$ hexo new post "blog"
```
***
链接github与本地
``` bash
$ git config --global user.name "liiuzhanchen"
$ git config --global user.email "liuzhanchen1987@163.com"
$ ssh-keygen -t rsa -C "liuzhanchen1987@163.com"
$ ssh -T git@github.com
```
***
## 修改_config.yml
```
deploy:
  type: git
  repository: git@github.com:liiuzhanchen/xiaomangguo.github.io
  branch: main
```

---
title: hexo+travis+github博客环境搭建（一）----本地hexo环境搭建  
date: 2017-02-14   
categories: web   
tags: hexo   
comments: true   

---

### 前言
作为一个不懂前端、不懂css、不懂git的人来说，搭建一个博客真的是太折腾，经过几天在一篇篇网文的指导和说明下，终于搭建完毕，第一篇博客当然要对这几天折腾进行一下总结梳理。

### 环境准备   

Hexo：是一个快速、简洁且高效的博客框架。
travis：自动构建静态页面，并push回github。   
github：放置博客源文件和静态页面文件   

<!-- more -->


### 详细的搭建过程   
一、本地博客环境搭建      
二、上传源文件至github      
三、使用travis自动发布   

#### 本地博客环境搭建
git下载：https://git-scm.com/   
node.js下载：https://nodejs.org/en/


选定相应的版本安装即可    


安装完毕后，在D盘下创建文件夹（blog）存放博客文件

在开始程序中找到Git Bash   
```
# 进入blog目录   
cd /d/blog   

# 安装hexo   
npm install -g hexo-cli   

# 初始化   
hexo init hexo   
```  

初始化结束后，会创建一个hexo的文件夹，进入hexo目录   
```   
cd hexo   

# 生成静态文件            
hexo generate   

# 安装发布插件   
npm install hexo-deployer-git --save   

# 启动hexo server服务器   
hexo server   
```   
![安装完毕](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-001.jpg)      

打开浏览器，输入http://localhost:4000   

至此本地的hexo博客就可以了，但这是本地的，只有自己看，怎么给别人看呢，先别急，咱门再调整调整，默认的博客主题太丑，安装个next的主题。   

安装   
```   
cd /d/blog/hexo   
git clone https://github.com/iissnan/hexo-theme-next themes/next   
```   

修改d:/blog/hexo/_config.yml配置项theme：   
```   
theme: next
```   
       
保存退出   

再次运行   
      
```   
hexo server   
```   

打开浏览器，输入http://localhost:4000，可以看到页面的主题已经发生了改变      
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-002.png)        
关于hexo的详细设置请参考： https://hexo.io/zh-cn/docs/configuration.html      
关于next主题详细的设置参考：http://blog.csdn.net/willxue123/article/details/50994852     


至此本地的blog系统就可以工作了，自己写自己看，这有啥用呀，自己看的同时要分享给别人看。 

####  上传源文件至github         

删除hexo默认主题的目录：d:/blog/hexo/themes/landscape/
删除next主题的本地仓库目录：D:\blog\hexo\themes\next\.git，D:\blog\hexo\themes\next\.github 

创建本地仓库   

```   
# 初始化仓库     
git init   

# 提交暂存区   
git add .   

# 提交   
git commit -m "u00.1"   
```   

至此本地仓库创建完毕。

就是要将本地的仓库push到github，之前要做一些准备工作。
首先需要注册github的账号,http://gihub.com，自己注册，注册完毕后，会发一封邮件到注册的邮箱需要激活。

##### 创建仓库   
使用注册的账号登录后
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-003.png)  

![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-004.png)  



##### 生成访问github的密钥   
在本地git bash中

ssh-keygen -t rsa -C "thikfly@163.com"

一路回车后，在用户目录下生成2个文件id_rsa(私钥)和id_pub(公钥)，我们需要把公钥内容输入到github网站   
点击右上角用户信息      
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-005.png)     
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-006.png)     
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-007.png)   
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-008.png)   

密钥添加完毕。   

##### push本地仓库到github   
我们将本地仓库的master分支push到github仓库的sources分支   

```   
# 配置push的信息   
git config user.name "flyivso"   
git config user.email "thikfly@163.com"  

# push本地仓库
git push git@github.com:flyivso/flyivso.github.io.git master:sources   
```   
   
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-009.png) 

![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-010.png)    

github上已经有上传的文件了。           

#### 使用travis自动构建静态文件并发布   
-创建travis访问github的access token   
-设置travis更新   
##### 创建access tonken   
登录github，在用户设置页面   
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-016.png)   
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-017.png)
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-021.png)     
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-018.png)    
记下生成的字符串，等会要用到   

##### 设置travis
使用travis构建需要用到.travis.yml文件，在hexo目录内创建文件
```   
language: node_js
node_js: stable

# S: Build Lifecycle
install:
  - npm install

before_script:
  - npm install -g gulp

script:
  - hexo g

after_script:
  - cd ./public
  - git init
  - git config user.name "flyivso"
  - git config user.email "thikfly@163.com"
  - git add .
  - git commit -m "Update Posts"
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master
# E: Build LifeCycle

branches:
  only:
    - sources

env:
 global:
   - GH_REF=git@github.com:flyivso/flyivso.github.io.git
```   

浏览器打开travis主页https://travis-ci.org，使用github的账号登录   
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-011.png)    
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-012.png)  
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-013.png)  
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-014.png)  
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-015.png)   
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-016.png)  
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-020.png) 
![next](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/hexo/hexo-019.png)  
这里会用到access token   

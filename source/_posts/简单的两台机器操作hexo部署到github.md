---
title: 简单的两台机器操作hexo部署到github
date: 2017-03-06 18:38:52
tags: [hexo, git, github]
---
在本地安装完成hexo之后，包括主题已更换完毕之后，将生成文章的代码和hexo 开发环境源码一同上传到github上进行托管，方便换机器管理开发

## 在yourname.github.io 项目下新增dev分支，后用于保存hexo开发环境源码。该笔记默认你已经安装完成hexo环境 并且运行显示正常

#### 在一台机器上下载你新建的仓库

``` bash
$ git clone git@github.com:youname/yourname.github.io.git
```

#### 进入该仓库，检查分支情况，如没有除master分支外其他分支，则创建新分支并自动切换至改分支

``` bash
$ git branch -a		//检测该分支情况
```
``` bash
$ git checkout -b dev origin/dev		//创建本地新分支并自动切换至该分支
```
安装依赖
```
npm install
```

#### 将新机器上的新开发改动的 hexo 开发源码拷贝至该目录并上传

``` bash
$ git push origin dev		
```

#### 切换回master 分支，本地仓库内此时应该就是master 分支里的内容，然后生成发布

``` bash
$ hexo g -d	
```

#### 此时的仓库分支结构

![分支结构](用Markdown写Markdown笔记/mulu.png "仓库分支结构")


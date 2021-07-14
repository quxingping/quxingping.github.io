---
title: hexo
tags:
  - hexo
categories:
  - node
date: 2021-05-19 11:22:12
---

## 部署

参考官方文档

## 更新提交到GitHub

***解决在不同电脑更新博客***

利用git分支实现，hexo生成的静态文件放到master分支，hexo源文件放到hexo分支上。直接git clone hexo 分支。

#### 创建分支

在GitHub的仓库（quxingping.github.io）新建一个分支，并切换到该分支，并在仓库设置中Branches-Default branch中将默认分支设置一个分支名，保存。

#### 首次提交到分支

```shell
git clone 分支
git branch 确认分支正确
将本地部署的文件全部拷贝进username.github.io
进入username.github.io文件下，将全部文件提交到分支
```

**注意：**
将themes目录中的.git目录删除，一个仓库中不能包含其他git仓库，否则会失败。

**提交：**

```shell
git add .
git commit -m "hexo files"
git push
```

## 更新维护

1. clone 分支到本地。

2. 进入username.github.io目录，npm install 

3. 编辑后提交源码。

4. 更新博客。

   ```shell
   hexo d -g 或者需要hexo clean
   ```


**注意：**

在更新时先git pull 一下。




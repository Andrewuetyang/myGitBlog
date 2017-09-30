---
layout: single
title:  "git常用命令集"
date:   2016-07-10 19:04:08 +0800
categories: git
---

## git拉取提交（一般开发流程）

### 1. 克隆版本，切换到分支，切换创建本地分支

```
git clone .......
git checkout develop
git checkout -b dev

```
### 2.提交

```
git add .(较高版本 git add -A) // 添加改动文件
git commit -m '备注' // 提交到目前分支（自己建的本地分支）
git checkout develop // 切换到develop分支
git merge dev // 将dev本地分支的内容merge到develop分支
git push origin develop:develop // push到远程develop分支

```
### 3. 拉取

```
git checkout develop  // 切换到develop分支
git pull origin develop // 拉取更新 低版本git pull就可以
git checkout dev // 切换回dev分支
git rebase develop // 将develop分支的内容同步到dev分支

```
### 4.版本穿梭

```
git reflog // 版本历史查看
git reset --hard // 加上版本号，就可以切换版本了

```

### 5. 查看当前分支状态

```
git status 查看当前状态 
6.拉取其他分支的某个点
git cherry-pick // 加上版本号，就行 有冲突就手动解决冲突
```

### 7. 删除分支
```
git branch -D ..
```
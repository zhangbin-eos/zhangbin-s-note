> git 本地配置

# 一. 概述

主要内容

* 配置文件位置
* 基本设置
* 忽略文件设置
* 钩子的使用

# 二. 配置文件位置

## linux 中的配置文件位置

## Windows中的配置文件位置

### 全局配置路径

一般都在 你自己的User目录下

> C:\Users\Administrator\.gitconfig

使用文本编辑器打开\*\*.gitconfig\*\*文件,如下

```
[user]
	name = zhangbin_eos
	email = zhangbin.eos@foxmail.com
[gui]
	fontui = -family \"DejaVu Sans Mono\" -size 9 -weight normal -slant roman -underline 0 -overstrike 0
	fontdiff = -family \"DejaVu Sans Mono\" -size 10 -weight normal -slant roman -underline 0 -overstrike 0
	encoding = utf-8

```

### 局部配置 

局部配置仅仅针对某个仓库,配置文件位于某个具体的仓库之中\*\*.git/config\*\* 比如\lig\_mtrx\_dll.git\config 记录了当前分支和远程库url等

```
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	ignorecase = true
[remote "origin"]
	url = ssh://git@192.168.20.39/~/2017/lig_mtrx_dll
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master


```

# 三. 基本的配置

# 四. 忽略文件设置

## 全局设置

## 针对仓库的设置

## 设置忽略文件时注意

## 参考

# 五. 钩子的使用




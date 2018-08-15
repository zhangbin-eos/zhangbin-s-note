> git 本地配置

# 一. 概述

主要内容

* 配置文件位置
* 基本设置
* 忽略文件设置
* 钩子的使用

# 二. 配置文件位置

## linux 中的配置文件位置

linux中,git的配置文件分为:全局配置文件,用户配置文件,仓库配置文件

用户目录下的.gitconfig文件是用户级的配置文件





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
我的配置文件
```
[user]
	name = zhangbin-eos
	email = zhangbin.eos@foxmail.com
[commit]
        template = C:/Users/admin/.commit-msg

[gui]
	fontui = -family \"DejaVu Sans Mono\" -size 9 -weight normal -slant roman -underline 0 -overstrike 0
	fontdiff = -family \"DejaVu Sans Mono\" -size 10 -weight normal -slant roman -underline 0 -overstrike 0
	encoding = utf-8
[merge]
	tool = P4Merge
        
[mergetool "p4merge"]
        cmd = p4merge \"$BASE\" \"$LOCAL\" \"$REMOTE\" \"$MERGED\"

[diff]
	tool = P4Merge
#-----------------------------------
#别名
[alias]
        co = checkout
        ci = commit
        br = branch
        st = status

        p       = push # Push you changes to a remote
        ba      = branch -a # List both remote-tracking branches and local branches.
        bd      = branch -d # Delete a branch only if it has been merged
        bD      = branch -D # Force delete a branch
        dc      = diff --cached # Display the staged changes查看 git 的提交状态
        lg      = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
        last    = log -1
        unstage = reset HEAD #add file 
        me      = merge
        me-nolog= merge --no-log 
        me-sq   = merge --squash
        upremote= fetch origin
        reclean   = reset --hard HEAD
        unsave  = checkout -- #file 

[winUpdater]
	recentlySeenVersion = 2.18.0.windows.1

```
# 四. 忽略文件设置

## 全局设置
在全局配置文件路径下编辑一个.gitignore文件即可
## 针对仓库的设置
在仓库的根目录下创建.gitignore,并添加追踪
## 设置忽略文件时注意
/file.xxx
filen.xxx
/dir/filen.xxx
filen.*
*.filen.xxx

## 参考

# 五. 钩子的使用

# 六. 自动补全

linux 下 git 的补全方法在git源码目录下,有个git-completion.bash文件,这个是git自动补全的脚本,把它放在/etc/profile.d里,再执行. /etc/profile就行了(注意这条命令是点空格/etc/profile)

```
if [ -f ~/.git-completion.bash ]; then
. ~/.git-completion.bash
fi 
```



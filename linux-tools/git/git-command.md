# git command

## git fetch origin 远程分支名:本地分支名
获取远程分支

## git checkout -- . 
## git checkout --  < file >
弃工作区的改动.

## git reset HEAD  < 文件 >... 
    取消暂存
## git remote prune origin
    移除失效的本地的远程追踪分支

## git diff 
    比较的是工作区和暂存区的差别

## git diff --cached 
    比较的是暂存区和版本库的差别

## git diff HEAD 
    可以查看工作区和版本库的差别

---
## git log --pretty=format:"%an-%h-%s-%H" -1
    -1是表示最近一次提交，就是最新的提交记录
关于%an %h %s %H的含义，可以看下面，还有更多的字段信息，请移步到git log介绍吧~
```
选项	说明
%an	    作者（author）的名字
%h	    提交对象的简短哈希字串
%s	    提交说明
%H	    提交对象（commit）的完整哈希字串
```
## git log \-L \<start\>,\<end\>:\<file\>, \-L :\<funcname\>:\<file\>
    用于查找函数的变更
---



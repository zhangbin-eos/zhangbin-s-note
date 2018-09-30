# git管理keil工程

## git的安装

安装Windows版本的git

- [git-base shell](https://git-scm.com/downloads)
- [git 文档获取](https://git-scm.com/doc)
- [git 图形界面客户端](https://git-scm.com/downloads/guis)

其中图形界面客户端不是必须,详细安装过程参考附件<git 在Windows上的安装和使用实践\>

## 初始化keil工程

1. 新建工程文件夹(名字不要有空格和中文)
2. 组织目录组织目录结构,添加代码,并新建或者拷贝keil工程
3. 添加README.md 文件
4. 添加git忽略文件(.gitignore),文件内容参考下文
5. 使用`git init `命令初始化仓库,然后添加(`git add .`)文件跟踪,并进行第一次提交(`git commit `)

> 建议: keil工程的工程文件放在整个工程的根目录下,方便寻找

> 提示: 第一次提交可能有不完善的地方,进行修改后使用`git commit --amend`命令修改第一的提交


**[keil C工程的忽略文件]**

文件内容来自[https://www.gitignore.io/api/keil](https://www.gitignore.io/api/keil)
忽略规则参考[官方说明](https://git-scm.com/docs/gitignore)
```
# Created by https://www.gitignore.io/api/keil

### Keil ###
# something maybe need to add
# *.bat  # generate batch file
# output/project_name # uv project generate without file ext
# *.SRC # generate source file
# *.src

# Prerequisites
*.d
*.dep
# Object files
*.o
*.obj
*.OBJ
*.crf
*.htm
*.html

# Listing Files
*.COD
*.cod
*.[Ii]
*.LST
*.lst
*.MAP
*.map
*.[Mm]51
*.[Mm]66
*.SCR
*.scr

# LINK output
*.ABS

# Libraries
*.lib
*.LIB


# uv output
*.lnp
*.build_log.htm
*.HEX
*.hex
*.AXF
*.axf

# Build Files
*._IA
*.__[Ii]
*._[Ii][Ii]

# project
*.uvgui.*
*.uvguix.*



# End of https://www.gitignore.io/api/keil

```
## 初始化完成的标准

一个状态良好的仓库并不是完成了第一提交就算完成了,还要进行一些仓库的测试
1. 测试是否忽略了所有不必要存储的文件(生成的目标文件和中间文件)
2. 是否忽略了不能忽略的文件

测试1:将仓库拷贝一份到其他目录,进行build,查看仓库状态是否是干净的,如果不干净,则说明有文件未忽略

测试2:在另外一个路径下克隆仓库,本地克隆的方法是`git clon [dir]`,如`git clon C:/xxx/xxx/仓库文件夹`.克隆后,进行build,查看仓库状态是否是干净的,如果不干净,则说明有文件未忽略,如果编译不通过,则可能是多忽略了什么文件.

## 注意
如果你已经有了一个仓库,.gitignore文件是后来就加进去的,那么,为了使.gitignore文件生效,需要将原本在仓库中记录的文件删掉
比如原来的仓库记录了*.o文件,那么需要先`git rm xxxx.o` ,之后才能正常的忽略

换而言之,.gitignore只能忽略掉未加入仓库跟踪的文件,已经被仓库跟踪的文件,不能被忽略掉,必须先解除跟踪
# git管理keil工程

## git的安装

安装Windows版本的git

- [git-base shell](https://git-scm.com/downloads)
- [git 文档获取](https://git-scm.com/doc)
- [git 图形界面客户端](https://git-scm.com/downloads/guis)

其中图形界面客户端不是必须,详细安装过程参考附件<git 在Windows上的安装和使用实践\>

## 初始化keil工程

1. 新建工程文件夹(名字不要有空格和中文)
2. 组织目录组织目录结构,添加代码,并新建或者拷贝keil工程
3. 添加README.md 文件
4. 添加git忽略文件(.gitignore),文件内容参考下文
5. 使用`git init `命令初始化仓库,然后添加(`git add .`)文件跟踪,并进行第一次提交(`git commit `)

> 建议: keil工程的工程文件放在整个工程的根目录下,方便寻找

> 提示: 第一次提交可能有不完善的地方,进行修改后使用`git commit --amend`命令修改第一的提交


**[keil C工程的忽略文件](http://192.168.20.39/snippets/11)**

文件内容来自[https://www.gitignore.io/api/keil](https://www.gitignore.io/api/keil)
忽略规则参考[官方说明](https://git-scm.com/docs/gitignore)


## 初始化完成的标准

一个状态良好的仓库并不是完成了第一提交就算完成了,还要进行一些仓库的测试
1. 测试是否忽略了所有不必要存储的文件(生成的目标文件和中间文件)
2. 是否忽略了不能忽略的文件

测试1:将仓库拷贝一份到其他目录,进行build,查看仓库状态是否是干净的,如果不干净,则说明有文件未忽略

测试2:在另外一个路径下克隆仓库,本地克隆的方法是`git clon [dir]`,如`git clon C:/xxx/xxx/仓库文件夹`.克隆后,进行build,查看仓库状态是否是干净的,如果不干净,则说明有文件未忽略,如果编译不通过,则可能是多忽略了什么文件.

## 注意
如果你已经有了一个仓库,.gitignore文件是后来就加进去的,那么,为了使.gitignore文件生效,需要将原本在仓库中记录的文件删掉
比如原来的仓库记录了*.o文件,那么需要先`git rm xxxx.o` ,之后才能正常的忽略

换而言之,.gitignore只能忽略掉未加入仓库跟踪的文件,已经被仓库跟踪的文件,不能被忽略掉,必须先解除跟踪

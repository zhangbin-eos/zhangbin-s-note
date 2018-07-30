# 项目创建和管理

* 每个用户都可以创建项目/组
* 普通用户创建的项目/组在其用户目录下
* 管理员创建项目/组在服务器根目录下

每个项目/组都具有三种级别的权限

| visibility | Level |
| :--- | :--- |
| Private | Private visibility has been restricted by the administrator. |
| Internal | The project can be accessed by any logged in user. |
| PublicThe | project can be accessed without any authentication. |

## 前期和用户权限相关的一些设置
---
从我个人角度来看,我是希望用户拥有自己的工程的,比如我写个测试代码啊,写个小东西之类的.我也希望能在服务器上进行管理和备份,这样能节省我的大量时间.但是用户能够自己使用的空间应该受到管理和限制,不然,个人的无休止的创建项目会侵占服务器资源,给集体造成不便.
以下的设置都是在管理员账上进行的操作,我也建议**正式的项目由管理进行创建**,而个人创建的项目数,只有**10**个,并且**不能再创建组**.

## 项目创建
---
### 创建一个组
照着图创建就行了
![](/assets/create_group.jpg)

### 设置组成员
![](/assets/set_group_merber.jpg)

### 创建一个项目
![](/assets/create_pjt.jpg)
### 设置权限
一个(gitlab)项目中,主要包含以下的可编辑项(在项目的Settings->General->Permissions)
```
Issues
    需求和BUG
    Lightweight issue tracking system for this project

Repository
    仓库,项目(源)文件的主要存贮位置,并记录文件修改差异等
    View and edit files in this project

Merge requests
    合并请求,用于新功能的提交或者BUG修复
    Submit changes to be merged upstream

Pipelines
    自动集成和测试功能
    Build, test, and deploy your changes

Wiki
    文档界面
    Pages for project documentation

Snippets
    与其他仓库共享代码
    Share code pastes with others out of Git repository
```
在创建项目的时候,不同的visibility level,在这几项上对应不同的处理方式,每一项中,都包含着两个控制选项
- 1.所有人可以访问
- 2.仅项目成员可以访问

### 仓库的权限控制
上节中提到,在Settings->General->Permissions中有Repository的权限设置,但是只是限制了项目成员访问或者所有人可以访问,实际使用的过程中,仓库作为项目文件的存储实体,是一个项目的主要成果,所以至关重要,同时,仓库在实际的开发过程中,为了方便管理和高效开发,会划分几个分支,项目成员对分支的访问权限也异常重要.

#### 保护分支
在项目的**Settings->Repository->Protected Branches**(如下图),可以设置保护分支,可以设置分支的运行推送的和允许合并的项目组成员角色,如图可以避免非项目组成员的提交和合并,可以避免测试人员在测试过程中意外的合并或者提交.设置保护分支可以使用通配符,比如***protect/* *** 可以设置 protect/*开头的所有分支.[参考wildcard-protected-branches](https://docs.gitlab.com/ee/user/project/protected_branches.html#wildcard-protected-branches)
![](/assets/保护分支设置.jpg)

#### 保护标签
标签是git仓库中的一种标记,通常用来记录版本发布,所以,对于一些特殊格式的标签,需要限制项目成员对其的读写操作,避免误操作.
在项目的**Settings->Repository->Protected Tags**可设置创建标签的权限(删除标签的权限仅master用户有),例如可以设置`v* or *-release` 标签仅master创建的权限.

### 设置参与项目的人员
参考设置组成员即可
值得一提的是成员角色,项目成员有四种角色


1. Master
    项目的管理者
1. Developer
    项目的开发者
1. Reporter
    项目的测试人员
1. Guest
    访客

项目中各个角色的权限参考[Project members permissions](https://docs.gitlab.com/ee/user/permissions.html#project-members-permissions)
### README.md的编写
建议每个项目写个README.md文件,放在项目的根目录下,这样你就能在你项目的首页上,直接看到
![](/assets/gitlab_md.jpg)
**注**:README.md是markdown格式的文本文件
[Markdown语法参考](https://docs.gitlab.com/ee/user/markdown.html)

## 项目流程的管理
---
gitlab的服务器搭建完成后,基本项目流程其实也就出现了,并且,这个流程会随着服务器功能的完善而越发的严谨细致帮助我们避免错误.提高效率

### 标准流程
参考官方流程的说明
[什么是gitlab flow](https://docs.gitlab.com/ee/university/training/gitlab_flow.html#doc-nav)
[Introduction to GitLab Flow](https://docs.gitlab.com/ee/workflow/gitlab_flow.html#doc-nav)
### 简化流程





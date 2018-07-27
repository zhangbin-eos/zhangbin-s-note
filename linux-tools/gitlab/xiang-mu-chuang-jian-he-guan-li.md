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


### 简化流程





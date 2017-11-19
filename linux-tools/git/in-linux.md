# git 在Linux上的安装和使用实践

## Git的安装

### 下载Git源码包

* 建议使用源码安装,因为rpm包通常版本太低,导致很多功能无法使用
  官方源码下载[https://www.kernel.org/pub/software/scm/git/](https://www.kernel.org/pub/software/scm/git/)
* 选择一个发布时间比较新的版本下载,选择gz格式的包,xz的不好解压
* 版本低于2.0.0的可能在push时出现一下问题

  ```
    ! [remote rejected] master -> refs/for/master (change 144 closed)

    error: failed to push some refs to ...
  ```

### 安装依赖库

得到源码后,阅读INSTALL和configure --help的内容\(一定要看,不然很多问题不知道为啥\),里面会介绍安装的方法和依赖包,并注意依赖包的版本要求

* 我的git下载的是git-2.9.5,有几个比较特殊的包是

  **expat** 这个是XML的解析库  
    **perl-5.26.1**: PERL库  
    **openssl**:这个是ssh的包  
    **curl-7.56.1**:这个是支持https协议的包

这几个包必须有,而且标记了版本号的,是比较新的版本,git对此有版本要求

* 依赖库的安装可以同样需要阅读README和INSTALL,不过安装过程直接

  > ./configure  
  >   make  
  >   make install

  就可以了,有些软件包需要生成./configure或者使用./configure -de\(默认安装的意思\),所以安装前看README和INSTALL

### 安装git

* 首先配置

  > ./configure --with-openssl --with-curl --with-expat  --with-perl=PATH &gt; install\_log

  其中path指的是对应的库所在路径,这几个需要指定,不然configure默认不选,结果安装完以后git部分功能无法使用  
    重定向到install\_log是为了能够较好的检查配置结果  
    我安装时configure过程的部分结果

  ```
        checking for SHA1_Init in -lcrypto... yes
        checking for curl_global_init in -lcurl... yes
        checking for Curl_ssl_init in -lcurl... yes
        checking for curl-config... curl-config
        checking for XML_ParserCreate in -lexpat... yes
  ```

  **configure一定要指定--with-openssl --with-curl --with-expat 这些选项,否则会出现**

  > "git clone: fatal: Unable to find remote helper for 'https'"

* 看INSTALL
  ```
    $ make prefix=/usr all doc info ;# as yourself
    # make prefix=/usr install install-doc install-html install-info ;# as root
  ```

  我们按照root的来安装
  安装过程如果出错,只要不是install过程的错误都可以忽略
  比如提示make install-doc error 之类的,可能是一些小工具没有安装,导致一些文档无法导

* 配置

  让人头大的安装过程终于结束,接下来是令人愉悦的配置过程  
    git的配置还是很简单的  
    以下为配置名字和账户,建议使用--global,用户级的配置文件

  **git config --global ...**

  > $ git config --global user.name "zhangbin"  
  >   \(如果是只使用局域网,邮件瞎填没关系,但是最好填个能联系到的\)  
  >    $ git config --global user.email xxx@xxx.com  
  >    $ git config --global core.editor vim  
  >    $ git config --global merge.tool vimdiff  
  >    $ git config --list\(\)

### 使用Git

#### 服务器端
1. 创建版本库
     找一个文件夹,空的,然后运行git init 就可以了
     比如

        #mkdir ~/pro_test`
        # cd ~/pro_test`
        # git init`

1. 添加本地库为远程库(这是server端的)
    `# git remote add origin ssh://用户名@IP/库路径/.git`
        比方说我的server端用户名就叫git,IP192.168.20.39
    `# git remote add origin ssh://git@192.168.20.39/~/pro_test/.git`
1. 添加文件     
    `# echo "hello" > README`
    `# git add .`

1. 查看现在的状态
    `#git status`

        ```
        显示
        >>> git status
        位于分支 master

        初始提交

        要提交的变更：
          （使用 "git rm --cached <文件>..." 以取消暂存）

                新文件：   README

        [Alex@Archimonde ~/test] @test
        ```

1. 提交修改到本地暂存
    `#git commit -m "init"`
    `#git status`

1. 推送本地的库到远程
    `# git push origin master`

    server端大功告成

#### 客户端
    随便找个路径
    `# git clone ssh://git@192.168.20.39/~/pro_test/.git`
    其他的和server端的操作一样,这里会提示输入git用户的密码

# SSH 公钥的使用

主要是为了设置权限和避免使用密码登录

参考  
[http://blog.csdn.net/kongqz/article/details/6338690](http://blog.csdn.net/kongqz/article/details/6338690)

# 错误

**Git Push 错误 \[remote rejected\] master -&gt; master \(branch is currently checked out\) & 无法查看push后的git中文件**
 
这是由于git默认拒绝了push操作，需要进行设置，修改.git/config文件后面添加如下代码：

```
[receive]
     denyCurrentBranch = ignore
```

也可以在~/.gitconfig文件中添加

安装的步骤就先这样吧

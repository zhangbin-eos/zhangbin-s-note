# 账户注册和设置

## 注册

---

账户有两种类型

* 管理员
* 非管理员\(普通账户\)

目前我们的服务器是用户自己注册,还有一种方式是邀请注册,即用户不能自行注册账户,但是邀请注册需要邮件服务的支持,我们并未配置,以下仅介绍自行注册的方式

### 普通用户自行注册

1. 通过浏览器访问服务器ip地址:xxx.xxx.xxx.xxx
2. 点击**Register**,如图
   ![](/assets/sign_in.jpg)
   其中Username不能重名,这个属于账户名,full name随便起,相当于昵称
3. 注册之后就进入了欢迎界面
   ![](/assets/gitlabWelcome.jpg)

## 设置

---

### 普通用户的设置

1. 设置ssh key  
   ssh key的设置是为了让你本地的git能够无密码的登录你自己的账号,进行上传和下载的操作  
   点击自己的头像\(右上角\)-&gt;选择Settings-&gt;选择SSH Key\(左侧\)  
   ![](/assets/setsshkey.jpg)

2. 在自己电脑的控制台上\(或者是git bash\)运行下面的命令

   ```
   ssh-keygen -t rsa -C "${Email}"
   ```

   ${Email}是你的gitlab账户的邮箱,命令的显示和打印输出如下

   ```
   Generating public/private rsa key pair.
   Enter file in which to save the key (/home/zb/.ssh/id_rsa): 
   /home/zb/.ssh/id_rsa already exists.
   Overwrite (y/n)? y
   Enter passphrase (empty for no passphrase): 
   Enter same passphrase again: 
   Your identification has been saved in /home/zb/.ssh/id_rsa.
   Your public key has been saved in /home/zb/.ssh/id_rsa.pub.
   The key fingerprint is:
   1e:2c:1e:7e:ca:f0:da:93:0d:be:eb:4a:64:6c:57:7a zhangbin.eos@foxmail.com
   The key's randomart image is:
   +--[ RSA 2048]----+
   |                 |
   |                 |
   |        .        |
   |   .   +         |
   |    = = E        |
   |   + +.= .       |
   |    o.o+o        |
   |   . =+o.        |
   |    o+O+         |
   +-----------------+
   ```

   生成的公钥文件在/home/${user}/.ssh/id\_rsa.pub中,将文件的内容添加到gitlab的界面上,点击add key即可

在PC上测试ssh是否生效,192.168.20.39为gitlab的IP

```
$ ssh git@192.168.20.39

Enter passphrase for key '/c/Users/liguo/.ssh/id_rsa':
PTY allocation request failed on channel 0
Welcome to GitLab, Administrator!
Connection to 192.168.20.39 closed.
```

**这个的意思是ssh能访问,但是不允许登录,这个是gitlab添加的限制**  
然后就可以使用git来客隆服务器上的代码了,使用::

```
#或者使用git clone git@192.168.20.39/learn/learnGitBranching.git
>>> git clone http://192.168.20.39/learn/learnGitBranching.git          
Cloning into 'learnGitBranching'...
remote: Counting objects: 15622, done.
remote: Compressing objects: 100% (6152/6152), done.
remote: Total 15622 (delta 8964), reused 15622 (delta 8964)
Receiving objects: 100% (15622/15622), 48.30 MiB | 10.96 MiB/s, done.
Resolving deltas: 100% (8964/8964), done.
Checking connectivity... done.
Checking out files: 100% (146/146), done.

>>> ls learnGitBranching/
assets  CNAME  Gruntfile.js  lib  LICENSE.md  npm-shrinkwrap.json  
package.json  README.md  src  __tests__  todo.txt
```

即可获取到服务器上的源码  
**你能看到项目,能克隆项目,但是不见得能够提交**

### 管理员用户的设置

管理员拥有最高的权限,能够限制普通用户登录或删除普通用户等功能,同时管理员还可以修改gitlab服务器的一些全局设置

1. sshkey的设置同上  
1. Overview
   - 添加/删除用户或者限制用户登录
   - 创建/删除组,或者修改组的权限和参与人员
   - 创建/删除项目,或者修改组的权限和参与人员
   - 管理runner\(自动集成中使用\)
     ![](/assets/sudooverview.jpg)
   - 管理员可以设置各个用户的一些权限,在Admin area-&gt;Overview-&gt;users中
   
     - 设置用户的账号名/密码等
     - 设置用户的权限
     - 项目数量
     - 是否能够创建组
     - 用户等级\(可以设置为普通用户或者管理员用户\)
     - 管理员还可以自己新建用户
1. Monitoring--包含一些系统信息和服务器状态信息
1. Settings--一些针对服务器的全局设置,例如默认的用户权限,默认的项目权限等
1. Appearance--这里是一些图标或者说明的设置,如
  - 登录界面的图片和说明设置
  - 项目创建时的说明文档
  - 等等
1. 其他的我暂时还没有弄的太明白

未完待续!


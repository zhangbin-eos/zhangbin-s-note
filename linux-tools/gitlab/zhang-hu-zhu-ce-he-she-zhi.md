# 账户注册和设置


## 注册
---
账户有两种类型
 - 管理员
 - 非管理员(普通账户)

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
1. 点击自己的头像(右上角)->选择Settings->选择SSH Key(左侧)
![](/assets/setsshkey.jpg)

1. 在自己电脑的控制台上(或者是git bash)运行下面的命令
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
生成的公钥文件在/home/${user}/.ssh/id_rsa.pub中,将文件的内容添加到gitlab的界面上,点击add key即可

在PC上测试ssh是否生效,192.168.20.39为gitlab的IP
```
$ ssh git@192.168.20.39

Enter passphrase for key '/c/Users/liguo/.ssh/id_rsa':
PTY allocation request failed on channel 0
Welcome to GitLab, Administrator!
Connection to 192.168.20.39 closed.
```
**这个的意思是ssh能访问,但是不允许登录,这个是gitlab添加的限制**

### 管理员用户的设置
管理员拥有最高的权限,能够限制普通用户登录或删除普通用户等功能,同时管理员还可以修改gitlab服务器的一些全局设置
1. sshkey的设置同上
1. Overview
- 添加/删除用户或者限制用户登录
- 创建/删除组,或者修改组的权限和参与人员
- 创建/删除项目,或者修改组的权限和参与人员
- 管理runner(自动集成中使用)
 ![](/assets/sudooverview.jpg)

未完待续!



































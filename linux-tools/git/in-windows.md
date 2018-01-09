# git 在Windows上的安装和使用实践

下载git-base  
[Git-2.15.0-32-bit](https://git-scm.com/downloads)

git 文档获取  
[Documentation](https://git-scm.com/doc)

git 图形界面客户端  
[GUI Clients](https://git-scm.com/downloads/guis)

# git-base安装

需要安装的功能,git-base自带一个GUI界面,但是功能和Git bash差的比较多,可以简单的使用.  
![图1](/assets/Git Bash 1.bmp)

![图2](/assets/Git Bash 2.bmp)

如果不使用HTTPS协议\(只是在局域网内使用,或者单机使用\)的话,不选择也行  
![图3](/assets/Git Bash 3.bmp)

这个选项可以保证代码怎么进入的怎么出来,git不去做关于平台的判断,不修改回车换行等  
![图4](/assets/Git Bash 4.bmp)

以下的两个选项任意选择其中一个,我选择第一个只是因为cmd.exe丑,但是其实两个长的没差哪去  
![图5](/assets/Git Bash 5.bmp)

![图6](/assets/Git Bash 6.bmp)

# git-base使用

## 克隆一个远程库

1. 新建一个测试用的目录F:\Work\_Space\git\_source\_tree\test\_git\_base
2. 鼠标右击选择Git Bash Here,就会弹s出一个git bash的界面,并且当前路径位于F:\Work\_Space\git\_source\_tree\test\_git\_base
   ![](/assets/Git Bash.bmp)
3. 输入命令
   > git clone ssh://git@192.168.20.39/~/2017/lig\_mtrx\_dll

即从git@192.168.20.39这个电脑上,拷贝一个仓库到这个路径下,注意,当前路径不能有非空的lig\_mtrx\_dll同名文件夹  
![](/assets/Git Bash_1.bmp)

## 创建一个本地库

1/2步和上面相同,然后新建一个lig\_mtrx\_dll的文件夹\(可以是空的但是不能是一个仓库\),使用命令,

> git init

即可,此时的lig\_mtrx\_dll会出现git用来记录版本信息的.git目录

# git 图形界面客户端

一下以 SourceTree这款软件举例说明\(因为我只安装过这一个图形客户端\)

## 下载地址 &gt; [SourceTree](https://www.sourcetreeapp.com/)

我安装的时候,最新的版本是2.3.5.0,可以下载最新的使用  
尽量在在联网的状态下安装,SourceTree安装时会自动下载一些依赖

SourceTree在安装的时候需要注册,直接关闭掉软件,然后按照如下的步骤操作即可  
在目录C:\Users{youruser}\AppData\Local\Atlassian\SourceTree 下创建文件accounts.json  
**注意：{youruser}需要替换为登录系统用户名。如果找不到AppData,那么可能是AppData被隐藏了,设置文件夹选项,打开隐藏文件可见即可.**  
**注意：文件中,"zhangbin": "zhangbin.eos@foxmail.com" 这个要替换成你自己的名字和邮箱**

写入如下内容：

```JSON
[
  {
    "$id": "1",
    "$type": "SourceTree.Api.Host.Identity.Model.IdentityAccount, SourceTree.Api.Host.Identity",
    "Authenticate": true,
    "HostInstance": {
      "$id": "2",
      "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountInstance, SourceTree.Host.AtlassianAccount",
      "Host": {
        "$id": "3",
        "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountHost, SourceTree.Host.AtlassianAccount",
        "Id": "atlassian account"
      },
      "BaseUrl": "https://id.atlassian.com/"
    },
    "Credentials": {
      "$id": "4",
      "$type": "SourceTree.Model.BasicAuthCredentials, SourceTree.Api.Account",
      "zhangbin": "zhangbin.eos@foxmail.com"
    },
    "IsDefault": false
  }
]
```

重新启动软件，顺利进入界面

## 添加远程库

可以先使用git bash将仓库克隆下来,在使用添加命令(直接克隆会报错,因为sourcetree没有ssh权限)

![](/assets/STree1.jpg)


![](/assets/STree2.jpg)





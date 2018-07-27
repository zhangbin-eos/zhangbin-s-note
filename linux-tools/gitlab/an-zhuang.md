参考
> [gitlab在线文档:安装说明](https://about.gitlab.com/installation/)
> [gitlab在线文档:docker方式安装](https://docs.gitlab.com/ee/install/docker.html)

## 安装前的准备
---
1. 我们使用docker镜像的方式安装
 - 这种安装方式的好处是部署方便,gitlab的docker镜像包含了gitlab运行的所有环境,一键安装,无需配置环境
 - 安装后的gitlab所有的配置和使用数据集中,方便管理.
 - 同时具备以上两点后,服务器迁移也变得简单了
2. 硬件和系统要求
 - 双核,2GB运行内存,硬盘无特殊要求
 - 64位linux发行版,建议使用Centos7,系统相对其他几个发行版来说更为稳定,并且在运维方面的资料比其他发行版要丰富
 - 联网安装,比较简单
 
## 安装过程
---
系统最小安装,完成之后配置网络,确保网络通畅
```
root@admin ~]# ping www.baidu.com
PING www.a.shifen.com (119.75.213.61) 56(84) bytes of data.
64 bytes from 119.75.213.61: icmp_seq=1ttl=56time=5.48 ms
64 bytes from 119.75.213.61: icmp_seq=2ttl=56time=7.26 ms
```
安装docker前先更新下系统的软件包,如果更新的较慢的话可以更换国内的镜像源()
```
[admin@alex ~]$ sudo yum update
```
### docker安装
1. 使用docker提供的网络安装脚本进行安装
```
[admin@alex ~]$ sudo curl -sSL https://get.docker.com/ | sh 
```
2. 添加系统启动项
```
[admin@alex ~]$ sudo systemctl start docker
[admin@alex ~]$ sudo systemctl enable docker
```
3. 查看docker版本 
```
[admin@lig-linux ~]$ sudo docker version 
[sudo] admin 的密码：
Client:
 Version:       18.02.0-ce
 API version:   1.36
 Go version:    go1.9.3
 Git commit:    fc4de44
 Built: Wed Feb  7 21:14:12 2018
 OS/Arch:       linux/amd64
 Experimental:  false
 Orchestrator:  swarm
 
 Server:
 Engine:
  Version:      18.02.0-ce
  API version:  1.36 (minimum version 1.12)
  Go version:   go1.9.3
  Git commit:   fc4de44
  Built:        Wed Feb  7 21:17:42 2018
  OS/Arch:      linux/amd64
  Experimental: false
```

### gitlab镜像下载
3. 下载gitlab的docker镜像
```
[admin@alex ~]$ sudo docker pull gitlab/gitlab-ce:latest
latest: Pulling from gitlab/gitlab-ce
1be7f2b886e8: Pull complete 
6fbc4a21b806: Pull complete 
c71a6f8e1378: Pull complete 
4be3072e5a37: Pull complete 
06c6d2f59700: Pull complete 
3e236075b07f: Pull complete 
9d3aa9a75297: Pull complete 
1fad4f75154b: Pull complete 
5383bef86102: Pull complete 
7343d4ab63b3: Pull complete 
7f250f1b8a34: Pull complete 
Digest: sha256:ed6ce86e7847d14f0c75cacf7b5b4dfedeb3bb55b3be4c06c3d6124df8b20689
Status: Downloaded newer image for gitlab/gitlab-ce:latest
```
gitlab-ce指的是个人免费板,企业版是gitlab-ee,收费的

### gitlab镜像安装
这个就不用装了,镜像就是gitlab的运行环境,每次gitlab启动,都是通过docker来启动这个镜像从而启动gitlab的所有服务
接下来参考[配置和启动](/linux-tools/gitlab/pei-zhi-he-qi-dong.md)


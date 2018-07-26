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
yum update
```
### docker安装


### gitlab镜像下载
### gitlab镜像安装

 
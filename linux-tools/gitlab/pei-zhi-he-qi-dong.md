# 配置和启动

docker已经安装完成,gitlab的docker镜像也下载完毕,接下来我们就可以通过docker来启动gitlab

## 启动前的准备
---
为了确保启动成功,和gitlab的正常运行,需要做些准备工作

1. gitlab包含的服务

   * ngix服务器\(提供http服务,默认使用80端口,443端口\)
   * git
   * ssh\(默认使用22端口\)

2. 如果电脑本身在运行以上的服务,则需要先关闭这些服务\(其实有不用关闭的方法\),避免一些端口冲突造成gitlab无法正常的启动或者功能异常

   ```
   #停止sshd服务,如果你是通过ssh登录的,
   #最好先安装telnet-server,并配置使服务器可以通过远程登录进行操作
   systemctl stop sshd
   ```

   也可以不关闭ssh服务,但是在启动的时候,需要配置一个网桥连接到镜像,让镜像使用和当前本机IP不同的IP地址,以避免端口冲突

## 启动
---
1. 启动命令

   ```
   docker run --detach \
     --hostname 192.168.20.39 \
     --publish 443:443 \
     --publish 80:80 \
     --publish 22:22 \
     --name gitlab \
     --restart always \
     --volume /home/gitlab/config:/etc/gitlab \
     --volume /home/gitlab/logs:/var/log/gitlab \
     --volume /home/gitlab/data:/var/opt/gitlab \
     docker.io/gitlab/gitlab-ce
   ```

   * --hostname是镜像的IP
   * --publish映射了一些端口
   * --restart always是gitlab关闭总是自动的重启
   * --volume 映射了一些路径,可以看出,在服务器上,gitlab的数据是存放在/home/gitlab/目录下的,而在gitlab的镜像中,gitlab的数据是在冒号右边的路径中

2. 查看启动状态  
   执行了启动命令之后,gitlab需要1-2分钟的启动时间

   ```
   [admin@lig-linux ~]$ sudo docker ps -a --format='{{.Names}} {{.Status}}'| grep gitlab
   [sudo] admin 的密码：
   gitlab Up 8 hours (healthy)
   ```

   这时就可以访问gitlab了,随便找个电脑,设置IP为服务器的同网段,然后在浏览器上输入服务器的地址192.168.20.39  
   后续的东西就很简单了,登录上去,按照指导做就行

3. 重设管理员账户和密码
   在服务器上启动gitlab的bash
   ```
   sudo docker exec -it gitlab /bin/bash
   ```

   然后在终端中,使用以下命令进行重新设置管理员账户信息
   ```
   # gitlab-rails console production
   Loading production environment (Rails 4.2.8)
   irb(main):001:0> user = User.where(id: 1).first
   => #<User id: 1, email: "admin@example.com"......
   irb(main):009:0> user.password = '12345678'
   => "12345678"
   irb(main):010:0> user.password_confirmation = '12345678'
   => "12345678"
   irb(main):011:0> user.save!
   Enqueued ActionMailer::DeliveryJob (Job ID: 510bb5be-a156-4522-9983-44d8a895e92a) to Sidekiq(mailers) with arguments: "DeviseMailer", "password_change", "deliver_now", gid://gitlab/User/1
   => true
   irb(main):011:0> exit
   ```

## 注意
---
* 在`sudo docker ps -a --format='{{.Names}} {{.Status}}'| grep gitlab`结果为health之后,再访问,否则会碰到404之类的错误
* docker镜像启动的gitlab在异常关机时,退出状态异常,所以在下次开机后将不能正常的重启gitlab\(如果是正常关机没问题\),所以在这种情况下启动时,需要先停止并删除之前的启动信息
  ```
  docker stop gitlab
  docker rm -f  gitlab
  ```

## 启动脚本
---
> \[admin@lig-linux ~\]$ cat gitlab.sh

```
ifconfig enp2s0 192.168.20.39
ifconfig enp2s0 netmask 255.255.0.0

#如果涉及网络修改的时候需要重新启动docker 的守护进程
systemctl restart docker.service 
systemctl stop sshd 

docker stop gitlab
docker rm -f  gitlab

docker run --detach \
     --hostname 192.168.20.39 \
     --publish 443:443 \
     --publish 80:80 \
     --publish 22:22 \
     --name gitlab \
     --restart always \
     --volume /home/gitlab/config:/etc/gitlab \
     --volume /home/gitlab/logs:/var/log/gitlab \
     --volume /home/gitlab/data:/var/opt/gitlab \
     docker.io/gitlab/gitlab-ce
```

## 检测gitlab状态的重启脚本
---
> \[admin@lig-linux ~\]$ cat check\_docker\_gitlab\_stat.sh

```
#! /bin/bash
# zhangbin.eos@foxmail.com

#need run by root
#every 10 min check once
#see /etc/crontab 

#get docker gitlab stat

#$1 = gitlab

stat=$(docker ps -a --format='{{.Names}} {{.Status}}'| grep $1 | grep -E "[^un]healthy")

#echo $stat
if [ -z "$stat" ] 
then
        echo "unhealthy or Exited" > /dev/null
        #this shell need root run 
        if [ "$1" = "gitlab" ]
        then
                echo "$(date) $1 unhealthy or Exited ,will restart " >> /home/admin/check_gitlab.log
                sh /home/admin/gitlab.sh > /dev/null

        fi
else 
        #do nothing
        echo "healthy" > /dev/null
        logger -i -p info -t "check docker state" "$stat"
fi
```




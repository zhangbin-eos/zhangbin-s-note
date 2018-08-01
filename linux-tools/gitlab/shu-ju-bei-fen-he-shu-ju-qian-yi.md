# 数据备份和迁移

> 最简单不过的事情了,主要是两个操作  
> 1. 拷贝  
> 2. 粘贴

## 数据备份
---
前面的内容中,在启动gitlab docker镜像时,有一堆的设置参数,其中就有gitlab的数据存放路径

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

在gitlab启动后,我们看下/home/gitlab/下的内容\(仅显示部分\)

```
[admin@lig-linux ~]$ s tree -L 3 --filelimit 20 /home/gitlab/  
/home/gitlab/
├── config
│   ├── gitlab.rb
│   └── trusted-certs
├── data
│   ├── backups
│   ├── bootstrapped
│   ├── gitaly
│   │   ├── config.toml
│   │   └── gitaly.socket
│   ├── git-data
│       └── repositories
│  
└── logs
```

* 其中/home/gitlab/data/git-data/repositories/这个文件夹下放的就是项目文件了
* 其中/home/gitlab/config/gitlab.rb 是gitlab的配置文件

**其实**前面这么多信息都没啥用,**备份的时候,只要**

```bash
tar -czvpf $bpsavedir/bak$(date +%Y%m%d%H).tar.gz --exclude *.log  -C /home/gitlab/ $(ls -A /home/gitlab/)
```

其中$bpsavedir/bak$\(date +%Y%m%d%H\).tar.gz是要生成的备份文件的路径和文件名+时间,压缩格式是gzip

将**$bpsavedir** 替换为一个路径否则将在/下生成bak2018010101.tar.gz这样的备份文件

**这是一个备份脚本**

```
#! /bin/bash

cur_data="$(date +%Y%m%d%H%M)"
srcpath="/home/gitlab/"
srcfile=$(ls -A $srcpath)

objpath="/mnt/disk/gitlab-bak"
objname="bak$(date +%Y%m%d%H%M).tar.gz"
exclude="*.log"


mount /dev/mapper/vg_linuxserver-lv_home /mnt/disk

tar -czpf $objpath/$objname  --exclude $exclude  -C /home/gitlab/ $(ls -A /home/gitlab/)
# 记录备份信息
echo -e "$objname\t" "$(du -h $objpath/$objname )"  >>  /home/admin/backup.log

umount /mnt/disk
```
**这是一个清除冗余备份的脚本**
```
#!/bin/bash

# one mouth run once
# every mouth 01day 00h 00m run 

objpath="/mnt/disk/gitlab-bak"
# mount bak disk
mount /dev/mapper/vg_linuxserver-lv_home /mnt/disk
file_list="$(ls $objpath | grep -v -E "bak[0-9]{6}010000.tar.gz")" 

# 判断下文件列表是不是空
if [ -z $file_list  ]
then
        umount /mnt/disk
        exit 1
fi

#切花到目标目录
cd $objpath
rm -rf $file_list
#随便切换个目录就行,主要是为了umont的时候没有在挂载目录下就行,否则会挂载失败
cd
 
umount /mnt/disk
```
**这是一个定时执行软件crontabs的配置**
```
[root@lig-linux admin]# cat /etc/crontab 
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

#每十分钟检测一次状态
*/10 *  *  *  * root       /home/admin/check_docker_gitlab_stat.sh gitlab

#每6小时进行一次备份
0   */6 *  *  * root       /home/admin/backup-gitlab.sh
```


# 迁移
---
参考安装过程,然后将数据拷贝过去即可,注意修改启动脚本中的hostname.


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

### 提示

备份操作在linux中可以通过定时软件来进行定时备份,同时,备份的数据最好存放在其他的硬盘上,总之写好备份脚本后,就不用管了

# 迁移
---
参考安装过程,然后将数据拷贝过去即可,注意修改启动脚本中的hostname.


# u-boot笔记

check out from u-boot tags:v2013.10
获取官方源码,切换版本到v2013.10,这个版本中还有ads5121的配置

## 清理代码
乱七八糟的板级代码清理掉,只保留了ads5121的,拷贝mpc5121ads为mpc5125ads
```
>>> ls board/freescale/
common  mpc5121ads  mpc5125ads
```
将mpc5125ads/下的文件改名字,改完后:
```
>>> ls board/freescale/mpc5125ads/
Makefile  mpc5125ads.c  mpc5125ads.su  README
```

# 修改配置,makefile等
1. 修改README,其中的配置指令错误,修改为`make  mpc5125ads_config`
1. 修改boards.cfg ,只保留了mpc5125ads的
```
Active  powerpc     mpc512x        -           freescale       mpc5121ads          mpc5121ads                           -                                                                                                                                 -
Active  powerpc     mpc512x        -           freescale       mpc5125ads          mpc5125ads                           -                                                                                                                                 -
```

执行make  mpc5125ads_config,出现头文件错误,继续改头文件的问题
```
fatal error: configs/mpc5125ads.h: No such file or directory
```
切换到include/configs/,发现有好多的头文件,先将mpc5121ads相关的拷贝为5125,然后删除一些明显没用的头文件

## 清理代码

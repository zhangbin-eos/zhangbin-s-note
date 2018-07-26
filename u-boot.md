# u-boot笔记

## 获取飞思卡尔原厂的u-boot代码
1. 从官网上下载了MPC512xADS_20090603-ltib.iso
1. 里面是各种的安装包,其中包含了u-boot-v2009.03版本的代码,还有很多的patch.
1. u-boot-v2009.03的代码中并没有5125的芯片代码,后来看了看补丁
    ```
    >>> ls /mnt/media/pkgs/u-boot-2009.03*.patch
    /mnt/media/pkgs/u-boot-2009.03-ADS5121-01-Resume-from-ram-2.patch                 /mnt/media/pkgs/u-boot-2009.03-ADS5121-08-enable-icache-2.patch
    /mnt/media/pkgs/u-boot-2009.03-ADS5121-02-Add-NAND-driver-3.patch                 /mnt/media/pkgs/u-boot-2009.03-ADS5121-09-reset-fix-3.patch
    /mnt/media/pkgs/u-boot-2009.03-ADS5121-03-Add-CW-debug-2.patch                    /mnt/media/pkgs/u-boot-2009.03-ADS5121-10-update-enabled-features-2.patch
    /mnt/media/pkgs/u-boot-2009.03-ADS5121-04-Update-default-env-2.patch              /mnt/media/pkgs/u-boot-2009.03-ADS5121-11-Change-naming-on-DRAM-2.patch
    /mnt/media/pkgs/u-boot-2009.03-ADS5121-05-Change-CONFIG_SYS_MONITOR_LEN-2.patch   /mnt/media/pkgs/u-boot-2009.03-ADS5121-12-add-new-elpida-memory-config-2.patch
    /mnt/media/pkgs/u-boot-2009.03-ADS5121-06-only-read-dvi-xvr-regs-2.patch          /mnt/media/pkgs/u-boot-2009.03-ADS5121-13-Add-board-ADS5125-3.patch
    /mnt/media/pkgs/u-boot-2009.03-ADS5121-07-FASTBOOT-delay-ide-pci-and-net-3.patch  /mnt/media/pkgs/u-boot-2009.03-ADS5121-14-Fixed-the-bug-on-ping.patch
    ```
    可以看出在第13个补丁处,有个add 5125的信息,所以说,需要对u-boot-v2009.03版本的代码进行打补丁
1. 因为patch看格式像是git-format-patch,所以需要从github上下载u-boot的仓库
    ```
    git clone https://github.com/u-boot/u-boot.git
    cd u-boot

    #切换到v2009.03
    git co v2009.03 
    
    #开始打补丁#(使用-s或--signoff选项，可以commit信息中加入Signed-off-by信息)
    git am --signoff < /mnt/media/pkgs/u-boot-2009.03-ADS5121*.patch
    # 如果出错了就reset --hard,然后一个一个打补丁
    ```
1. 将打完补丁的代码拷贝到u-boot-v2009.03-mpc5125ads,删除旧的.git,然后重建仓库,以后就从这个版本开始改

**我是了下在打完补丁后,将v2013.10版代码与之合并,冲突并不多,在搞明白2009.03版的移植后可以试试**

## 清理代码

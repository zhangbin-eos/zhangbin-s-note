# 持续集成部署
> 又叫 runner的安装和使用

## 准备一个生产/部署环境
以CPMS-34为例,即准备一台PC机和控制板

1. PC机需要运行linux环境,并安装有powerpc交叉编译器,和其他常用工具,如python,tftp,telnet,nfs,minicom等
1. PC机安装gitlab runner软件,这个软件的服务端在gitlab服务器上
安装过程参考[官方给的说明](https://docs.gitlab.com/runner/install/linux-repository.html)
1. 注意安装特定的版本,最新版的可能和服务器端不兼容
```
# for DEB based systems
apt-cache madison gitlab-runner
sudo apt-get install gitlab-runner=10.0.0
```

## 配置runner
1. 登录gitlab,在项目的Settings-->CI / CD Settings-->Runners settings中,找到如下的界面,我的这个是已经有runner的
![](/assets/ci-set.jpg)

1. 在runner的PC上,使用root用户,进行注册
```
su
gitlab-ci-multi-runner register
```
此过程中会需要填写服务器的url和token,和runner的标签和描述,标签和表述随便填,后面可以修改,选择runner的环境为shell环境,然后注册成功后,gitlab服务器上的runner settings中会显示注册的runner信息
	
1. 重启runnerPC
1. 在项目中添加.gitliab_ci.yml(参考gitlab相关说明和CPMS-34的例子)
1. 提交修改,查看gitlab服务器上的ci结果
![](/assets/ci_res.jpg)

## .gitliab_ci.yml文件说明
- [官方说明](https://docs.gitlab.com/ee/ci/yaml/#doc-nav)

- CPMS-34中的例子

```
# This file is a template, and might need editing before it works on your project.
# see https://docs.gitlab.com/ce/ci/yaml/README.html for all available options
# change from template
# author :zhangbin

before_script:
  - make > /dev/null 
  - make packet > /dev/null
  - git log --pretty=format:"%H" -1
  - cp nandflash.tar /tftproot/nandflash-$(git log --pretty=format:"%H" -1).tar

after_script:
  - echo "clean"
  - make clean > /dev/null
  
style_check:
  stage: build
  script:
    - echo "style_check"

test_1:
        stage: test
        script:
                - sh ./.lig_ci.sh
```
**stage**是关键字,只能是build,test,deploy
**before_script**:运行在每个测试项之前
**after_script**:运行在每个测试项之后
如:
test_1的执行过程:`before_script->test_1->after_script`
**script**命令,其后的以`-`开头的是shell命令,注意test_1中调用了一个仓库中的测试脚本`.lig_ci.sh`


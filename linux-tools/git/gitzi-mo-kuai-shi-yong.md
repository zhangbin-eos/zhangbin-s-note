# git子模块使用

## 添加子模块
将远程仓库设置为子模块,子模块的路径为 APP/thttpd-web
```
git submodule add git@192.168.20.39:2018/vs-xx-webpage.git APP/thttpd-web
```

## 获取子模块
克隆了/拉取了一个带有子模块的仓库(我们后续叫这个仓库是主仓库)后.使用如下命令进行更新
```
#初始化本地配置文件
git submodule init

#抓取子模块数据
git submodule update

```

这样子模块目录下就出现了文件


## 查看子模块配置文件/.gitmodules
在主仓库的目录下 运行 `cat .gitmodules`
```
[submodule "APP/thttpd-web"]
        path = APP/thttpd-web
        url = git@192.168.20.39:2018/kramer-vs-34fd-webpage.git
        branch = develop
```
其中`path`是子模块的存放的路径,`url`是子模块的远程链接,`branch`是追踪的默认分支

## 子模块拉取上游修改
1. 进入到子模块目录中运行`git fetch` 与`git merge`,合并上游分支来更新本地代码。
2. 进入到子模块目录中运行`git pull`,效果同上
3. **在主仓库的目录下**运行`git submodule update --remote --merge`效果同上


## 子模块的本地修改提交
切换到子模块目录下进行修改,和普通的git 仓库操作没有区别
1. 切换到子模块目录
2. 修改文件
3. 添加暂存
4. 提交

 
**注意**:提交子模块/切换子模块的分支(修改子模块的HEAD),你的主仓库会被标记为修改状态,
因为主仓库中记录了主仓库当前提交对应的子模块的提交,如,主仓库的提交是host_commit_1,记录的子模块的提交是sub_commit_1,如果当前的子模块HEAD!=sub_commit_1,主仓库将要求你进行一次提交,以更新主仓库所记录的子模块提交

## 子模块的推送
子模块推送时在子模块路径进行,和普通的推送一样
如果没有推送子模块,直接推送主仓库,主仓库就会引用到一个远程不存的提交.这时,别人在获取仓库时,将无法获取到子模块内的信息

为了避免以上的情况发生,在主仓库推送时,使用
```
# 检测子模块是否推送,如果没有,就停止主仓库的推送,推送失败
git push --recurse-submodules=check
```
或者
```
# 检测子模块是否推送,如果没有,就推送子模块,如果子模块推送失败,就停止推送,且主仓库的推送失败
git push --recurse-submodules=on-demand

```


命令太长,可以设置命令的别名 

1. 显示主项目和子模块的所有改动
	```
	$ git config alias.sdiff '!'"git diff && git submodule foreach 'git diff'"`
	```
1. 推送主仓库，同时检查子模块，如果没有推送，结束此命令提示用户推送子模块先
	```
	$ git config alias.spush 'push --recurse-submodules=on-demand'
	```
1. 推送主仓库，同时检查子模块，如果没有推送先推送子模块
	```
	$ git config alias.spush 'push --recurse-submodules=on-demand'
	```
1. 更新子模块，并且合并代码到本地
	```
	$ git config alias.supdate 'submodule update --remote --merge'
	```


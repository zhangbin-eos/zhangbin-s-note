> git work flow
> git 工作流程

# git使用规范(CPMS-34)
version:01.00.0002
author:张斌

## 分支约定

### 开发者本地分支

- develop 用于功能集成和集成测试,是feature/xxxx 父节点
- func/x_y_z.funcname 	用于开发功能

### 热修补分支
- hotfix/xxx  
用于发布后的issue修复,以及开发时同步异常代码修改

## 工作流约定
1. 任何功能分支都需要从develop上开始
2. 创建本地功能分支,分支名称规则为`func/x_y_z.funcname`
 1. 其中`x_y_z`是需求编号,可能使2位,可能是3位,根据每个人领到的需求不同
 2. 其中`funcname`是需求的功能简称,要求简短,辨识度高
3. 开发完成后合并功能分支到本地develop中,并推送develop

	```
	git pull origin develop(保证本地的develop是最新的)
	git checkout develop
	git merge --no-log func/x_y_z.funcname
	git push develop
	git branch -d func/x_y_z.funcname(选做)
	
	```

	![git流程](/assets/git流程1.jpg)
4. 测试<-->发布前bug的修改在develop分支中完成

	![gitfix流程](/assets/git流程2_fix.jpg)


## 提交

1. 只提交自己做的功能和修改.
2. 如果提交记录不符合规范,需要使用git commit --amend 修改提交日志

## 合并
1. 合并时,使用 merge --no-ff参数 ,以减少/简化冗余的提交信息(不会压缩提交)
2. hotfix的合并可以使用--squash,用以压缩提交
3. 合并提交的格式 

	```
	Merge branch (功能分支) : 功能描述
	
		可做详细描述
	```


## 提交格式规范
commit Message 格式

 <**type**> (<**scope**>):<***subject***>


**type必须为一下几种之一**

- feat: 特性提交
- fix:BUG修复提交
- docs:文档修改提交
- style:代码格式修改提交(空格,格式,缺失符号等)
- refactory:不是特性也不是修复的提交???
- perf:性能优化
- test:测试用例
- chore:构建工具,类库等修改

**scope 为影响范围,可选**

- 修改位于的模块
- 修改影响的分支或者功能

**subject 为针对此次提交的文字描述**

- 错误/修改的描述
- 修改的原因和理由等

## BUG的检出与合并
1. 当已发布版本或者已测试通过的版本上面出现BUG,要新建hotfix分支,进行修改
2. 测试也在此分支上进行
3. 通过后更新到所有的分支上


## 文件名和文件编码

1. 文件的命名尽量不要使用中文
2. 文本文件(包括脚本,sql,源码文件等),编码上建议统一的使用UTF-8,换行为UNIX(LF)(可以使用dos2unix 工具进行批量转化),便于文件的对比和冲突解决

## readme的编写
1. 仓库中的每个二级目录(如**./APP/**或者**./doc/**)下都需建立readme,命名不统一,可以是readme/README/README.md(markdown格式将在未来添加支持),但是要以纯文本格式编写
2. doc目录下的文档,除文档的结构过于复杂或者内容较长或者内容有图形的,其他建议以文本方式进行书写.

## end
目前的规则仅限于目前的情况,目前我们对于git的使用还不熟练,对于git工作方式的理解还不完全,处于一个思维方式转变的过程中.当现有的规则无法帮助我们,而是限制我们的时候,则会考虑再优化这个工作流.

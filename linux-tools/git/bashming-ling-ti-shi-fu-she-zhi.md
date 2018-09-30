## 都配置号了之后
在home下的显示
```
[alex@alex-ubuntu ~]@~
>>> 
```
在仓库中的显示
```
[alex@alex-ubuntu ~/git_space/VS-XXXXDN-EM]@VS-XXXXDN-EM
(develop)>>> 
```
修改仓库后显示
```
[alex@alex-ubuntu ~/git_space/VS-XXXXDN-EM]@VS-XXXXDN-EM
(develop *)>>>
```


## 配置

### 配置PS1

`alex.sh`是一个自定义的配置文件,可以放在home下或者随便什么地方,这里放在了`/etc/profile.d/`,这里会被自动的调用

cat /etc/profile.d/alex.sh
```
#　#[alex@alex-ubuntu ~] @~
#>>>

PS1_1='[\u@\h \[\e[31;1m\e[1m\]\w\[\e[0m]\]'
PS1_2='@\[\e[4m\e[32;1m\]\W\[\e[0m\]\r\n'
PS1_3='$(__git_ps1 "(\[\e[32;1m\]%s\[\e[0m\])")\[\e[1m\]>>>\[\e[0m\] '

export PS1="$PS1_1$PS1_2$PS1_3"

#为了实时的更新PS1
PROMPT_COMMAND='export PS1="$PS1_1$PS1_2$PS1_3"'
```
执行了上面的配置之后,可能会报错误,找不到__git_ps1,或者显示不了仓库状态,继续下面的配置

### 配置git-prompt

ubuntu安装了git之后会自动的在补全中安装git-prompt 文件,我这个是安装时就安装好的文件,稍加一些修改就行

cat /etc/bash_completion.d/git-prompt 
```
# In git versions < 1.7.12, this shell library was part of the
# git completion script.
#
# Some users rely on the __git_ps1 function becoming available
# when bash-completion is loaded.  Continue to load this library
# at bash-completion startup for now, to ease the transition to a
# world order where the prompt function is requested separately.

#这些都是配置__git_ps1显示的
export GIT_PS1_SHOWDIRTYSTATE=1
export GIT_PS1_SHOWSTASHSTATE=1
export GIT_PS1_SHOWUNTRACKEDFILES=1
export GIT_PS1_SHOWUPSTREAM="verbose git svn"

if [[ -e /usr/lib/git-core/git-sh-prompt ]]; then
        . /usr/lib/git-core/git-sh-prompt
fi
```

如果是源码安装,则在源码中找找就行,在源码中的补全相关的路径下名字是`git-sh-prompt`
将这个文件拷贝到自己的配置目录中,在配置文件中添加`. git-sh-prompt`即可
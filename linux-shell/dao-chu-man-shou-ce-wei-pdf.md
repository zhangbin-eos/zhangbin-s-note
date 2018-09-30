经常man一些shell命令，有时候有想导出来编辑或注释一下，所以要导出。方法有很多种，根据自己的实际需要觉得比较实用的记录下分享一下。

1. 导出成txt
```
man –t bash |col –b > bash_man.txt
```
这个是大家经常使用的，导出成txt文件，格式基本正确

2. 导出成pdf
```
man –t bash |ps2pdf – bash_man.pdf
```
这个是最近学习到的，可以导出成PDF格式，方便查看，也插方便的。

3. 导出成html
```
man -t --html=/usr/bin/firefox  bash
cp /tmp/hmanLSa2jh/bash.html /home/talen/Documents
```

或者在使用firefox另存页面

这个默认是使用elinks在终端下打开man页面，也可以自己指定浏览器，我指定了firefox作为打开的浏览器

这个也是我比较喜欢的，显示格式算是比较完美的。

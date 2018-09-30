# 命令行使用
## 设置
1. 右击keil4/keil5的快捷方式,选择"打开文件位置"
2. 将文件路径拷贝下来,设置到PATH中.(计算机->属性->高级系统设置->环境变量)
3. 打开cmd.exe(或者powershell)

## 测试过程

1. 重编译目标VS_88UHD_LPC1857,并将信息输出到"C:\temp\log.txt",不显示GUI
```C:\Users\liguo> uv4 -r -j0 VS_88UHD_LPC1857.uvproj -t "VS_88UHD_LPC1857" -o "C:\temp\log.txt"```

2. 编译目标VS_88UHD_LPC1857,并将信息输出到"C:\temp\log.txt",不显示GUI
```C:\Users\liguo> uv4 -b -j0 VS_88UHD_LPC1857.uvproj -t "VS_88UHD_LPC1857" -o "C:\temp\log.txt"```

3. 下载目标VS_88UHD_LPC1857到flash,并将信息输出到"C:\temp\log.txt",不显示GUI
```C:\Users\liguo> uv4 -f -j0 VS_88UHD_LPC1857.uvproj -t "VS_88UHD_LPC1857" -o "C:\temp\log.txt"```

4. -r/-b/-f 几个选项间不能同时使用


## 意义

1. 命令行的形式对于开发来说意义不大,但是对于集成和自动化测试来说,命令行意味着操作能够以文件的形式保存和执行
2. 是否能对生成效率进行改善?(目前的生成方式是什么样的)


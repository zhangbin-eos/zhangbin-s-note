# net-phy-driver

网络驱动的结构:

socket--> 网络协议(网卡)-->数据收发(phy)

## phy驱动

整个phy的驱动其实是一个状态机,通过中段或者轮训phy的状态,执行不同的操作

### 涉及的文件
在drivers/net/phy/下,
```
drivers/net/phy/built-in.o
drivers/net/phy/libphy.o
drivers/net/phy/mdio_bus.o    
drivers/net/phy/phy.o
drivers/net/phy/phy_device.o
drivers/net/phy/smsc.o
```
#### 文件的作用
phy的驱动划分了两个部分,一个是驱动部分,这部分是一个通用的逻辑流程,一个状态机;一个是设备描述部分,描述芯片的特性,功能,一些芯片状态获取接口和设置接口等

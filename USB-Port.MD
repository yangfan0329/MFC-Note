1.关于设备的GUID有两种:
  一种是叫设备类GUID，也就是你指的USB设备GUID、软盘GUID，这些都是windows指定的GUID，是确定的（可查看文件devguid.h）
  另一种叫接口GUID，是针对单独的设备GUID，由驱动程序指定，也就是说你可以在驱动里随意设置一个GUID

  两种GUID都可以用来enum某个设备，具体查看系列函数
   hardwareDeviceInfo = SetupDiGetClassDevs(
                           pGuid,
                           NULL, // Define no enumerator (global)
                           NULL, // Define no paraent window
                           (DIGCF_PRESENT | // Only Devices present
                            DIGCF_INTERFACEDEVICE)); // Function class devices.

  最后一个参数指出pGuid表示一个接口GUID而不是设备类GUID
  
  
一个支持即插即用的设备，有2个不同的GUID相关，一个设备接口GUID， 一个是设备类GUID

设备类GUID:定义了广泛意义上一类设备的GUID，如果你打开设备管理器[我的电脑右键—>设备管理器],默认的是按照“类型”排列的，每一个“类型”就是一个设备类，同时每一个设备类有一个唯一的ID就是设备类GUID。设备GUID定义了此类设备的图标、默认的安全设置、安装属性（例如用户不能手动安装这类设备，而必须通过PNP来遍历），以及其他的设置信息。设备类GUID没有定义对应的I/O接口(请参考术语表),而更像是设备的分组。我认为一个比较好的例子是端口类。串口COM和并口LPT 都是端口类的一部分，但其各有各的I/O接口，而且彼此互不兼容.一个设备仅仅属于一个设备类。我们可以通过设备驱动的INF文件的开头来查看该设备的设备类GUID。

设备接口GUID：定义了相互关联I/O接口的GUID，每一个接口GUID的具体实例都支持基本的I/O设置。设备接口GUID也是对应的驱动程序基于PNP状态来注册、启用、禁用设备。如果需要，一个设备甚至可以注册多个同样GUID的实例(假使每个都有相同的名字)[注：在实际的程序中，多次插拔USB口，确实会驱出相同的串口，例如port12,port12,port12…]，尽管在现实世界中完全不需要这样。一个简单的I/O关联接口是键盘设备，每个键盘设备的接口GUID必须相同。

可以通过如下的注册表路径来查看当前设备类GUID， 设备接口GUID：

\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class
\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\DeviceClasses
常用设备的接口GUID如下：

设备接口名称

GUID

USB Raw Device/USB设备

{a5dcbf10-6530-11d2-901f-00c04fb951ed}

Disk Device/磁盘设备

{53f56307-b6bf-11d0-94f2-00a0c91efb8b}

Network Card/网卡

{ad498944-762f-11d0-8dcb-00c04fc3358c}

Human Interface Device (HID)/人机界面设备

{4d1e55b2-f16f-11cf-88cb-001111000030}

Palm/手持设备

{784126bf-4190-11d4-b5c2-00c04f687a67}

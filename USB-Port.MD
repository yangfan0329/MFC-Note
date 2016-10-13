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

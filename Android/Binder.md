## Binder

Android IPC跨进程通讯的一种机制，实现了IBinder接口，在Framework中是连接Manager和Service的桥梁</br>（例如PackageManager - PackageManagerService）

![](http://upload-images.jianshu.io/upload_images/2154124-bd83d477ef791b81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://img.blog.csdn.net/20170531235617078?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTI0MDg3Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

其中`Messenger`基于`AIDL`，而`AIDL`称为Android接口描述语言，是基于`Binder`的封装方式来做处理跨进程通信。`Linux`用户进程都有独立空间，且不能直接操作系统内核的空间

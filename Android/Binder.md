## Binder

[图文详解 Binder](http://blog.csdn.net/carson_ho/article/details/73560642)

Android IPC跨进程通讯的一种机制，实现了IBinder接口，在Framework中是连接Manager和Service的桥梁</br>（例如PackageManager - PackageManagerService）

![](http://upload-images.jianshu.io/upload_images/2154124-bd83d477ef791b81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://img.blog.csdn.net/20170531235617078?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTI0MDg3Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

其中`Messenger`基于`AIDL`，而`AIDL`称为Android接口描述语言，是基于`Binder`的封装方式来做处理跨进程通信。`Linux`用户进程都有独立空间，且不能直接操作系统内核的空间

`Client`通过获取到IBinder调用 **transact** 函数将方法细节传入，由Binder驱动完成调用，并且会在Binder线程池中执行  **onTransact** 方法，将方法参数传入 `Server` 的函数

### AIDL 数据流向

以下都是针对多进程：

Client端获取Server的对象每次都不是同一个实例

#### in

表示数据从Client流向Server

- Client端对应数据的改动不会影响Server端
- Server端对应数据的改动不会影响Client端

#### out

表示数据从Client流向Server，输出型

- Server端接受不到Client端发送的数据，但是可以通过实例对象来操作Client端的对象
- Client端发送out输出型数据后，实例对象内的数据就为空了

#### inout

表示数据是双向流通的

- Server端可以接收到来自Client端的数据，并且可以直接修改Client端的数据
## Service

和线程的区别在于，线程执行完毕后就结束了，而Service会尽可能存活着

Service有两种方式开启，**startService** 和 **bindService** 区别在于：一个开启后在Activity销毁后不会关闭服务，并且不能直接调用Service中的方法，另外一个是绑定的方式（在Activity销毁时对其解绑），可以获取到远程Binder对象来执行方法

其中当Service还存活时重复startService，不会多次回调onCreate，绑定着的服务无法通过stopService来关闭

- startService：onCreate-onStartCommand

- bindService：onCreate-onBind

![](http://images.cnitblog.com/blog/325852/201303/24233205-ccefbc4a326048d79b111d05d1f8ff03.png)

IntentService：带有工作线程的service，原理基于HandlerThread
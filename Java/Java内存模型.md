## Java内存模型

> 模型定义了Java虚拟机（JVM）在计算机内存（RAM）中的工作方式，规定了线程工作内存（私有内存）与JVM堆内存之间的交互、同步细节，实现数据之间的协同与安全，JVM内存中包含以下区块

- **方法区** Method Area：存放已经被JVM加载的类信息，编译器编译后的代码信息，
静态变量，常量池

- **堆内存** Heap：线程共享内存，JVM的主内存，存放变量的引用空间

- **虚拟机栈** VM Stack：线程私有内存，记录变量的引用地址

- **本地方法栈** Native Method Stack：线程私有内存

- **程序计数器** Program Counter Register：线程私有内存，记录程序执行的位置，例如if-else，while，try-catch

![](http://upload-images.jianshu.io/upload_images/1685558-eb8d84cfa8fc186f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**线程同步问题是由线程的私有内存和堆内存之间不同步导致的**
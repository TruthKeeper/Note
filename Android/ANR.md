## ANR

> Application Not Responding

- 按键、触摸事件 **5S** 无响应
- 广播 **10S** 未处理完成
- 服务在 **10S** 未处理完成

### [BlockCanary工具检测](https://github.com/markzhai/AndroidPerformanceMonitor)

核心原理通过对主线程的Looper设置日志输入器，由于在Looper的loop方法会在每次调度Message时调用，通过判断是系统时间和当前线程的时间差值是否大于阈值，判断是否发生了ANR

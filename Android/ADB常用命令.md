## ADB常用命令

> adb shell dumpsys activity activities

查看当前Activity栈中的顶层

> adb shell am start -W [packageName]/[packageName.MainActivity]

- **ThisTime**:一般和**TotalTime**时间一样，除非在应用启动时开了一个透明的Activity预先处理后，再显示出主Activity，这样将比**TotalTime**小。 
- **TotalTime**:应用的启动时间，包括创建进程+App初始化+Activity初始化到界面显示。 
- **WaitTime**:一般比**TotalTime**大点，包括系统影响的耗时
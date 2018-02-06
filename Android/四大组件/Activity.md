## Activity

### 生命周期

正常的跳转：A界面 -&gt; B界面

- A.onCreate
- A.onStart
- A.onResume
- A.onPause
- B.onCreate
- B.onStart
- B.onResume
- A.onStop

> onPause 用来操作保存数据量，仅限数据量较少的情况

> 重写onSaveInstanceState，以及在onRestoreInstanceState或者onCreate中恢复数据

### LaucherMode

- Standard：默认模式，新的activity都会加入到栈顶
- SingleTop：栈顶复用模式，与默认相比，如果栈顶就是这个activity的实例，这不会重新创建，会调用onNewIntent()
- SingleTask：栈内复用模式，如果栈中已经有该activity实例时，会重用并且调用onNewIntent()并且将在该实例以上的统统出栈，一般用来作为MainActivity的LaunchMode。
**规范定义：** 开启时Android会检查所有任务栈的taskAffinity，是否有和要开启的taskAffinity相同的（Activity的taskAffinity默认是Application的taskAffinity，也就是包名），如果有，则将这个Activity在这个栈中开启，并将栈移动为前台栈，如果没有就新建一个栈在里面实例化activity
- SingleInstance：栈内复用模式加强版，独占一个栈，首次开启activity的时候创建实例到新的栈，并保证新的栈只有一个实例，适合作为闹钟等独立出去的模块，在SingleInstance中开启其他3个模式时会切换当前栈。
- taskaffinty，可以将SingleTask放入新栈，不同app设置相同的taskaffinty可以在同个栈中等等


有3个界面A，B，C，其中包名为com.tk.test，在AndroidManifest.xml中定义为A为主界面，默认lauchMode，B为singleTask，taskAffiniy为com.tk.task1，C也为singleTask，taskAffiniy为com.tk.task1。
</br>
在A中开启了B界面，B中开启了C界面，此时情况为2个栈，第一个栈也就是后台栈，栈中只有一个A，栈的taskAffinity为Application的taskAffinity也就是包名com.tk.test，第二个栈的taskAffinity为com.tk.task1，为前台栈，从底到顶为BC。
</br>
此时在C中开启A，A会被添加到第二个栈中，此时第一个栈（还是后台栈）有一个A，第二个栈（还是前台栈）从底到顶为BCA。
</br>
此时在A中开启B，栈内复用模式的特性会将CA出栈，此时第一个栈（还是后台栈）有一个A，第二个栈（还是前台栈）从底到顶为B。
</br>
此时按返回键第二个栈就不在了，会到第一个栈，也就是A界面

### 匹配规则

在AndroidManifest设置<intent-filter>，可以设置多组匹配规则
</br>
其中<action> 定义行为，多个filter只要有一个对应就能匹配到
</br>
<category>设置类型，Intent默认带有DEFAULT的类型，在AndroidManifest中要手动添加

### 启动过程

- Activity中最终到startActivityForResult()mMainThread.getApplicationThread()传入了一个ApplicationThread检查APT
- Instrumentation#execStartActivity()和checkStartActivityResult()，这是在启动了Activity之后判断Activity是否启动成功，例如没有在AM中注册那么就会报错)
- ActivityManagerNative.getDefault().startActivity()类似AIDL，实现了IAM，实际是由远端的AMS实现startActivity()
- ActivityStackSupervisor#startActivityMayWait()
- ActivityStack#resumeTopActivityInnerLocked
- ActivityStackSupervisor#realStartActivityLocked()在这里调用APT的scheduleLaunchActivity,也是AIDL，不过是在远端调起了本进程Application线程
- ApplicationThread#scheduleLaunchActivity()这是本进程的一个线程，用于作为Service端来接受AMS client端的调起
- ActivityThread#handleLaunchActivity()接收内部类H的消息，ApplicationThread线程发送LAUNCH_ACTIVITY消息给H
- 最终在ActivityThread#performLaunchActivity()中实现Activity的启动完成了以下几件事：
- 从传入的ActivityClientRecord中获取待启动的Activity的组件信息
- 创建类加载器，使用Instrumentation#newActivity()加载Activity对象
- 调用LoadedApk.makeApplication方法尝试创建Application，由于单例所以不会重复创建。
- 创建Context的实现类ContextImpl对象，并通过Activity#attach()完成数据初始化和Context建立联系，因为Activity是Context的桥接类，
最后就是创建和关联window，让Window接收的事件传给Activity，在Window的创建过程中会调用ViewRootImpl的performTraversals()初始化View。
- Instrumentation#callActivityOnCreate()->Activity#performCreate()->Activity#onCreate().onCreate()中会通过Activity#setContentView()调用PhoneWindow的setContentView()
更新界面。


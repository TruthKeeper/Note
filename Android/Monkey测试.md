## Monkey测试

通过Android自带的测试工具，模拟一系列的触摸、滑动等事件进行压力测试

adb shell monkey -p &lt;包名&gt; &lt;次数&gt;

adb shell monkey -p &lt;包名&gt; -p &lt;包名&gt; &lt;次数&gt;

-p &lt;包名&gt;</br>
多个包名则使用多个-p 标签并且加包名

-v 日志级别</br>
-v 缺省值，少量信息</br>
-v -v 详细日志</br>
-v -v -v 最详细的日志

--throttle &lt;毫秒&gt;</br>
用于模拟指定用户每次操作的间隔，不设置monkey会尽快模拟一次操作

&gt; &lt;日志路径&gt;
![](http://upload-images.jianshu.io/upload_images/1949836-e6783b371272be0f.png?imageMogr2/auto-orient/strip)

## Cold Observable

- 当一个Observer订阅Cold Observable时，Cold Observable会重新开始发射事件到该Observer
- 当多个Observer订阅同一个Cold Observable，它们收到的事件是相互独立的
- 当一个Observer取消订阅Cold Observable后，Cold Observable会停止发射事件给该Observer，但不会停止发射事件给其他的Observer

## Hot Observable

通过 **publish** 或者 **replay** 方法可以将Cold Observable转换成一个Hot Observable

- 无论Hot Observable有没有被Observer订阅，只要调用了 **connect** 方法，Hot Observable就开始发送事件
- **connect** 方法会返回一个Disposable对象，调用了dispose方法，Hot Observable将会停止发送数据，所有Observer也无法收到数据
- 当一个Observer订阅Hot Observable后，该Observer会收到在订阅之后，Hot Observable发送的事件。
- 当多个Observer订阅到同一个Hot Observable时，它们收到的事件是相同的。
- 当一个Observer取消对Hot Observable的订阅，不会影响其他Observer收到事件
 
## 转换

### refObservable

Hot Observable可通过 **refCount** 转换成一个特殊的refObservable

- 第一个Observer订阅到refObservable后，Cold Observable开始发送事件
- 之后的Observer订阅到refObservable后，只能收到在订阅之后Cold Observable发送的事件
- 如果一个Observer取消订阅到refObservable后，假如它是当前refObservable的唯一一个订阅的Observer，那么Cold Observable会停止发送事件，否则，Cold Observable仍然会继续发送事件，其它的Observer仍然可以收到Cold Observable发送的事件

### autoObservable

Hot Observable可通过 **autoConnect** 转换成一个特殊的autoObservable

- 当有N个Observer订阅autoObservable后，Cold Observable开始发送事件
- 之后的Observer订阅到refObservable后，只能收到在订阅之后Cold Observable发送的事件
- 只要Cold Observable开始发送事件，即使所有订阅autoObservable的Observer取消了订阅，Cold Observable也不会停止发送事件
- 使用autoConnect中Consumer返回的Disposable对象来使Cold Observable停止发送事件




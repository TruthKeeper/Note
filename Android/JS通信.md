## JS通信

### 调用JS方法

WebView中通过`loadUrl("javascript:show('xxx');");`就可以调用

**缺点：**无法获取到JS方法的返回值，但是iOS可以获取到(;¬_¬)

### JS调用Android方法

`addJavascriptInterface(new AndroidObj(), "androidObj");`

### JsBridge的开源方案

通过拦截`shouldOverrideUrlLoading`来进行封装，使其支持双向通信的回调
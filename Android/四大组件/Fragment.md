## Fragment

### 生命周期

- setUserVisibleHint(在ViewPager中会调用)
  
- onAttach(绑定Context)
  
- onCreate
  
- onCreateView(绑定view视图)
  
- onViewCreated(视图创建完毕)
  
- onActivityCreated(activity onCreate执行完毕)
  
- onStart(activity onStart后)
  
- onResume(activity onResume后)
  
- 。。。
  
- onDestoryView
  
- onDestory
  
- onDetach
  
- onDestory(activity的)

![](https://user-gold-cdn.xitu.io/2017/10/10/59d579beadc3a3efe65cf8587c9e65c9?imageView2/0/w/1280/h/960/ignore-error/1)

### 创建方式

- XML中&lt;fragment&gt;标签
- FragmentManager添加
- ViewPager中

### 通信方式

- getArguments：Fragment获取Activity的初始化参数
- getActivity：Fragment获取Activity实例，或者用[APT的方式](https://github.com/hongyangAndroid/FABridge)
- FragmentManager操作Fragment

### 懒加载

在ViewPager中`setUserVisibleHint`回调时机比其他生命周期都要早，在LazyFragment中记录一个boolean来判断是否视图加载完毕，在`onActivityCreated`中设为true，并且通过getUserVisibleHint来判断是否执行懒加载

### Adapter

`FragmentStatePagerAdapter`和`FragmentPagerAdapter`的区别：
- FragmentStatePagerAdapter适合多个数量级的Fragment，会销毁回收，节约内存
- FragmentPagerAdapter不会彻底销毁，并且在意外关闭后会重新恢复Fragment（而不是重新实例化）
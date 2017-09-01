## SnapHelper原理

> 用于在用户滑动后对其进行条目的偏移修正，实现类似`ViewPager`的效果，默认2个实现，`LinearSnapHelper`，`PagerSnapHelper`。</br>
其中`LinearSnapHelper`支持一次滑动多页

SnapHelper的`attachToRecyclerView`方法实现了对RecyclerView的引用绑定，内部对其额外监听了滚动和惯性滚动（addOnScrollListener和setOnFlingListener），SnapHelper内部有一个`Scroller`，用来做平滑滚动。</br>
在 **Scroll监听** 中，滚动状态为IDLE（静置）时，计算修正的滚动位置，并且平滑滚动。</br>
在 **Fling监听** 中，根据当前的X、Y方向速度计算出应该滚到最后的Position位置。

### SnapHelper 3大抽象实现

```
public abstract int findTargetSnapPosition(LayoutManager layoutManager, int velocityX, int velocityY)
```

该方法在 **Fling** 操作时触发，需要返回RecyclerView需要滚动到哪个位置，该位置对应的ItemView就是那个需要进行对齐的列表项。如果找不到targetSnapPosition，就返回RecyclerView.NO_POSITION，在 **Fling** 监听中返回false，惯性滚动，如果指定位置，就不惯性滚动，用RecyclerView内置的SmoothScroller平滑滚动

```
public abstract View findSnapView(LayoutManager layoutManager)
```

该方法在 **Scroll** 操作时和绑定RecyclerView时触发，通过该方法来找到当前layoutManager上最接近对齐位置的那个view，如果返回null，就表示没有需要对齐的View，也就不会做滚动位置进行偏移修正。

```
public abstract int[] calculateDistanceToFinalSnap(@NonNull LayoutManager layoutManager, @NonNull View targetView);
```

计算当前View和需要对齐View的坐标之间的距离。该方法返回一个大小为2的int数组，分别对应X轴，Y轴方向上的距离。


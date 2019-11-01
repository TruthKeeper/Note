# Dart

## 特殊点

- 类型为数字的 变量如何没有初始化其值也是 null，数字类型也是对象
- 只有 `true` 对象才被认为是 true。 所有其他的值都是 flase。这点和 JavaScript 不一样， 像 `1`、 `"aString"`、 以及 `someObject` 等值都被认为是 false


## 语法糖

- `b ??= value`; // 如果 b 是 null，则赋值给 b；// 如果不是 null，则 b 的值保持不变
- `p?.y = 4;` // If p is non-null, set its y value to 4.

### 构造函数

```dart
class Point {
  num x;
  num y;

  Point(this.x, this.y);
  // 等价于如下
  // 只有当名字冲突的时候才使用 this。否则的话， Dart 代码风格样式推荐忽略 this
  //Point(num x, num y) {
  //  this.x = x;
  //  this.y = y;
  }
}
```



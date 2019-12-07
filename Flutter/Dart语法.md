# Dart

## 特殊点

- 类型为数字的变量如果没有初始化其值也是 null，数字类型也是对象
- 只有 `true` 对象才被认为是 true。 所有其他的值都是 flase。这点和 JavaScript 不一样， 像 `1`、 `"aString"`、 以及 `someObject` 等值都被认为是 false，**不能用于if判断**
- 没有关键词 **public** 、**private** 等修饰符，_ 下横向直接代表 **private**
- Dart 中 final 和 const 表示常量，比如 `final name = 'GSY'; const value= 1000000;` 同时 `static const` 组合代表了静态常量。其中 const 的值在编译期确定，final 的值要到运行时才确定
- 不支持`print("aaaa" + i)`，需要`+i.toString()`
- var变量**一旦赋值，类型便会确定，则不能再改变其类型**，只作为一个类型推断的语法糖
- 类定义中所有的变量, Dart语言都会隐式的定义 setter 方法，针对非空的变量会额外增加 getter 方法
- 


## 语法糖

- `b ??= value`; // 如果 b 为空的时候赋值给 b
- `p?.y = 4;` // 如果p不为空的时候，赋值给y属性
- `AA ~/999` //AA 对于 999 整除
- 数组展开`var list; var list2 = [0, ...?list];`可用?判空

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

### 级联操作符

```
event
  ..id = 1
  ..type = ""
  ..actor = "";
```


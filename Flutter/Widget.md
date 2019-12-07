# Widget

### Text

```dart
new Text(
          '哈哈哈哈哈',// 文字内容
          textAlign: TextAlign.center,// 文字对齐方式
          style:TextStyle(
            fontSize: 30, //文字大小
            color: Color(0xffff6262), //颜色
            decoration: TextDecoration.underline, //文字装饰
            decorationStyle: TextDecorationStyle.solid //文字装饰风格
          )
        )
```

### Container

> 最常见的默认布局，只能包含一个child，支持padding，margin，背景，边框，阴影

- color属性和decoration属性都表示修饰背景，不允许同时存在

#### 圆角按钮

```dart
Container(
        child: Text(
          '哈哈哈哈',
          style: TextStyle(
              fontSize: 16, color: Color.fromARGB(255, 255, 255, 255)),
        ),
        alignment: Alignment.center,
        width: 120,
        height: 40,
        decoration: BoxDecoration(
            color: Color.fromARGB(255, 255, 0, 0),
            border: Border.all(
                color: Colors.blueAccent, width: 2, style: BorderStyle.solid),
            borderRadius: BorderRadius.circular(20)),
      )
```

### Image

- fit属性：`BoxFit.cover`



### Row、Column

- `mainAxisAlignment`: `MainAxisAlignment.center` Row：横向居中，Column：竖向居中
- `mainAxisSize`: `MainAxisSize.max`默认是max，即match_parent，可以设置成`.min`，即wrap_content

#### Expanded

> 在Row和Column中代表了填充，默认带有`flex:1`的属性

### 


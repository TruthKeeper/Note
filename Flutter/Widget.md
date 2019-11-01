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

- fit属性：`BoxFit.cover`,`BoxFit.cover`
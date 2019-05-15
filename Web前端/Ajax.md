# Ajax

## 发起一个请求

```javascript
//1.创建对象
var xhr = new XMLHttpRequest();
//2.初始化请求
xhr.open('GET', 'url')
//4.添加接受
xhr.onreadystatechange = function () {
    switch (xhr.readyState) {
        case 2://接收到响应头
            console.log(xhr.getAllResponseHeaders());
            break;
        case 3://正在下载响应体
            break;
        case 4://接收到响应体
            var res = JSON.parse(xhr.responseText);
            break;
    }

};
//3.发送请求
xhr.send();
```

## 响应的区别

- `response`会根据`responseType`的变化而变化
- `responseText`永远是字符串形式的响应体

## 模板引擎

1. 选择一个模板引擎
2. 下载JS引擎
3. 导入到页面中
4. 准备一个模板
5. 准备一个数据
6. 通过模板引擎的JS函数将模板和数据整合到html中
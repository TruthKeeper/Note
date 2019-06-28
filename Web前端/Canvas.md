# Canvas

## 开始

```javascript
//获取到绘图元素
/** @type {HTMLCanvasElement} */
var canvas = document.querySelector('.canvas')
//获取到绘制工具箱
var ctx = canvas.getContext('2d')
```

## 填充区域

```javascript
ctx.fillStyle="#FF0000"
ctx.fillRect(0,0,150,75)
```

## 画线

```javascript
ctx.moveTo(100,100)
ctx.lineTo(200,150)
ctx.stroke()
```

## 画圆

```javascript
ctx.beginPath()
ctx.arc(100, 100, 50, 0, 1.5 * Math.PI)
ctx.stroke()
```

## 画文字

```javascript
ctx.font='20px Microsoft Yahei,sans-serif'
//实心文字
ctx.fillText('哈哈哈',50,20)
//空心文字
ctx.strokeText('哈哈哈',50,20)
```




# JavaScript

> 基于***对象***和***事件驱动***的的***多范式脚本***语言
>
> ## script标签的特点
>
> - 标签内的`innerHtml`不会显示在界面上
> - 如果`type`不等于`text/javascript`的话，内容不会作为JS代码执行

##  对象

> 在JS中，对象就是键值对的集合，

- `Object.prototype.toString.call`原型方法打印对象的具体类型

### 抽象性

> 如果需要用一个对象描述一个数据，需要抽取这个对象的核心数据

1. 抽取核心数据
2. 不在特定条件下不知道是什么

### 封装性

> 将属性与方法组合、封装到一起

1. 如果键值是数据，就称为对象的属性
2. 如果键值是函数，就称为对象的方法，方法就是将过程封装起来

### 继承

> 实现复用的手段，在JS中没有明确的继承语法，一般都是按照继承的**理念**来实现对象的成员扩充

#### 原型链模式

> 利用原型来实现，实现复用，只要原型有，那我也就有

***特点***：

1. **简单**，一目了然
2. 父类**新增原型属性**后，子类也能访问到

***缺点***：

1. 来自原型对象的所有属性都是**共享**的，对引用类型的操作也是**共享**的
2. 在创建子类实例的时候，无法为**父类构造函数传递参数**
3. 无法实现**多继承**

```javascript
function Parent(name) {
    this.name = name;
    this.sex = true;
}
Parent.prototype.say = function() {}

function Child(id) {
    this.id = id;
}

Child.prototype = new Parent("hhh");
Child.prototype.constructor = Child;

var child = new Child(1);
console.log(child);
console.log(child instanceof Child); //true
console.log(child instanceof Parent); //true
```

#### 构造继承模式

> 借用父类的构造函数来增强子类示例，等于是复制父类的实例属性给子类

***特点***：

1. 创建子类示例的时候，可以为**父类构造函数传递参数**
2. 可以实现**多继承**
3. 返回的是**子类实例**

***缺点***：

1. 只能继承父类的**实例属性**，**不能继承原型属性**
2. 复用性差，每个子类都有父类实例函数，**消耗内存**

```javascript
function Parent(name) {
    this.name = name;
    this.sex = true;
}
Parent.prototype.say = function() {}

function Child(name,id) {
	Parent.call(this, name);
    this.id = id;
}

var child = new Child("张三",1);
console.log(child);
console.log(child instanceof Child); //true
console.log(child instanceof Parent); //false
```
#### 实例继承模式

> 通过给父类的实例添加新属性，作为子类的实例返回

***特点***：

1. 返回的是**父类实例**，拥有原型属性
2. 不限制调用，new 子类() 的效果等同 子类() 的返回对象

***缺点***：

1. 不支持**多继承**
2. 由于实例是父类，所以function不是标准意义上的构造函数，不能**添加原型属性**

```javascript
function Parent(name) {
    this.name = name;
    this.sex = true;
}
Parent.prototype.say = function() {}

function Child(name, id) {
    var p = new Parent(name);
    p.id = id;
    return p;
}

var child = new Child("张三", 1);
console.log(child);
console.log(child instanceof Child); //false
console.log(child instanceof Parent); //true
```

#### 拷贝模式

> 将别的属性拷贝到我的身上，就拥有这个成员了

***特点***：

1. 支持**多继承**

***缺点***：

1. 如果拷贝的成员是引用对象的话，修改之后会在父类的子类实例之间**共享**
2. **性能较低**
3. 无法获取到**不可枚举的方法**
```javascript
function Parent(name) {
    this.name = name;
    this.sex = true;
}
Parent.prototype.say = function() {}

function Child(name, id) {
    var p = new Parent();
    for (var v in p) {
        this[v] = p[v];
    }
    this.name = name;
    this.id = id;
}

var child = new Child("张三", 1);
console.log(child);
console.log(child instanceof Child); //true
console.log(child instanceof Parent); //false
```
#### 构造继承+原型链模式

> 将构造继承模式和原型链模式组合起来

***特点***：

1. 返回对象既是**父类的实例**，也是**子类的实例**
2. 集两家之长，既可以访问到父类的属性，也可以获取到父类的原型属性，父类**新增原型属性**后，子类也能访问到
3. 可以给**父类构造函数传递参数**

***缺点***：

1. 不支持**多继承**
```javascript
function Parent(name) {
    this.name = name;
    this.sex = true;
}
Parent.prototype.say = function() {}

function Child(name, id) {
    Parent.call(this, name);
    this.id = id;
}

Child.prototype = Parent.prototype;
Child.prototype.constructor = Child;

var child = new Child("张三", 1);

Parent.prototype.talk = function() {}
console.log(child);
console.log(child instanceof Child); //true
console.log(child instanceof Parent); //true
```

### 构造函数

> 初始化数据，在JS中的**构造函数**是用来初始化属性的，不是用来**创建对象示例**的，**new**才是

### 创建对象的过程

> 构造原型示例三角形关系

```javascript
function Person() {}
var p = new Person();
```
1. 初始化Person**构造函数**，初始化**prototype**原型属性，并且将**prototype**对象内部的**constructor**属性指向Person**构造函数**
2. 申请内部空间，执行构造函数，将this指向当前对象，如果支持**\_\_proto\_\_**属性的话，创建此属性，将其指向**prototype**对象
3. 将内存地址赋给p

## 字符串String

- `padStart`：补全函数，传递补全的位数和补全的字符串，从字符串开头开始，`padEnd`从字符串尾开始
- `indexOf`：查询字符串中指定字符串的索引，函数第二个参数表示开始查询的下标
- `includes`：返回字符串中是否包含指定字符串，函数第二个参数表示开始查询的下标
- `startsWith`：是不是以某个字符串作为开头，`endswith`，是不是某个字符串作为结尾
- `repeat`：将字符串重复几遍返回，源字符串不变

## 数组Array

[JavaScript数组所有API全解密](https://juejin.im/post/5902d56e1b69e60058c634d6)

### 判断是否是一个数组

```javascript
var a = [];
// 1.基于instanceof
a instanceof Array;
// 2.基于constructor
a.constructor === Array;
// 3.基于Object.prototype.isPrototypeOf
Array.prototype.isPrototypeOf(a);
// 4.基于getPrototypeOf
Object.getPrototypeOf(a) === Array.prototype;
// 5.基于Object.prototype.toString
Object.prototype.toString.apply(a) === '[object Array]';

function P () { }
P.prototype = Array.prototype
var a = new P()

```

> 上述前4种在修改原型链后都返回`true`，所以判断是否是一个数组的标准方式应该是`Object.prototype.toString.apply(a) === '[object Array]'`或者ES6中的`Array.isArray`

### 常见API


- `push`：接受任意数量的参数，将其添加到数组末尾，返回数组的新长度，可以通过`Array.prototype.push.apply(array,array2)`来实现数组合并
- `pop`：移除末尾项，会减少数组的length，并且返回移除的项
- `concat`：将当期数组拷贝一份，将传入的任意参数添加至末尾后，返回新数组（不会影响原有数组），如果参数是数组，会展开一级数组
- `shift`：删除数组的第一项
- `unshift`：接受任意数量的参数，将其添加到数组开头，返回数组的新长度

<!-------------------------------------------------------------->

- `join`：将数组元素以分隔符的形式拼接成字符串
- `sort`：默认将每一项转换成字符串后按升序排列，可传入两参的function自定义排序
- `reverse`：反转数组


- `slice`：接受一个或两个参数，返回从指定下标到结束下标的新数组
- `splice`
  两参：起始下标，要删除的项数，返回删除的项
  三参以上：起始下标，要删除的项数，要插入的项，splice(2,1,4,6) 删除当前数组的下标2，从下标2开始插入4和6
- `indexOf`、`lastIndexOf`：查找项在数组中的位置，可传入第二个参数，开始查找的索引位置

<!-------------------------------------------------------------->

- `Array.of`：语义性更强，仅表示传入的参数都转换成数组
- `copyWithin`：传递三个参数，分别是目标索引，开始索引和结束索引，`['a','b','c','d'].copyWithin(0, 1, 2)`指将1号位的b拷贝覆盖到第0位
- `fill`：填充数组，传递填充的值，开始填充的位置，结束的位置



#### forEach

> 没有返回值，只是针对每个元素调用func（没有返回值，如果里面有操作方法就会改变原数组）

```javascript
var array = '12345678'.split('');
array.forEach(function (item, index, array) {
    array[index] = Number(item) + 1;
});
console.log(array);
```

#### map

> 返回一个新的Array，每个元素为调用func的结果（并没有改变原数组）

```javascript
var array = 'abcdefg'.split('');
var newArray = array.map(function (item, index, array) {
    return item + '=';
});
console.log(newArray);
```

#### filter

> 返回一个符合func条件的元素数组（并没有改变原数组）

```javascript
var array = '12345678'.split('');
var newArray = array.filter(function (item, index, array) {
    return item > 3;
});
console.log(newArray);
```

#### some

> 返回一个boolean，判断**是否有元素**是否符合func条件(有一个就行)（并没有改变原数组）

```javascript
var array = '12345678'.split('');
var res = array.some(function (item, index, array) {
    console.log(item)
    return item > 5;
});
console.log(res);
```

#### every

> 返回一个boolean，判断**每个**元素是否符合func条件（所有都判断）（并没有改变原数组）

```javascript
var array = '12345678'.split('');
var res = every.every(function (item, index, array) {
    console.log(item)
    return item > 5;
});
console.log(res);
```

### findIndex

> 返回搜索到的索引值

```javascript
var array = '12345678'.split('');
var index = array.findIndex(function (item, index, array) {
    return item == '5';
});
console.log(index);
```


## DOM

> Document Object Model：文档对象模型

![](E:\Note\Web前端\DOM和BOM.jpg)

父节点
	兄弟节点
	当前节点
		属性节点
		文本节点
		子节点
	兄弟节点

### 增加

#### 创建

- **document.createElement**：创建元素节点
- **document.createTextNode**：创建文本节点
- **document.createAttribute**：创建属性节点
- **innerHTML** 
- **innerText ** 用来设置文本的，设置标签是没有效果的
- **cloneNode()**

#### 加入

- **appendChild**：追加到最后结尾处
- **innerHTML**
- **insertBefore(新元素 , 旧元素)**

##### 增加属性的三种方式

- setAttribute("xxx",yyy)
- .xxx=yyy
- ```javascript
  var attr = document.createAttribute("xxx");
  attr.nodeValue = "yyy";
  element.setAttributeNode(attr);
  ```
  
##### 其他

- **style**的操作
- **setAttribute(属性名 , 属性值)**

### 删除

- **removeChild**
- **removeAttribute**：接收属性名称来删除
- **removeAttributeNode**：接收属性对象来删除

### 修改

#### 修改节点

- 删除节点再加入

#### 修改样式

- **style.xxx=yyy**
- **setAttribute**
- **.classList.add** 添加类名
- **.classList.remove** 删除类名

#### 修改文本

- **innerText**
- **innerHtml**
- **nodeValue**：获取节点的名字，一般文本节点才使用

#### 修改属性

- **.xxx=yyy**
- **style.xxx=yyy**

### 查询

#### 基础API

- **document.getElementById**
- **document.getElementByName**
- **document.getElementByTagName**
- **document.getElementByClassName**
- **document.querySelector**传入css选择器，返回查询到的第一个
- **document.querySelectorAll**传入css选择器，返回查询到的所有结果

#### 亲属访问



#### 属性

- **getAttribute**：返回属性值
- **getAttributeNode**：返回属性节点，是一个对象



#### 其他

- **nodeValue**：节点的值，一般在文本节点使用，获取值
- **nodeName**：节点的名称，输出是大写
- **nodeType**：1：元素节点 2：属性节点 3：文本节点

## BOM

> Browser Object Module：浏览器对象模型

- 页面中的顶级对象`document`率属于浏览器的顶级对象`window`
- `onload`：页面加载完的回调
- `window.location.href`获取到当前地址
- `window.location.replace`用该方法跳转不会记录到历史中
- `window.location.reload`重新刷新
- `window.history.back()`退回到上个历史
- `window.history.forward()`前进到上个历史
- `windown.navigator.userAgent`获取浏览器类型
- `windown.navigator.platform`获取系统平台类型

### 定时器

```javascript
//开始一个反复的定时器
var intervalId = setInterval(function() {
    
}, 1000);
//移除定时器
clearInterval(intervalId);

//开始一个一次性的定时器
var timeId = setTimeout(function() {
    
}, 1000);
//移除定时器
clearTimeout(timeId);
```

## WebAPI

> 浏览器提供一套操作**DOM**和**BOM**的**API**

- `onclick`方法`return false` 或者`e.preventDefault()`会取消浏览器默认行为，例如`a`标签的跳转
- `onmouseover`鼠标经过，`onmouseout`鼠标离开
- `onfocus`获取到焦点，`onblur`失去焦点
- `addEventListener`绑定事件，不需要带on

### 获取样式

- 如果样式代码是在style标签中设置的，js默认是获取不到属性的，不过可以通过`offsetLeft`等属性获取到
- 如果样式代码是在style的属性中设置（行内样式）的，js可以获取到
- 通过`window.getComputedStyle`获取到元素计算后的样式

### offset

- `offsetLeft`：当前元素 左边框 外边缘 到 最近的已定位父级（`offsetParent`） 左边框 内边缘的距离。如果父级都没有定位，则分别是到 `body` 顶部 和左边的距离
- `offsetWidth`是包含边框的

### scroll

- `scrollWidth`：元素中内容的实际宽度（不包含边框），如果没有内容，就是元素无边框的宽
- `scrollHeight`：元素中内容的实际高度（不包含边框），如果没有内容，就是元素无边框的高
- `scrollTop`：元素滚动的上距离
- `scrollLeft`：元素滚动的左距离

### client

- `clientWidth`：可视区域的宽度，不包含边框，包含padding
- `clientHeight`：可视区域的高度，不包含边框，包含padding
- `clientTop`：上边框的宽度
- `clientLeft`：左边框的宽度

## 事件

### 事件冒泡

> 多个元素嵌套，有层次关系，并且这些元素都注册了相同的事件，如果里面的元素时间触发了，外面的元素的该事件也会自动触发

取消事件冒泡

`window.event..cancelBubble=true`：IE、谷歌

`e.stopPropagation()`：谷歌、火狐

### 事件阶段

- `Event.CAPTURING_PHASE`：1：事件捕获阶段，从外向内
- `Event.AT_TARGET`：2：事件目标阶段
- `Event.BUBBLING_PHASE`：3：事件冒泡阶段，事件会从里到外（**一般都是冒泡阶段**）

`addEventListener`第三个参数，是否在捕获阶段绑定事件监听，`e.eventPhase`表示当前事件的阶段

### 事件对象

`e.clientX`属性获取在窗体可视区域内的X轴，可以通过`document.onmousemove`设置鼠标跟随图标

## 原型

```javascript
function User() {
    this.name = "user";
}

function Person() {
    this.id = 1;
}

User.prototype.do = function() {
    console.log("do it")
};

new User().do();

Person.prototype = new User();
var person = new Person();
console.log(person.name);
```


- 对象被创建的时候会自动连接上**prototype**，即**凡是对象就有原型，原型也是对象**

- 构造函数的**原型属性** 就是 创建出来的对象的**原型对象**，o.**\_\_proto\_\_** === O.**prototype**

- 由于**\_\_proto\_\_**是一个非标准对象，在早期浏览器中可能不兼容，通过**obj.constructor.prototype**来访问到

- 每个对象都有**.prototype**这个原型属性，查找属性的时候，如果找不到，会从原型中去查找（**原型链**）

- **prototype**属性中有一个属性**constructor**，用来和构造函数关联

- 通过**直接替换**的方式修改了原型对象后，一般会添加一个**constructor**属性，因为在构造函数中可能还会调用构造函数，所以在内部应该使用**this.constructor()**
   ```javascript
   function Person() {}
   Person.prototype = {
    constructor = Person;
    //...
   } 
   ```



## 函数

1. 函数是对象，就可以使用对象的**动态特性**
2. 函数是对象，构造函数可以创建函数
3. 函数是函数，可以创建其他对象
4. 函数是唯一可以限定变量作用域的结果
5. 若函数无返回值，则默认返回undefined
6. 实参和形参的个数可以不相等
7. 没有重载（函数名可以相同，参数个数不同）的概念,后面的会覆盖前面的

### 创建函数

```javascript
function fun2() {
    console.log("hhh");
}
//等价于如下
var fun1 = new Function('console.log("hhh");')
```

```javascript
function fun2(arg0, arg1) {
    console.log(arg0 + " " + arg1);
}
//等价于如下
var fun1 = new Function('arg0', 'arg1', 'console.log(arg0 + " " + arg1);')
```

#### new Function

> new Function(arg0,arg1,arg2,body)

- Function 中的所有参数都是字符串，可以在**程序运行过程中**构建函数
- 如果有多个参数，最后一个为函数体
- 如果没有参数，则表示创建一个空函数
- 在使用`var o=(new Function('{name:"hhh",id:1}'))()`的问题，由于在识别花括号之后，会把对象属性的冒号申明解析成**标记语言**，例如break指定循环的位置（Java），此时需要在{}外面包裹一层()


### arguments对象

> **伪数组对象**，表示在函数调用过程中传入的所有参数的集合，在每一个函数代码体中**都有**一个默认的对象arguments，存储着**实际传入的所有参数**。如果编程中需要变长参数的函数，就设计成**无参函数**，利用arguments

```javascript
function getMax( /*...*/ ) {
    var max = arguments[0];
    for (var i = 1; i < arguments.length; i++) {
        if (max < arguments[i]) {
            max = arguments[i];
        }
    }
    return max;
}
console.log(getMax(1, 4, 6, 2, 5));//6
```

### 原型链结构

- 任意一个函数，都是**Function**的实例，相当于{}和new Obejct的关系

- Object 的构造函数是Function的一个实例

- Object 作为对象继承于Function.prototype

  

### 自调用函数

将函数视为对象，进行直接调用

```javascript
(function() { console.log("run") })()
```

### eval函数

将字符串当做函数使用，轻量级的new Function("")，***开发中不要使用***

```javascript
eval('var a = 10');
console.log(a);
```

### 两种定义

#### 函数声明

> 是单独写在一个结构里面，不存在逻辑判断，预解析的时候会提升到当前作用域的最前面（**函数提升**）

```javascript
function fun() {
    function fun1() {
        //声明
    }
    if(true) {
        function fun2() {
            //函数表达式
        }
        var f = function() {
            //函数表达式
        }
    }
}
```

#### 函数表达式

> 预解析的时候会触发**变量提升**

### this调用

#### .call

> 第一个参数指this指向的环境，后面允许多个参数传递给function

#### .apply

> 第一个参数指this指向的环境，第二个参数是传递给function的参数数组

#### .bind

> 和call类似，不过返回的是新的包装函数

## 闭包

> 一个具有封闭功能和包裹功能的一个结构，在JS中**函数可以构成闭包**，一般函数是一个代码结构的封闭结构，即包裹的特性，同时根据**变量作用域**的规则，只允许函数访问外部数据，外部不允许访问函数内部的数据
>
> 闭包是为了实现具有**私有访问空间**的函数

### 解决的问题

1. 闭包不允许外界访问
2. 就是间接访问内部的数据

### 方式

1. return函数

```javascript
function fun() {
    var o = { name: "hhh" };
    return function() {
        return o;
    };
}
var f = fun();
var v = f();
```

2. return对象

```javascript
function fun() {
    var o1 = { name: "aaa" };
    var o2 = { name: "bbb" };
    return {
        getO1: function() {
            return o1;
        },
        getO2: function() {
            return o2;
        },
        setO1: function(o) {
            o1 = o;
        },
        setO2: function(o) {
            o2 = o;
        }
    }
}
var c = fun();
```

### 潜在的问题

```javascript
var f = (function() {
    //私有数据
    var num = 10;
    return function() {
        return num;
    }
})();
f();
f = null;
```

函数执行需要内存，在函数中定义的变量会在函数执行结束后自动回收，但是在闭包结构中由于一直引用着，所以这些私有数据将**不会被回收**，需要在不使用的时候手动赋值为**null**



## Date类

- 获取年：`new Date().getFullYear()`
- 获取月：`new Date().getMonth()+1`从0开始
- 获取日：`new Date().getDate()`
- 获取周：`new Date().getDay()`从0开始
- 获取时：`new Date().getHours()`
- 获取分：`new Date().getMinutes()`
- 获取秒：`new Date().getSeconds()`

## 存储

![](E:\Note\Web前端\存储.jpg)

## this关键字

```javascript
let fn = function(){
  alert(this.name)
}
let obj = {
  name: '',
  fn
}
fn() // 方法1
obj.fn() // 方法2
fn.call(obj) // 方法3
let instance = new fn() // 方法4

```

1. 方法1，**默认绑定**，this指向了window
2. 方法2，**隐式绑定**，this指向点前面的对象
3. 方法3，**显示绑定**，利用call或者apply函数，将this的指向了第一个参数，即obj
4. 方法4，**new绑定**，实例化对象的时候，将this指向了实例对象

上述规则的优先级是递增关系

### 解析

```javascript
var number = 5;
var obj = {
    number: 3,
    fn: (function () {
        var number;
        this.number *= 2;
        number = number * 2;
        number = 3;
        return function () {
            var num = this.number;
            this.number *= 2;
            console.log(num);
            number *= 3;
            console.log(number);
        }
    })()
}
var myFun = obj.fn;
myFun.call(null);
obj.fn();
console.log(window.number);
```

#### fn自调用函数

> 即默认绑定，this的环境是window

```javascript
var number;//在闭包中定义了一个number
this.number *= 2;//即window.number*=2 ,window.number变成了10
number = number * 2;//还未赋值，无效，number变成了NaN
number = 3;//number赋值为3
```

#### myFun.call(null)

> 传的参数为null，即默认绑定，this的环境是window

```javascript
var num = this.number;//在闭包中定义了一个num,赋值为window.number的值，即10
this.number *= 2;//即window.number*=2 ,window.number变成了20
console.log(num);//打印10
number *= 3;//闭包中的number*=3， 3*3=9
console.log(number);//打印9
```

#### obj.fn()

> 隐式绑定，this指向obj对象

```javascript
var num = this.number;//在闭包中定义了一个num,赋值为obj.number的值，即3
this.number *= 2;//即obj.number*=2 ,obj.number变成了6
console.log(num);//打印3
number *= 3;//闭包中的number*=3， 9*3=27
console.log(number);//打印27
```

#### console.log(window.number)

> 输出20


## ES6

### let关键字

> 块级作用域

- let 和 const 定义的变量不会出现变量提升，而 var 定义的变量会提升
- let 和 const 不允许重复声明(会抛出错误)
- let 和 const 定义的变量在定义语句之前，如果使用会抛出错误(形成了暂时性死区)，而 var 不会

### 模板字符串

会保留换行和空格，可以实现运算和函数

```javascript
let str='123'
let c=`<h1>${str}</h1>`
```



### 箭头函数=>

> 会捕获其所在上下文的 this 值，作为自己的 this 值，箭头函数没有原型属性

```javascript
var obj = {
  data: function (arg1, arg2) {
    return arg1 + arg2
  }
}
//可缩写成如如下
var obj = {
  data: (arg1, arg2) => {
    return arg1 + arg2
  }
}
//一个形参时还可以缩写
var obj = {
  data: arg1 => {
    return JSON.stringify(arg1)
  }
}
//省略return的方式
var obj = {
  data: arg1 => JSON.stringify(arg1)
}
```

### 导入和导出模块

#### 导入

```javascript
//导入模块
import 模块名称 from '模块标志符' or '路径'
//导入外部的变量或者函数
import {firstName, lastName as newName, year} from './profile'
//导入模块，并立即执行，除了export
import './test'
```

#### 导出

```javascript
//在一个模块中只能出现一次，向外暴露的成员可以用任意变量来接受
export default {
    
}
//import {str, f} from './testModule'，按需导出的方式，必须严格按照导出的命名来接受，或者在导入的时候用as来取别名
export const str = 'hello world'

export function f(a){
    return a+1
}
```

### Promise

> 为了解决回调地狱的一种方案

- `Promise`是一个构造函数
- 在`Promise`中，有两个函数，`resolve`：成功之后的回调函数，`reject`：失败之后的回调函数
- 在`Promise`的原型属性上，有一个`then`方法 
- `Promise`表示一个异步操作（语义上），每当新建一个实例的时候，就表示一个具体的异步操作，无法return结果，只能通过回调函数的形式
- 每当创建一个`Promise`实例的时候，就会执行传递进去的`function`代码（通过`then`方法传递回调的`function`）
- 如果在`then`方法中不传入失败的`reject`，那么链式结构不会继续下去，也可以在最后用`catch`方法（有错误就全部跳出）



```javascript
//使用方式 ↓
function getP () {
  return new Promise(function (resolve, reject) {
    let sum = 0
    for (let i = 0; i < 10; i++) {
      sum += Math.random()
    }
    if (sum > 5) {
      resolve(sum)
    } else {
      reject('error')
    }
  })
}

getP()
  .then(function (data) {
    console.log(data)
    //返回一个新的Promise
    return getP()
  }, function (error) {
    console.log(error)
  })
  .then(function (data) {
    console.log('success')
    console.log(data)
  }, function (error) {
    console.log(error)
  })
```



#### 执行顺序

```javascript
setTimeout(()=>{
   console.log(1) 
},0)
let a=new Promise((resolve)=>{
    console.log(2)
    resolve()
}).then(()=>{
   console.log(3) 
}).then(()=>{
   console.log(4) 
})
console.log(5) 

//2 5 3 4 1 因为promise的executor是一个同步函数，一直在当前的微任务中，执行完毕后才去setTimeout这个宏任务
```

- `Promise`本身的定义是同步执行的，所以按照顺序，`in promise`会在`sync`前输出
- `Promise`回调的优先级是高于`setTimeout`等异步操作的，所以`promise resolve`会先于`seTtimeout`输出。浏览器会将`promise callbacks`加入到**同步事件队列的末尾**，而不是**异步事件的队列**

[Eventloop不可怕，可怕的是遇上Promise](https://juejin.im/post/5c9a43175188252d876e5903)

### async，await

- async 函数返回一个 Promise 对象，当async函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句
- async 告诉程序这是一个异步，awiat 会暂停执行async中的代码，等待await 表达式后面的结果，跳过async 函数，继续执行后面代码
- await  操作符用于等待一个Promise 对象，并且返回 Promise 对象的处理结果（成功把resolve 函数参数作为await 表达式的值），如果等待的不是 Promise 对象，则用 Promise.resolve(xx) 转化


```javascript
function say () {
  return new Promise(function (resolve) {
    setTimeout(function () {
      resolve('hello')
    }, 1000)
  })
}

async function demo () {
  const v = await say()
  console.log(v)
}
demo()
```

上述的例子会等到`setTimeout`执行完毕后，才会在打印出`hello`字符串

### 形参默认值

> 为形参加上等号

```javascript
function test(x = 1, y = 2) {
    console.log("x："+x ,"y："+ y);
}
test();
test(100);

```

```javascript

//这是一种可以不按照形参定义顺序传值的写法
function test({x = 1 ,y = 2} = {}) {
    console.log("x："+x ,"y："+ y);
}
test();
test({y:100,x:200});
test({x:100});

```

### 对象属性的声明

> 当属性的`key`和变量的名相同时，而要使用变量的值做`value`，就可以直接简写

```javascript
var name = 'abc'
var person = {
  //简写name : name
  name
}

var obj = {
  fun1: function (a, b) {
    return a + b
  },
  //简写如下
  fun2 (a, b) {
    return a + b
  }
}
```

### class定义对象

```javascript
class Person {
  constructor(name = "yuese", id = 1) {
    this.name = name;
    this.id = id;
  }
  say() {
    console.log("I am " + this.name);
  }
}
Person.prototype.talk = function() {
  console.log("My id is " + this.id);
};
var p = new Person("www");
p.say();
p.talk();
```



### 解构赋值

#### 方便取值

```javascript
let obj = {
    a: 123,
    b: "www",
    c: true
};
//对象取值
let {a, c} = obj;//let {a:num, c:flag} = obj;  为a赋值到新的变量num中
console.log(a + ' ' + c);

//数组取值
let [x,y,z]=['1',2,3]

//将数组除了第一项以外都赋值给b
let [a,...b]=[1,2,3,4,5]

//数组和对象嵌套
let obj1={s:{n:'1'},n:[1,2,'3','4'],m:[[1]]}
let {m:[x1],s:{n},n:[,...m2]}=obj1
console.log(x1,n,m2) //[1]  '1'  [2,'3','4']

//支持默认值
let {v:a=3}={v:'111'}//a的值是'111'，只有对象的属性值是undefined，默认值才会生效（例如传递function）

//字符串会被转换成数组
let [x, y, z] = '123456'
let { length } = '54321'
console.log(x, y, z, length)
```

#### 交换值

```javascript
[a,b]=[b,a]
```

### 扩展运算符

```javascript

//将两个数组进行拼接
[...[1,2,3],...[4,5,6]]

var obj = { name: 'aa' }
//将obj对象的属性拷贝
console.log({ ...obj, id: 1 })
```



### Map结构

任何值都可以成为键，

```javascript
var map=new Map([
  ['key1','v1'],
  [{key:123},'v2'],
  [123,'v3'],
])
```



### rest参数

> **...args**，Rest就是为解决传入的参数数量不一定， rest parameter(Rest 参数) 本身就是数组，数组的相关的方法都可以用

```javascript

```




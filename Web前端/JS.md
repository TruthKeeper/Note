# JavaScript

> 基于***对象***和***事件驱动***的的***多范式脚本***语言

##  对象

> 在JS中，对象就是键值对的集合

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

#### 混合方案

> 将别的属性强加在我的身上，就拥有这个成员了。
> **缺点**：如果拷贝的成员是引用对象的话，修改之后会在父类子类之前共享的

```javascript
function mix(src, dest) {
  for (var v in dest) {
      src[v] = dest[v];
  }
  return src;
}
console.log(mix({ name: "张三" }, { id: 2 }));
```

#### 原型链模式

> 利用原型来实现，实现复用，只要原型有，那我也就有
> **缺点**：对原型对象的引用对象修改后，会引发共享的问题


```javascript
function Parent(name) {
    this.name = name;
}

function Child(id) {
    this.id = id;
}

Child.prototype = new Parent("hhh");
Child.prototype.constructor = Child;

console.log(new Child(1));
```

#### 冒充

> 借用父类的构造函数
> **缺点**：无法获取到原型上的数据

```javascript
function Parent(name) {
    this.name = name;
}
Parent.prototype.say = function() {}

function Child(name, id) {
    Parent.call(this, name);
    this.id = id;
}
console.log(new Child("hhh", 1));
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

## DOM

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
- **innerText**
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

#### 亲属访问



#### 属性

- **getAttribute**：返回属性值
- **getAttributeNode**：返回属性节点，是一个对象



#### 其他

nodeValve：节点的值，一般在文本节点使用，获取值

nodeName：节点的名称，输出是大写

nodeType：1元素节点 2属性节点 3文本节点

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


- 对象被创建的时候会自动连接上**prototype**
- 构造函数的**原型属性** 就是 创建出来的对象的**原型对象**，o.**\_\_proto\_\_** === O.**prototype**
- 每个对象都有**.prototype**这个原型属性，查找属性的时候，如果找不到，会从原型中去查找（**原型链**）
- **prototype**属性中有一个属性**constructor**，用来和构造函数关联
- 由于**\_\_proto\_\_**是一个非标准对象，在早期浏览器中可能不兼容，通过**obj.constructor.prototype**来访问到
- 通过**直接替换**的方式修改了原型对象后，一般会添加一个**constructor**属性，因为在构造函数中可能还会调用构造函数，所以在内部应该使用**this.constructor()**
   ```javascript
function Person() {}
Person.prototype = {
    constructor = Person;
    //...
}

   ```

- 
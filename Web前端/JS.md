# JavaScript

> 基于***对象***和***事件驱动***的的***多范式脚本***语言

 ###  对象

> 在JS中，对象就是键值对的集合

#### 抽象性

> 如果需要用一个对象描述一个数据，需要抽取这个对象的核心数据

1. 抽取核心数据
2. 不在特定条件下不知道是什么

#### 封装性

> 将属性与方法组合、封装到一起

1. 如果键值是数据，就称为对象的属性
2. 如果键值是函数，就称为对象的方法，方法就是将过程封装起来

#### 继承

> 实现复用的手段，在JS中没有明确的继承语法，一般都是按照继承的理念来实现对象的成员扩充

```javascript
function mix(o1, o2) {
    for (var v in o2) {
        o1[v] = o2[v];
    }
    return o1;
}
console.log(mix({ name: "张三" }, { id: 2 }));
```

#### 构造函数

> 初始化数据，在JS中构造函数是用来初始化属性的，不是用来创建对象的，**new**才是

#### 创建对象的过程

1. `var obj=new Persoon();`
2. 用**new**创建了一个对象，类似**{}**，是一个**没有任何成员**的对象，对象的类型就是构造函数的名称，使用**{}**字面量创建的对象时object类型，相当于**new Object()**
3. 调用构造函数，将this指向创建出来的对象，利用其动态性并为其初始化成员

### DOM

父节点
	兄弟节点
	当前节点
		属性节点
		文本节点
		子节点
	兄弟节点

#### 增加

##### 创建

- **document.createElement**：创建元素节点
- **document.createTextNode**：创建文本节点
- **document.createAttribute**：创建属性节点
- **innerHTML**
- **innerText**
- **cloneNode()**

##### 加入

- **appendChild**：追加到最后结尾处
- **innerHTML**
- **insertBefore(新元素 , 旧元素)**

###### 增加属性的三种方式

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

#### 删除

- **removeChild**
- **removeAttribute**：接收属性名称来删除
- **removeAttributeNode**：接收属性对象来删除

#### 修改

##### 修改节点

- 删除节点再加入

##### 修改样式

- **style.xxx=yyy**
- **setAttribute**

##### 修改文本

- **innerText**
- **innerHtml**
- **nodeValue**：获取节点的名字，一般文本节点才使用

##### 修改属性

- **.xxx=yyy**
- **style.xxx=yyy**

#### 查询

##### 基础API

- **document.getElementById**
- **document.getElementByName**
- **document.getElementByTagName**
- **document.getElementByClassName**

##### 亲属访问



##### 属性

- **getAttribute**：返回属性值
- **getAttributeNode**：返回属性节点，是一个对象



#### 其他

nodeValve：节点的值，一般在文本节点使用，获取值

nodeName：节点的名称，输出是大写

nodeType：1元素节点 2属性节点 3文本节点

```

```
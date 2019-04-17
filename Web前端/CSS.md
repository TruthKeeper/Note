# CSS

## 书写规范

### 行内样式表

> &lt;标签名 style="属性1:属性值1; 属性2:属性值2"&gt;内容&lt;/标签名&gt;

### 内部样式表

> 在**head**标签中定义**style**

```html
<style type="text/css">
/*选择器{属性名 : 属性值;}*/
span {
    font-size: 20px;
    color: blue
}
</style>
```
### 外部样式表

> 统一在**head**中用**link**链接到外部CSS样式文件地址

## 选择器

### 标签选择器（元素选择器）

> 是把某一类标签全部选出来，批量设置样式

```css
span {
    font-size: 20px;
    color: blue
}
```


### 类选择器

> 选择class属性匹配的所有标签元素

```css
/*点声明，标签中定义class属性*/
.cls1 {
    color: blue;
}
```

#### 多类名选择器

- 样式显示效果和HTML中标签的class顺序没有关系，只和CSS书写的样式顺序有关
- 类名用空格分隔

```css
.cls1 {
    color: blue;
}
.cls2 {
    color: red;
}
.cls1.cls2 {
    color: green;
}

<span class="cls1">哈哈哈哈</span>
<span class="cls2">哈哈哈哈</span>
<span class="cls1 cls2">哈哈哈哈</span>
```




### id选择器

> 选择id属性匹配的所有标签元素。规范：相同id只允许出现一次

```css
/*井号声明，标签中定义id属性*/
#id1 {
    color: blue;
}
```

### 通配符选择器

选择所有标签元素，例如使用通配符选择器来清除所有HTML标签的默认边距

```css
* {
    margin: 0;
    padding: 0
}
```

### 后代选择器

> 又称包含选择器，用来选择元素或者元素组的后台，只要**发生嵌套**的标签就会被选择出来，中间用空格分隔

```css
/*找到所有作为class属性为.cls1标签后代的span标签*/
.cls1 span {
    color: #00f;
}
/*找出div标签内，包含的a标签下的span标签*/
div a span {
    color: #00f;
}

```

### 子代选择器

> 和后代选择器不同，后代是只要是后台，全部囊括，而子代只选择某元素的**子元素**

```css
/*找出所有作为div标签子元素的的span标签*/
div > span {
    color: #00f;
}
```

### 交集选择器

> 选择出所有满足多个条件的标签

```css
/*选择所有class属性为cls1的a标签*/
a.cls1 {
    color: blue;
}
```

### 并集选择器

适用于批量设置样式

```css
/*选择所有a,p标签*/
a, p {
    color: red;
}
/*选择所有id属性为container下的p，span标签*/
#container p, span {
    color: blue;
}
```

### 兄弟选择器

> 选择紧接在另一元素后的元素，且二者有相同父元素

```css
/*选择所有接连出现在h1标签后的p标签，h1和p标签拥有共同的父标签*/
h1 + p {
	color: red;
}
```

### 伪类选择器

> 用于向某些选择器添加**特殊**的效果

#### 链接

- :link  未访问过的链接状态
- :visited  访问过的状态
- :hover  鼠标经过链接的时候
- :activte  鼠标按下的状态

> hover属性必须在link和visited后，active必须在hover后，**lvha记法：LoveHate**


## 三大特性

### 层叠性

> 如果出现样式冲突，以最后出现的为准，**就近原则**

### 继承性

> 子标签会继承父级标签的**某些样式**，如文本颜色和字体大小

### 优先级 

- 权重相同，就近原则
- 权重会叠加
- 继承的样式即时修饰了!important，权重依然为0
- important：∞
- 行内样式---------------------------------------------1，0，0，0
- id选择器----------------------------------------------0，1，0，0
- 类、伪类选择器------------------------------------0，0，1，0
- 标签选择器------------------------------------------0，0，0，1
- 通配符选择器、继承样式------------------------0，0，0，0

```css
    /*0002*/
    div p {}

    /*0012*/
    .cls div a {}

    /*0011*/
    a:hover {}

    /*0101*/
    #nav p {}

  	/*0021*/
    span.cls1.cls2 {}
```
## 要点

### 外边距问题

#### 块级盒子居中对齐

- 盒子的左右margin设置为auto
- 为盒子设置宽度

#### 避免上元素margin-bottom和下元素margin-top

### 清除浮动带来的影响：闭合浮动

> 主要是为了解决父级元素因为子级浮动引起内部高度为0的问题

#### 额外标签法

> 通过在浮动元素末尾添加一个空的标签例如`<div style="clear: both" />`清除浮动后，父级会自动检测子的高度，以最高的为准

> 会增加冗余标签，一般不使用

#### 父级添加overflow属性

> `overflow:hidden|auto|scroll`，通过**BFC**的方式解决问题

#### 伪元素清除浮动

> 空元素的升级版，不用添加额外标签，添加额外的父级元素class

```css
.clearfix:after  {
    content: "";
    display: block;
    clear: both;
    visibility: hidden;
    height: 0;
}
.clearfix { 
	/*使ie6以下的可以识别*/
    *zoom: 1;
}
```

#### 双伪元素清除浮动

> 写法比上述的更加简洁

```css
.clearfix:before,.clearfix:after{
	content:".";
	display:table;
}
.clearfix:after{
	clear:both;
}
.clearfix{
	/*使ie6以下的可以识别*/
	*zoom:1;
}

```




## 属性

### 字体：font-family

- 各种字体直接需要用**逗号**分隔
- 如果字体中包含**空格**、**#**等符号，需要加**单引号**或者**双引号**
- 当设置

```css
p {
	font-family: Arial, "Microsoft YaHei", "宋体", sans-serif;
}
```

### 字体粗细：font-weight

- **lighter**：细体
- **normal**：正常，相当于number 400
- **bold**：粗体，相当于number 700
- **bolder**：特粗体
- **number**：数值

### 字体风格：font-style

- **normal**：正常
- **italic**：斜体

### 字体连写：font

按照如下顺序书写：

- font-style：可省略
- font-variant：一般用不到
- font-weight：可省略
- font-size/line-height：不能省略
- font-family：不能省略

```css
 p {
	font: bold 20px "宋体";
   }
```



### 字体颜色：color

- color: #ffffff
- color:green;
- color: rgb(155,100,0) or rgb(20%,100%,0%)

### 行高：line-height

> 行高设置为标签高度，可以设置为垂直居中

```css
p {
	line-height: 50px;
}
```

### 水平对齐方式：text-align

> 使标签内部的**文字**、**行内元素**、**行内块元素**水平对齐，没有垂直对其方式，
>
> - **left**
> - **right**
> - **center**

### 首行缩进：text-indent

```css
p {
    text-indent: 2em;/*段落首行缩进两个字*/
}
```

### 文字装饰：text-decoration

- **none**：无
- **underline**：下划线
- **overline**：上划线
- **line-througn**：删除线

### 背景：background

> `background`连写顺序如下

- 背景颜色：`background-color`，其中**CSS3支持rgba(0,0,0,0-1)**半透明
- 背景图片：`background-image: url(image.png)`
- 背景重复：`background-repeat:repeat|no-repeat|repeat-x|repeat-y`
- 背景固定：`background-attachment:fixed|scroll`
- 背景定位：`background-position: 10px 50px` ***Y坐标不写默认垂直居中***

### 边框：border

> `border`连写顺序如下

- `border-width`
- `border-style:none|solid|dashed|dotted` 无边框、实线、虚线、点线
- `border-color`

> 可以通过`border-left`指定仅某个方向有边框；
>
> 在表格中通过`border-collapse: collapse`来合并相连的边框

### 间距：padding

> `padding`连写顺序如下

- `padding-top`
- `padding-right`
- `padding-bottom`
- `padding-left`

### 边距：margin

 `margin`连写顺序如下

- `margin-top`
- `margin-right`
- `margin-bottom`
- `margin-left`

### 间距和边距重点

- 如果一个盒子指定宽度，设置左右的padding会撑开盒子
- 外盒子的宽度（元素空间尺寸)）= contentWidth + padding + border +margin 
- 内盒尺寸宽度（元素实际大小）= contentWidth + padding + border
- margin的单位除了正常的?px，?%还可以写成**auto**，即自动铺满，块级元素左右margin都为auto时会水平居中
- 相邻的块级元素的上下边距margin会**取较大值合并**
  ![](http://www.w3school.com.cn/i/ct_css_margin_collapsing_example_1.gif)
- 嵌套块级元素垂直外边距会发生合并，可通过设置border-top or padding-top 或者为父级元素设置`overflow: hidden`属性来避免
  ![](http://www.w3school.com.cn/i/ct_css_margin_collapsing_example_2.gif)

### CSS3：圆角：border-radius

> 多个参数时代表左上、右上、右下、左下，设置为50%即为原型

### CSS3：盒子阴影：box-shadow

- 阴影水平X轴
- 阴影水平Y轴
- 阴影的模糊距离
- 阴影的尺寸
- 阴影的颜色
- inset：内阴影

### 列表样式属性：list-style

## 浮动：float

> 脱离标准流，使多个div或块级元素可以在一行上显示，浮动只有左右浮动，`float: left or right`

- 浮动的元素排列位置和上一个块级元素有关系，如果上一个元素有浮动，那么与之顶部对齐
- 浮动属性可以让块级元素、行内元素隐式**转换成有行内块的特性**
- 不要使用浮动来做覆盖的效果，因为文本不会被覆盖，要用`postion:absolut`e来做
- 浮动的盒子`margin:0 auto`水平居中会失效

## 定位：position 

> 定位属性主要包括**定位模式**和**边偏移**

- 定位的盒子`margin:0 auto`水平居中会失效，可以通过`left:50%;margin-left:-子盒子宽一半px`来实现

### 边偏移

- top：偏移量，定义元素相对于父元素上边线的距离
- bottom：偏移量，定义元素相对于父元素下边线的距离
- left：偏移量，定义元素相对于父元素左边线的距离
- right：偏移量，定义元素相对于父元素右边线的距离

### 定位模式

#### 静态定位：static

> 默认定位模式，即标准流的特性，**唯一的用处就是用来取消定位**

#### 相对定位：relative

> 相对于原文档流的位置进行定位，即相对于自身左上角的位置进行偏移，但是任然**占有原来的位置**



#### 绝对定位：absolute

> 相对于已经定位（**relative**、**absolute**、**fixed**）的最近上级元素（没有的话以浏览器窗体body）进行定位，和浮动一样，**不占位置**。常用口诀：**子绝父相**，子元素是绝对定位的时候，父级元素基本上都有相对定位



#### 固定定位：fixed

> 固定定位，相对于浏览器窗口进行定位



## 显示模式：display

### 块级元素：block

> 默认独占一行，常见的有div，h1-h6，p，ul，ol

- 总是从新行开始
- 宽度默认是容器的100%
- 可以容纳**内联元素**和其他**块级元素**
- 文字类的块级元素p，h1-h6，里面不可以再放其他块级元素

### 行内元素：inline

> 不占有独立的空间，一般不可设置宽度和高度，常见的有span，a，strong

- 和相邻行内元素在一行上
- 默认宽度就算是本身内容的宽度
- 只能容纳文本或其他行内元素，**a标签比较特殊，可以放块级元素**

### 行内块元素：inline-block

> 拥有块级和行内的特点，较为特殊，img，input，td

- 与相邻的行内元素或行内块元素在一行上
- 默认宽度就算是本身内容的宽度
- 高度和宽度可以设置


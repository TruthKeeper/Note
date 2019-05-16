# Vue

## 标签属性

### v-cloak

> 能够解决表达式在未解析之前显示出来的问题

```html
<style type="text/css">
    [v-cloak] {
        display: none;
    }
</style>

<p v-cloak>msg = {{msg}}</p>
```

### v-text

> 也能够解决表达式在未解析之前显示出来的问题，和`v-cloak`的区别在于`v-text`会覆盖标签内的文本

```html
<p v-text="msg"></p>
```

### v-html

> 和上述不同的是支持把插入的内容解析成**html**元素

### v-bind:

> 对于**html**元素的属性需要使用该指令，也可以将`v-bind`省略掉，只保留**冒号**，在`v-bind`中可以写合法的JS表达式

```html
<input type="button" v-bind:value="mTitle">
<input type="button" v-bind:value="mTitle + 'hahah'">
```

### v-on

> Vue提供的事件绑定机制，也可以将`v-on`省略掉，用**@**符号
>
> *注意：在VM实例中如果想要访问**data**上的数据或者调用**methods**的方法，必须通过**this.**属性名来进行访问*

```html
<button v-on:click='myClick'>{{title}}</button>

<script type="text/javascript">
    var vm = new Vue({
        el: 'button',
        data: {
            title:'我是一个按钮',
        },
        methods:{
            myClick:function () {
                alert('aaa');
            }
            //也可以这样 myClick(){}
        },
    });

</script>
```

### 事件修饰符

#### .stop

> 阻止事件向上冒泡

 ```html
<div class="c1" @click="f1">
    <div class="c2" @click.stop="f2">
        <div class="c3" @click="f3"></div>
    </div>
</div>
 ```

#### .prevent

> 阻止默认事件

```html
<a href="https://www.baidu.com" @click.prevent="go">百度</a>
```

#### .capture

> 切换事件监听到捕获阶段

```html
<div class="c1" @click.capture="f1">
    <div class="c2" @click="f2">
        <div class="c3" @click="f3"></div>
    </div>
</div>
```

#### .self

> 只有当自身被点击的时候（不包含子元素）才触发

#### .once

> 只触发一次

### v-model

> `v-bind`只能实现数据的单向绑定，M绑定到V，无法实现双向绑定，使用`v-model`属性，可以实现表单元素的数据双向绑定，只能用在**表单元素**（**input**、**select**、**checkbox**、**textarea**）

```html
<input type="text" v-model="content">
```






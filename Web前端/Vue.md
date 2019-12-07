# Vue

## Webpack中使用

1. 安装`vue`的包
2. 安装解析`vue`组件模板的`loader`
3. 在入口`main.js`中导入`vue`模块`import Vue from 'vue'`
4. 定义一个以`.vue`结尾的组件，`.vue`组件由三部分组成，`template`，`script`，`style`
5. 在入口`main.js`中导入这个`vue`组件
6. 创建`vm`实例
7. 在页面中创建标签以供`vm`绑定

### Ps

- `.vue`文件中的`style`可以设置`lang="less"`启用`less`预编译，`scoped`属性表示仅在该容器中应用样式，核心原理是通过属性选择器实现的
- 



## 标签属性

### v-once

> 该数据节点的数据不会再次刷新，只执行一次性插值

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```

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

### v-bind  缩写:

> 对于**html**元素的属性需要使用该指令，也可以将`v-bind`省略掉，只保留**冒号**，在`v-bind`中可以写合法的JS表达式

```html
<input type="button" v-bind:value="mTitle">
<input type="button" v-bind:value="mTitle + 'hahah'">
```

#### 绑定类

> 类样式需要用字符串数组声明，可以用三元表达式或者对象的方式

```html
<div :class="['header',flag?'left':'right']">哈哈哈</div>

<div :class="['header',{'pink':pinkFlag},{'big':bigFlag}]">哈哈哈</div>

<!--属性名也可以不带引号-->
<div :class="{header:true,pink:pinkFlag,big:bigFlag}">哈哈哈</div>
```

#### 绑定内联样式

```html
<!--对象的形式，其中属性名称带-的必须用引号引起来-->
<div :style="{color:'red','font-weight':700}">哈哈哈</div>
<!--也可以直接饮用data中的对象-->
<div :style="bigStyle">哈哈哈</div>
<!--数组的形式，不用引号-->
<div :style="[bigStyle,flexStyle]">哈哈哈</div>

data: {
    bigStyle: {'color': 'blue', 'font-size': '20px'},
    flexStyle: {display: 'flex'}
},
```

### v-pre

> 跳过该标签元素的编译过程，用来显示原本的`Mustache `标签

```html
<span v-pre>{{ this will not be compiled }}</span>
```

### v-for

> *注意：在多次更新元素列表的场景下，设置`v-bind:key=''`属性来提高刷新效率，只能用Number和String*

```html
<!--item 和 i相当于形参，代表循环的 项 和 索引-->
<li v-for="(item,i) in list ">{{item }} {{i}}</li>

<!--循环对象数组-->
<li v-for="(user,i) in userList">{{user.id }} {{user.name}}</li>

<!--循环比那里对象的属性-->
<li v-for="(value,name,index) in user">{{name}}:{{value}},索引:{{index}}</li>

<!--循环数字,从1开始-->
<li v-for="count in 2">{{userList[count-1].name}}</li>

```

#### key的其他场景

```html
<!--如下代码在loginType切换时，input标签中的内容不会发生变化，因为未对其设置key属性，vue会将其当做是可复用的元素-->
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```



### v-on 缩写@

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
<!--等价于-->
<input type="text" :value="con" @input="con = $event.target.value">
```

### v-if v-else v-else-if

> 每次通过DOM操作来删除或者创建元素，**较高的切换性能消耗**，`v-else`必须跟在`v-if`或者`v-else-if`的后面，不然失效，`v-else-if`必须要跟在`v-if`后面

```html
<h3 v-if="flag">哈哈哈</h3>
```

### v-show

> 只是切换了display属性，**较高的初始渲染消耗**

```html
<h3 v-show="flag">哈哈哈</h3>
```

### 按键修饰符

也可以监听

```html
<!--监听按下回车键-->
<input type="text" @keyup.enter="enter">
```

- `.enter`：
- `.tab`
- `.delete`：删除和退格键
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

## 全局过滤器

> 用于花括号和v-bind中

```html
<h3>{{msg | msgF}}</h3>

Vue.filter('msgF', function (data) {
       return data + 'aaa';
   });

//也可以传参,过滤器后面还可以|其他过滤器
 <h3>{{msg | msgF(1,2,3)}}</h3>


Vue.filter('msgF', function (data, arg1, arg2, arg3) {
    console.log(arg1 + ' ' + arg2);
    return data + 'aaa';
});
```

## 私有过滤器

> 过滤器调用过程中，就近原则，如果私有和全局过滤器同名，调用私有过滤器

```javascript
var vm = new Vue({
    el: '.d',
    data: {
        d: new Date()
    },
    filters: {
        dateF(date) {
            return 'hock!';
        }
    }
})
```

## 自定义指令

> 定义指定时前面不需要加`v-`，但是调用的时候要加`v-`

### 全局指令

```javascript
Vue.directive('name', {
    //所有函数都会回调元素DOM对象el
    bind: function (el,binding) {
        //每当指令绑定到元素上的时候回调，只调用一次
        //和元素样式有关的操作，一般都可以在bind中执行
    },
    inserted: function (el) {
        //每当指令绑定的元素被插入到DOM的时候回调
        //和JS有关的操作，放到inserted方法内部处理

    },
    updated: function (el) {
        //当VNode更新的时候回调，可能会回调多次

    },
});

```

### 私有指令

```javascript
var vm = new Vue({
        el: '.d',
        data: {},
        directives:{
            'color':{
                bind: function (el,binding) { },
                inserted: function (el) { },
                updated: function (el) { },
            }
        }
    })
```



### binding对象

> 一个对象，包含以下属性：

- `name`：指令名，不包括 `v-` 前缀。
- `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
- `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
- `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"`中，表达式为 `"1 + 1"`。
- `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
- `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。

### 函数简写

> 在很多时候，可能只想在 `bind` 和 `update` 时触发相同行为，而不关心其它的钩子

```javascript
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

## 生命周期

![](E:\Note\Web前端\Vue生命周期.png)

```javascript
![Vue动画_过渡](E:\Note\Web前端\Vue动画_过渡.png)var vm = new Vue({
    el: '.d',
    data: {},
    //创建期间的生命周期函数↓
    beforeCreate() {
        //vm示例被完全创建出来之前的生命周期函数
    },
    created() {
        //data和methods初始化完毕
    },
    beforeMount() {
        //Vue完成了对模板的编译，生成DOM元素在内存中，尚未挂载到页面中
        //页面中的元素仍然是模板，
    },
    mounted() {
        //内存中的模板完成了挂载操作，页面中的元素已经完成了渲染更新
        //是实例创建期间的最后一个生命周期函数
    },

    //运行期间的生命周期函数↓
    beforeUpdated() {
        //当数据源发生了改变，页面的元素尚未更新
    },
    updated() {
        //DOM树完成了更新操作
    },

    //销毁期间的生命周期函数↓
    beforeDestory() {
        //实例上的data、methods等过滤器都可用，还未进入正式的销毁过程
    },
    destoryed() {
        //实例已经被完全销毁了
    }
})
```

## 动画

### 过渡类名

1. 使用`transition`标签（Vue提供的）将需要被动画控制的元素包裹起来
2. 定义样式![](E:\Note\Web前端\Vue动画_过渡.png)

```html
/*时间点：进入之前的起始状态*/
.v-enter,
/*时间点：离开之后的状态*/
.v-leave-to {
    opacity: 0;
}
/*入场过程中的时间段*/
.v-enter-active,
/*离场过程中的时间段*/
.v-leave-active {
    transition: all .4s;
}

<transition>
        <h3 v-if="flag">哈哈哈</h3>
</transition>
```

> *如果`transition`标签未设置`name`属性，则样式默认是`v-`前缀，如果设置了`name`属性，那么代替`v`*

### 第三方CSS

> 也可以通过对`transition`标签设置类名来接入第三方动画css*

```html
<transition enter-active-class="animated bounceInLeft"
                leave-active-class="animated zoomOutLeft">
        <h3 v-if="flag">哈哈哈</h3>
</transition>
```

### 钩子函数

> 可以对`transition`标签设置`v-on`属性

```html
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>

// ...
methods: {
  // 动画入场前的起始样式
  beforeEnter: function (el) {

  },
  // 表示动画开始之后的样式，即结束状态
  enter: function (el, done) {
	//这句代码是触发重绘	
	el.offsestWidth
    // done是afterEnter函数的引用
    done()
  },
  afterEnter: function (el) {

  },
  enterCancelled: function (el) {

  },

  // --------

  beforeLeave: function (el) {

  },
  leave: function (el, done) {

    done()
  },
  afterLeave: function (el) {

  },
  // leaveCancelled 只用于 v-show 中
  leaveCancelled: function (el) {
    // ...
  }
}
```

- 当只用 JavaScript 过渡的时候，**在 enter 和 leave 中必须使用 done 进行回调**。否则，它们将被同步调用，过渡会立即完成。

- 推荐对于仅使用 JavaScript 过渡的元素添加 `v-bind:css="false"`，Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响。

### 列表动画

> 在实现列表过渡的时候，如果需要过渡的元素是通过`v-for`创建出来的，需要用`transition-group`包裹，并且每个元素要设置`key`属性

- 为`transition-group`标签设置`appear`属性，实现入场效果
- 为`transition-group`标签设置`tag='ul'`属性，表示渲染时外部父容器是`ul`元素，如果不指定，会被渲染成`span`

## 数据监听

> 除了对标签元素监听按键抬起的方式以外，还可以监听数据的变化，并且能监听路由地址的改变

```javascript
new Vue({
        el: '#app',
        data: {
            str: ''
        },
        watch: {
            //监听str数据的改变
            str: function (newVal,oldVal) {
              
            },
            //监听路由地址的改变
            '$route.path': function (newVal, oldVal) {
                console.log(oldVal + '----' + newVal);
            }
        }
})
```

## 数据重置

`Object.assign(this.$data,this.$options.data())`

## 计算属性

> 在模板中不应该有太复杂的计算逻辑，可以放到计算属性中`<p>message: "{{ reversedMessage}}"</p>`（不需要加引号），和`methods`的区别在于，**计算属性具有缓存性，当依赖于计算属性的属性发生改变时，会响应式的刷新**

```javascript
var vm = new Vue({
  el: '#app',
  data: {
    message: 'wwwww'
  },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
```

```javascript
//下面的计算属性不会发生更新，因为缓存了
computed: {
  now: function () {
    return Date.now()
  }
}
```
```javascript
//传递参数的场景
computed: {
 isFuli: function () {
        return function (data) {
          return data.type === '福利';
        }
      }
}
```

## 组件化

- 模块化是从代码逻辑开进行划分，保证功能模块的职责单一
- 组件化是从UI界面的角度进行划分，方便代码重用
- 其中组件名称如果是大小驼峰，在使用过程中需要用小写和`-`拼接，不使用驼峰的话则直接使用即可
- 组件中的`data`中存放的是函数，返回对象实体

### 方式一

> 使用`extend`方法创建组件

```html
//第一个参数为组件的名称，也就是将来引用组件时的标签
//第二个参数为创建的组件
Vue.component('dataList', Vue.extend({
	//template为将来要展示的HTML内容
    template: '<h3>哈哈哈哈哈</h3>'
}));

<data-list></data-list>

```

### 方式二

简化版，不管是哪种方式创建出来的组件，`template`指向的模板内容，都只能有一个根元素

```html
Vue.component('dataList', {
	//template为将来要展示的HTML内容
    template: '<h3>哈哈哈哈哈</h3>'
});

<data-list></data-list>
```

```

```

### 方式三

> 通过`template`元素，定义组件的HTML模板

```html
<template id="tmpl1">
    <div></div>
</template>


Vue.component('dataList', {
    template: '#tmpl1'
});
```

### 私有组件

```javascript
var vm = new Vue({
      el: '.d',
      data: {},
      components: {
          login: {
              template: '<h1>哈哈哈</h1>'
              //template: '#tmpl1'
          }
        }
  })
```

### 引入组件

```html
//1 标签的形式直接使用
<login></login>

//2 component标签来动态切换
//login是组件名称
<component :is="'login'"></component>
//cName是data中的一个属性
<component :is="cName"></component>
```

#### 组件切换的动画

> 将`component`用`transition`包裹起来

```html
<!--mode属性out-in表示隐藏的组件完全隐藏后再显示要显示的组件-->
<transition name="component-fade" mode="out-in">
  <component :is="view"></component>
</transition>
```

### 组件间数据传递

#### 父组件传递给子组件数据

> ***核心：通过为子组件设置props数组，配置在子组件标签中使用的属性名称；子组件不要去修改来自父组件的数据，改了也没用，单向下行绑定***

```html
<!--v-bind绑定属性，绑定父组件data中的msg属性-->
<login :parentmsg="msg"></login>

var vm = new Vue({
    components: {
        login: {
            //子组件默认无法获取到父组件的data，methods等的数据
            template: '<h2>{{parentmsg}}</h2>',
            //在这个数组中定义属性名称
            props: [
                'parentmsg'
            ]
			//如下的写法可以规范传值类型
			props: {
                'pro1':{
  				 type:Number,
  				 default:100//require: true 必传
				}
            }
        }
    }
})
```

> 默认情况下`props`属性是**只读**（单向数据传递）的，`data`中的数据是**可读可写**的，可以在子组件声明`props`时添加**.sync**修饰，`:pMsg.sync="msg"`，然后子组件通过`this.$emit("update:pMsg", newV)`来更新父组件的`pMsg`属性

#### 父组件传递给子组件函数 && 子组件传递给父组件数据 or 函数

> ***核心：不能通过`props`来实现了，要通过`v-on`（省略为@）的方式传递，在子组件中通过`this.$emit`来调用***

```html
<login @parent-fun="callback"></login>

 new Vue({
        el: '.d',
        data: { },
        methods: {
            callback(arg) {
                alert('我是父组件的方法，子组件传递了：' + arg)
            }
        },
        components: {
            login: {
                //子组件默认无法获取到父组件的data，methods等的数据
                template: '<div><button @click="myClick">点我点我</button></div>',
                methods: {
                    myClick() {
                        this.$emit('parent-fun', 2333)
                    }
                }

            }
        }
    });
```

#### 父组件调用子组件函数

> 通过对子组件设置`ref`属性，然后通过`this.$refs.***`直接调用

#### 兄弟组件之间传值

> 不用vuex的方案：可以通过实例化一个vue对象作为Bus，要相互通信的兄弟组件都引入Bus，分别调用Bus的事件触发和监听来实现通信

```javascript
//Bus.js
import Vue from 'vue'
export default new Vue()

//发送端
import Bus from '../bus.js'
export default{
	methods: {
	    toBus () {
	        Bus.$emit('on', '来自兄弟组件')
	    }
	  }
}

//接收端
import Bus from '../bus.js'
export default {
    mounted() {
       Bus.$on('on', (msg) => {//不用箭头函数的话无法定位到this
         console.log(msg)
       })
     }
   }

 
```

### 获取子组件或者DOM元素

> ***核心：`ref`属性标记引用地址，在父组件中通过`this.$refs`来获取***

```html
<div class="d">
    <button @click="click">点我</button>
    <p id="art" ref="ppp">{{msg}}</p>
    <login ref="com_login"></login>
</div>


    var vm = new Vue({
        el: '.d',
        data: {
            msg: '我是父组件的内容',
        },
        methods: {
            click() {
				//获取到Dom元素
                console.log(this.$refs.ppp.innerText);
				//获取到组件
                console.log(this.$refs.com_login.msg);
            }
        },
        components: {
            'login': {
                data() {
                    return {
                        msg: '我是子组件的内容'
                    }
                },
                template: '<h3>{{msg}}</h3>'
            }
        }
    });
```



### 监听组件的生命周期

其余`created`事件也都可以监听到

```html
<Child @hook:mounted="doSomething"/>
```



## => 路由vue-router

1. 导入路由的包
2. 通过`VuewRouter`构造函数创建路由对象，传入一个配置对象
3. 在配置对象中对`routes`数组属性（路由匹配规则）进行配置
4. 每一个路由规则都有两个必须的属性，`path`，`component`
5. `path`代表监听的路由链接地址，`component`代表组件对象



```html
<div id="app">
    <a href="#/login">登录</a>
    <a href="#/register">注册</a>
    <!--路由匹配到的将在这里显示,相当于一个占位符-->
    <router-view></router-view>
</div>
<template id="temp_login"><h1>登录组件</h1></template>
<template id="temp_register"><h1>注册组件</h1></template>
<script type="text/javascript">
    var routerObj = new VueRouter({
        routes: [
            {
                path: '/login',
                component: {
                    template: '#temp_login'
                }
            },
            {
                path: '/register',
                component: {
                    template: '#temp_register'
                }
            },
        ]
    });
</script>
```

### 跳转

#### router-link

> 可以通过`<router-link to="/login">登录</router-link>`来导航，
>
> 或者`<router-link :to="{name:'login',params:{id:'123'}}">登录</router-link>`

- 默认`router-link`会被渲染成`a`标签，可以通过`tag`属性改成其他元素
- 当前激活的路由类名会默认变成`router-link-active`（可以在`VueRouter`中对`linkActiveClass`属性赋值其他的命名），可以调整样式

#### 编程式

> 注意：`$route`代表路由参数对象，可以获取到`query`，`param`，而`$router`是路由导航对象，用来实现路由的前进、后退和跳转操作

##### push

> 跳转导航，会向`history`生成一条记录

```javascript
//最简单的方式
this.$router.push('/account' + 123)
//对象的方式
this.$router.push({path: '/account?id' + 123})
//查询的方式
this.$router.push({path: '/account', query: {id: '321'}})
//带命名的路由,其中 path 不能和 params 一起用
this.$router.push({name: 'Account', params: {id: '123'}})
```

##### replace

和push很相似，唯一区别在于不会向`history`添加新纪录

##### go

```js
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)
// 后退一步记录，等同于 history.back()
router.go(-1)
```



### 重定向

不是指网络资源的重定向，是默认重定向来显示默认路由组件

```javascript
 routes: [
            {
                path: '/', redirect: '/login'
            },
            {
                path: '/login', component: loginC
            }
         ]
```

### 切换动画

> 用`transition`标签包裹

### 传递数据

#### query

> `<router-link to="/login?name=www">登录</router-link>`，对应的：`{{$route.query.name}}`，通过`$route`对象可以获取到

#### params

> **resful**风格，会通过正则自动匹配

```html
<router-link to="/login/hhh/2333">登录</router-link>
     
<h1>登录组件{{this.$route.params.name}}</h1>


routes: [
            {
                path: '/login/:name/:id',
                component: loginC
            }
]
```

### 路由嵌套

通过

```html
<div id="app">
    <router-link to="/account/login">进入Account组件</router-link>
    <router-view></router-view>
</div>

<template id="temp1">
    <div>
        <router-link to="/account/login">进入Login组件</router-link>
        <router-link to="/account/register">进入Register组件</router-link>

        <router-view></router-view>
    </div>
</template>

<script type="text/javascript">
    var routerObj = new VueRouter({
        routes: [
            {path: '/', redirect: '/account/login'},
            {
                path: '/account', redirect: '/account/login', component: {template: '#temp1'},
                children: [
                    //通过children属性配置子级路由，子路由下的path前缀不需要带/，，否则会以根路径开始匹配
                    {path: 'login', component: {template: '<h2>登录组件</h2>'}},
                    {path: 'register', component: {template: '<h2>注册组件</h2>'}}
                ]
            }
        ]
    });
</script>
```

### 命名识图来布局

> ***核心：对`routes`数组中的对象，将`component`改用`components`，标记名称，在`router-view`中用`name`属性来指向模板对象***

```html
<style type="text/css">
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html, body, #app {
            height: 100%;
        }

        #app {
            display: flex;
            flex-direction: column;
        }

        .header {
            height: 100px;
            background-color: pink;
        }

        .c {
            display: flex;
            flex: 1;
        }

        .left-nav {
            width: 200px;
            background-color: blue;
        }

        .main {
            flex: 1;
        }
</style>

<div id="app">
    <!--其中带冒号修饰时，要区分是属性还是字符串-->
    <!--不带冒号修饰时，都当成字符串-->
    <router-view :name="'header'"></router-view>
    <div class="c">
        <router-view name="leftNav"></router-view>
        <router-view name="main"></router-view>
    </div>
</div>

<script type="text/javascript">
    var routerObj = new VueRouter({
        routes: [
            {
                path: '/', components: {
                    //当前路径下显示多个路由组件
                     'header': {template: '<h1 class="header">我是Header</h1>'},
                    'leftNav': {template: '<h1 class="left-nav">我是左侧导航栏</h1>'},
                    main: {template: '<h1 class="main">我是主体</h1>'},
                }
            }
        ]
    });

    var vm = new Vue({
        el: '#app',
        router: routerObj
    });
```



## => Vuex

> 一个公共数据管理工具，可以将一些**共享**的数据保存到vuex中，方便整个程序中任何组件直接访问到这些公共数据

> 为了解决**多层组件嵌套**、**兄弟组件**场景下数据传递问题

1. 导入Vuex
2. 在入口js中创建一个Vuex.Store实例
3. 绑定到Vue实例下

```javascript
const store = new Vuex.Store({
  //可以当成组件下的data
  state: {
    //组件中通过$store.state.count来访问
    //不建议组件直接修改值，如果造成问题会难以跟踪 
    count: 0
  },
  //可以当成组件下的methods
  mutations: {
      //如果组件想要调用方法，通过this.$store.commit('方法名' , arg),方法最多两个参数
      increase(state){
          state.count++;
      }	
  },
  //类似组件中的computed计算属性（会缓存），只要state数据发生变化，getters也随之重新求值
  //也类似过滤器，对数据做了一层包装
  getters: {
      optCount:function(state){
          return '当前的值是：'+state.count;
      }
  }
})

var vm=new Vue({
  el: '#app',
  render: c => c(App),
  store,
})
```




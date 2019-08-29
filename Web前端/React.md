 # React

## 安装

- npm install -g create-react-app   安装create-react-app
- create-react-app my-app   使用create-react-app命令初始化项目

## 样式定义

### ~~行内样式~~

> 难以维护和复用，不建议大面积覆盖

```react
class App extends React.Component {
  // ...
  render() {
    return (
      <div style={{ background: '#eee', width: '200px', height: '200px'}}>
        <p style= {{color:'red', fontSize:'40px'}}>行内样式</p>
      </div>
    )
  }
}
```

### ~~声明式~~

行内样式的改进写法，不美观

```react
const fontSize = '20px'
const pStyle = {
  fontSize
}
const spanStyle = {
  fontSize,
  backgroundColor: '#ff6262'
}

class App extends Component {
  render() {
    return (
      <div>
        <p style={pStyle}>哈哈哈</p>
        <span style={spanStyle}>哈哈哈</span>
      </div>
    )
  }
}

```

### ~~引入样式~~

由于css的规则是全局的，会导致冲突以及冗余

```react
import './Person.css';
```

### styled-components

css in js的方案

```react
import React, { Component } from 'react'
import styled from 'styled-components'

const Main = styled.div`
  background-color: #ccc;
`
const Header = styled.div`
  font-size: 20px;
  text-align: center;
  color: red;
`
const Content = styled.div`
  font-size: 14px;
  ul,
  li {
    list-style: none;
    a {
      text-decoration: none;
    }
  }
`
class Home extends Component {
  render() {
    return (
      <Main>
        <Header>头部</Header>
        <Content>
          <ul>
            <li>
              <a href="#">哈哈哈</a>
            </li>
            <li>
              <a href="#">哈哈哈</a>
            </li>
          </ul>
        </Content>
      </Main>
    )
  }
}
```

#### 更多用法

[点击查看](https://www.jianshu.com/p/27788be90605)

## 事件处理

### click

[绑定事件的几种方式对比](https://blog.csdn.net/weixin_34191734/article/details/91379518)

```react
 {/* 传参 -- 该方法在 render 时会直接被执行 */}
<button onClick={this.handleClick(data)}>按钮</button>
{/* 如下就不会 */}
<button onClick={() => handleClick(data)}>按钮</button>
<button onClick={this.handleClick.bind(this, data)}>按钮</button>
```

```react
// 总结如下
class Mine extends Component {
  render() {
    return (
      <div className="mine">
        <button onClick={this.clickWithoutData}>不传递数据</button>
        <button onClick={e => this.clickWithData('123', e)}>传递数据</button>
      </div>
    )
  }
  clickWithoutData = e => {
    console.log('clickWithoutData', this)
  }
  clickWithData = (data, e) => {
    console.log('clickWithData', this, data, e)
  }
}
```

## 渲染

### 是否渲染

```react
class Mine extends Component {
  tip = false
  render() {
    return <div className="mine">{this.tip && <Header />}</div>
  }
}
function Header() {
  return <h1>我是头部</h1>
}
```

### 是否装载

> 和vue的区别在于不在dom树中，会走生命周期

```react
class Mine extends Component {
  tip = false
  render() {
    return (
      <div className="mine">
        <Header show={this.tip} />
      </div>
    )
  }
}
class Header extends Component {
  render() {
    return this.props.show ? <h1>我是头部</h1> : null
  }
  componentWillMount() {
    console.log('componentWillMount')
  }
}
```







## 组件

### 创建

#### 函数式创建

> 无状态组件，不能访问`this`，没有生命周期，只能访问`props`，通常用于合成更复杂的组件

```react
import React from 'react'

function Mine() {
  return <div>我的</div>
}

export default Mine

```

#### class式创建

```react
import React, { Component } from 'react'

class Mine extends Component {
  render() {
    return <div>我的</div>
  }
}

export default Mine

```

### 传递数据

> `props`是不可变的，而`state`的可以改变

```react
class App extends Component {
  name = '张三'
  render() {
    return (
      <div className="App">
        <Mine name={this.name} />
      </div>
    )
  }
}
//函数式接收
function Mine(props) {
  return <div>我叫{props.name}</div>
}
//class式接收
class Mine extends Component {
  render() {
    return <div>我叫{this.props.name}</div>
  }
}
```

#### 单向流动

父组件通过传递`state`对象中的属性，手动`setState`时刷新

```react
class App extends Component {
  num = 1
  constructor(props) {
    super(props)
    this.state = { num: this.num }
  }

  componentDidMount() {
    this.timerID = setInterval(() => {
      this.num++
      //setState会触发更新
      this.setState({
        num: this.num
      })
    }, 1000)
  }
  componentWillUnmount() {
    clearInterval(this.timerID)
  }
  render() {
    return (
      <div className="App">
        <Mine value={this.state.num} />
      </div>
    )
  }
}
```

#### 默认值

```react
class Mine extends Component {
  render() {
    return <div className="mine">我叫{this.props.value}</div>
  }
}
Mine.defaultProps = {
  value: 'AA'
}
```

#### 数据刷新

```react
class Header extends Component {
  constructor(props) {
    super(props)
    this.state = {
      num: 0
    }
  }
  render() {
    return (
      <div>
        <button onClick={this.ccc}>走你{this.state.num}</button>
      </div>
    )
  }
  ccc = () => {
    this.setState(
      function(prevState, props) {
        return { num: ++prevState.num }
      },
      () => {
        console.log('update')
      }
    )
  }
}
```

## 生命周期

- **componentWillMount** 在渲染前调用,在客户端也在服务端。
- **componentDidMount** : 在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异步操作阻塞UI)。
- **componentWillReceiveProps** 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。
- **shouldComponentUpdate** 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。 
  可以在你确认不需要更新组件时使用。
- **componentWillUpdate**在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。
- **componentDidUpdate** 在组件完成更新后立即调用。在初始化时不会被调用。
- **componentWillUnmount**在组件从 DOM 中移除之前立刻被调用。
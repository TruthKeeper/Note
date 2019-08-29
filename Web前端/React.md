 # React

## ��װ

- npm install -g create-react-app   ��װcreate-react-app
- create-react-app my-app   ʹ��create-react-app�����ʼ����Ŀ

## ��ʽ����

### ~~������ʽ~~

> ����ά���͸��ã���������������

```react
class App extends React.Component {
  // ...
  render() {
    return (
      <div style={{ background: '#eee', width: '200px', height: '200px'}}>
        <p style= {{color:'red', fontSize:'40px'}}>������ʽ</p>
      </div>
    )
  }
}
```

### ~~����ʽ~~

������ʽ�ĸĽ�д����������

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
        <p style={pStyle}>������</p>
        <span style={spanStyle}>������</span>
      </div>
    )
  }
}

```

### ~~������ʽ~~

����css�Ĺ�����ȫ�ֵģ��ᵼ�³�ͻ�Լ�����

```react
import './Person.css';
```

### styled-components

css in js�ķ���

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
        <Header>ͷ��</Header>
        <Content>
          <ul>
            <li>
              <a href="#">������</a>
            </li>
            <li>
              <a href="#">������</a>
            </li>
          </ul>
        </Content>
      </Main>
    )
  }
}
```

#### �����÷�

[����鿴](https://www.jianshu.com/p/27788be90605)

## �¼�����

### click

[���¼��ļ��ַ�ʽ�Ա�](https://blog.csdn.net/weixin_34191734/article/details/91379518)

```react
 {/* ���� -- �÷����� render ʱ��ֱ�ӱ�ִ�� */}
<button onClick={this.handleClick(data)}>��ť</button>
{/* ���¾Ͳ��� */}
<button onClick={() => handleClick(data)}>��ť</button>
<button onClick={this.handleClick.bind(this, data)}>��ť</button>
```

```react
// �ܽ�����
class Mine extends Component {
  render() {
    return (
      <div className="mine">
        <button onClick={this.clickWithoutData}>����������</button>
        <button onClick={e => this.clickWithData('123', e)}>��������</button>
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

## ��Ⱦ

### �Ƿ���Ⱦ

```react
class Mine extends Component {
  tip = false
  render() {
    return <div className="mine">{this.tip && <Header />}</div>
  }
}
function Header() {
  return <h1>����ͷ��</h1>
}
```

### �Ƿ�װ��

> ��vue���������ڲ���dom���У�������������

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
    return this.props.show ? <h1>����ͷ��</h1> : null
  }
  componentWillMount() {
    console.log('componentWillMount')
  }
}
```







## ���

### ����

#### ����ʽ����

> ��״̬��������ܷ���`this`��û���������ڣ�ֻ�ܷ���`props`��ͨ�����ںϳɸ����ӵ����

```react
import React from 'react'

function Mine() {
  return <div>�ҵ�</div>
}

export default Mine

```

#### classʽ����

```react
import React, { Component } from 'react'

class Mine extends Component {
  render() {
    return <div>�ҵ�</div>
  }
}

export default Mine

```

### ��������

> `props`�ǲ��ɱ�ģ���`state`�Ŀ��Ըı�

```react
class App extends Component {
  name = '����'
  render() {
    return (
      <div className="App">
        <Mine name={this.name} />
      </div>
    )
  }
}
//����ʽ����
function Mine(props) {
  return <div>�ҽ�{props.name}</div>
}
//classʽ����
class Mine extends Component {
  render() {
    return <div>�ҽ�{this.props.name}</div>
  }
}
```

#### ��������

�����ͨ������`state`�����е����ԣ��ֶ�`setState`ʱˢ��

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
      //setState�ᴥ������
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

#### Ĭ��ֵ

```react
class Mine extends Component {
  render() {
    return <div className="mine">�ҽ�{this.props.value}</div>
  }
}
Mine.defaultProps = {
  value: 'AA'
}
```

#### ����ˢ��

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
        <button onClick={this.ccc}>����{this.state.num}</button>
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

## ��������

- **componentWillMount** ����Ⱦǰ����,�ڿͻ���Ҳ�ڷ���ˡ�
- **componentDidMount** : �ڵ�һ����Ⱦ����ã�ֻ�ڿͻ��ˡ�֮������Ѿ������˶�Ӧ��DOM�ṹ������ͨ��this.getDOMNode()�����з��ʡ� ������������JavaScript���һ��ʹ�ã���������������е���setTimeout, setInterval���߷���AJAX����Ȳ���(��ֹ�첽��������UI)��
- **componentWillReceiveProps** ��������յ�һ���µ� prop (���º�)ʱ�����á���������ڳ�ʼ��renderʱ���ᱻ���á�
- **shouldComponentUpdate** ����һ������ֵ����������յ��µ�props����stateʱ�����á��ڳ�ʼ��ʱ����ʹ��forceUpdateʱ�������á� 
  ��������ȷ�ϲ���Ҫ�������ʱʹ�á�
- **componentWillUpdate**��������յ��µ�props����state����û��renderʱ�����á��ڳ�ʼ��ʱ���ᱻ���á�
- **componentDidUpdate** �������ɸ��º��������á��ڳ�ʼ��ʱ���ᱻ���á�
- **componentWillUnmount**������� DOM ���Ƴ�֮ǰ���̱����á�
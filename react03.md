# React 

## 非受控组件

- ref: 用来获取 DOM 对象，或者 组件的实例

```js
const Hello = () => {
  const txtRef = React.creatRef()


  // txtRef.current.value 文本框的值

  return (
    <input ref={txtRef} />
  )
}

```

## 组件通信

- 组件是相互独立且封闭的，无法直接获取到外部的数据，所以，需要一个机智来实现获取外部的数据，这就是：组件通信

### props

- 作用：接收传递的组件的数据
- 特点：
  - 只读对象！！！


```js
const Hello = props => {
  // props => { name : 'jack',age: 19}

  return (
    <div>
    </div>
  )
}

class Hello extends React.Component {
  constructor(props) {
    super(props)
    // 约定 只要写了 constructor 就要传递props给super
    console.log( this.props)
  }
  render () {
    // this.props 获取到props
    return ...
  }
}

<Hello name='jack' age={19} />
```



### 父传子
```js
class Parent extends React.Component {
  render () {
    return (
      <div>
        <Child name ='jack' />
      </div>
    )
  }
}

const Child = props => {
  // props => { name : 'jack'}
}

```



### 子 -> 父
- 思路： 父组件提供回调函数，由子组件调用，在调用的时候将数据作为参数传递


```js
class Parent extends React.Component {
  getChildMsg = (data) => {
    // data 就是传递过来的数据
  }

  render() {
    return (
      <div>
        <Child getChildMsg = {this.getChildMsg} />
      </div>
    )
  }
}

const Child = props => {
  // props => { getChildMsg : function}
  // 子组件调用父组件的方法：

  // props.getChildMsg (要传递父组件的数据)
}

```


### 兄弟组件


- 思想：`状态提升`
- 也就是说：将两个兄弟组件之间要共享的数据，提升到 离他们租金的公共父组件中，一个组件接受数据（父-> 子）：另一个组件 ： 修改数据 （ 子->父）
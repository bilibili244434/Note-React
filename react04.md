# React 

## Context

- App要传值给Child组件，可以一步步传值，还可以利用 `Context` 进行传值。
  - 作用是：跨组件传递数据
  - 使用步骤：
    - 1. 调用React.creatContext() 创建的Provider (提供数据) 和 Consumer (消费数据) 两个组件

```js
const { Prodiver, Consumer } = React.creatContext()
```

    - 2. 使用Provider组件作为父节点。

```js
<Provider>
  <div className = 'App'>
    <Child />
  </div>
</Provider>
```

    - 3. 设置value属性，表示传递的数据

```js
<Provider value='pink'>
```

    - 4. 调用Consumer组件接收数据

```js
<Consumer>
 { data => <span>data 参数表示接收到的数据 --- {data} </span>}
</Consumer>
```


因此，
 - 1 如果两个组件是远方亲戚可以使用Context实现组件通讯
 - 2 Context提供了俩个组件：Provider 和Consumer
 - 3 Provider组件：用来提供数据
 - 4 Consumer组件：用来消费数据、





## Props 深入

### children 属性

- children属性 ：表示组件标签子节点。当组件有子节点是，props就会有该属性

- children 属性和普通的props一样，值可以是任意值(文本，元素，组件，函数等)

```js

function Hello(props) {
  return (
    <div>组件的节点：{ props.children }</div>
  )
}

<Hello>我是子节点</Hello>
```

### children 校验

- 对于组件来说，props是外来的，无法保证组件的使用者传入什么格式的数据

- 如果传入的数据格式不对。可能会导致组件内部报错

- 关键问题：组件的使用这不知道明确的错误原因

因此，

- props 校验：允许在创建组件的时候，就指定props的类型，格式
- 作用：捕获使用组件时因为props导致的错误，给出明确的错误提示，增加组件的健壮性

#### 使用步骤

  - 1.安装包prop-types(yarn add prop-types/npm i prop-type)
  - 2.导入 prop-types 包
  - 3.使用组件名 .propType = {} 莱恩给组件添加校验规则
  - 4、校验规则 https://reactjs.org/docs/typechecking-with-proptypes.html 
  通过ProTypes对象指定 

```js
  import ProType from 'prop-types'

  function App(props) {
    return (
      <h1>
      Hi,{ props.color }
      </h1>
    )
  }

  App.propTypes = {
    // 约定color属性为array类型
    // 如果类型不对，啧报出错误
    colors : PropTypes.array
  }

```

#### 约束规则

  - 1.常见类型 ： array、bool、number、object、string
  - 2.React元素类型： element
  - 3.必填项：isRequired
  - 4.特定的结构对象：shape({})

### props 默认值

 - 场景：分页组件 -> 每页显示数
 - 作用：给props 设置默认值。在未传入props时生效

```js
function App(props) {
  return (
    <div>
    此处展示props的默认值： {props.pageSize} </div>
  )
}
// 设置默认值
App.defaultProps = {
  pageSize :10
}

<App />
```


## 组件的生命周期

#### 组件的生命周期概述
 - 意义：组件的生命周期有助于理解组件的运行方式、完成更复杂的组件功能、分析组件错误原因等
 - `组件的生命周期`：组件从被创建到挂载到页面中运行，再到组件不用时卸载的过程
 - 生命周期的每个阶段总是伴随着一些方法调用，这些方法就是生命周期的`钩子函数`。
 - 钩子函数的作用：为开发人员在不同阶段操作组件提供了时机。
 - `只有 类组件 才有生命周期。`

#### 生命周期的三个阶段

组件生命周期：
  
  - 1 创建时（ 挂载阶段 ）
    触发时机： 进入页面 或 组件第一次被渲染。
    因此，如果希望进入页面就触发一些操作，就可以在这些钩子函数中执行

```js
class App extends React.Component {
  // 作用：
  //  1 初始化 state
  //  2 可以使用 bind 解决事件处理程序中 this 指向问题
  constructor(props) {
    super(props)

    console.warn('1 组件生命周期钩子函数： constructor')
  }

  // 作用：
  //  1 渲染 UI，每次只要有状态变化了，那么，这个钩子函数都会被触发
  // 注意：不要在 render 方法中调用 setState() 否则，会造成递归渲染（死循环）。
  // 注意：不要将于渲染无关的逻辑放在 render 方法中！！！ render就是用来渲染UI的，
  //       那么，我们就只让它渲染UI。
  render() {
    console.warn('2 组件生命周期钩子函数： render')

    // 不要这么做！！！
    // this.setState({})

    return (
      <div>
        <h1>组件生命周期</h1>
      </div>
    )
  }

  // 执行时机： 组件已经完成渲染后（组件内容已经渲染在页面中了）会立即触发
  // 作用：
  //  1 操作DOM
  //  2 发送 ajax 请求
  componentDidMount() {
    // const h1 = document.getElementsByTagName('h1')[0]
    // h1.style.color = 'red'

    console.warn('3 组件生命周期钩子函数： componentDidMount')
  }
}

ReactDOM.render(<App />, document.getElementById('root'))
```

  - 2 更新时（ 更新阶段 ）:在组件更新后，也就是 DOM 完成重新渲染，就会触发这个钩子函数
    三种导致组件更新的情况：
      - 1 setState()
      - 2 new props 表示：组件接收到新的 props 值，那么，该组件就会被更新
      - 3 forceUpdate() 表示：强制组件触发一次更新
```js
class App extends React.Component {
  state = {
    count: 0
  }

  handleClick = () => {
    this.setState({
      count: this.state.count + 1
    })
  }

  render() {
    console.warn('组件生命周期钩子函数： render')
    return (
      <div>
        <h1>统计豆豆被打的次数： {this.state.count}</h1>
        <button onClick={this.handleClick}>打豆豆</button>
      </div>
    )
  }

  // 触发时机： 在组件更新后，也就是 DOM 完成重新渲染，就会触发这个钩子函数
  // 作用：
  //  1 DOM 操作
  //  2 可以发送 ajax，但是，一定要将 setState() 放在一个条件中运行！！！
  componentDidUpdate(prevProps, prevState) {
    // prevProps 表示：更新前的props
    // prevState 表示：更新前的state
    console.log('更新前的 state：', prevState)
    console.log('最新的 state：', this.state)

    // 如果需要获取到最新的 props 或 最新的 state ，需要通过：
    // this.props
    // this.state

    // 因此，既可以拿到更新前的数据，又可以拿到更新后的数据（最新的数据）
    // 所以，就可以比较更新前后的值，是否相同，来决定是否执行某个操作了

    const h1 = document.getElementsByTagName('h1')[0]

    // 不要这么做！！！
    // this.setState({})

    console.warn('组件生命周期钩子函数： componentDidUpdate', h1.innerText)
  }
}

ReactDOM.render(<App />, document.getElementById('root'))
```

  - 3 卸载时（卸载阶段）
    - 执行时机：组件从页面中消失

    componentWillUnmount(){

    }
    组件卸载（从页面上消失） 执行清理工作：例如清理定时器，绑定的事件


## render-props 和高阶组件

#### React组件复用概述 
  - 处理方式：复用相似的功能（联想函数封装）
  - 复用什么？1. state 2. 操作state的方法 （组件状态逻辑 ）
  - 两种方式：1. render props模式 2. 高阶组件（HOC）
  - 注意：这两种方式不是新的API，而是利用React自身特点的编码技巧，演化而成的固定模式（写法）



  在使用组件时，添加一个值为函数的prop，通过函数参数来获取（获取组件内部实现）

#### 使用步骤
 - 1.创建Mouse组件，在组件提供复用的`状态逻辑`的代码（1，状态 2 操作状态的方法）
 - 2. 将`复用的状态`作为 prop.render(`state`)方法的参数，暴露到组件外部
 - 3. 使用props.render() `返回值`作为要渲染的内容
 - 4. children代替render属性


```js
state = {
    x: 0,
    y: 0
  }
render() {
    const { x, y } = this.state
    return (
      <div>
        {/* 假设 children 是一个函数，那么，在此处就可以调用这个函数了 */}
        {this.props.children(x, y)}
      </div>
    )
  }


ReactDOM.render(
  <Mouse>
    {(x, y) => {
      // console.log('在Mouse组件外部获取到x和y：', x, y)
      return (
        <p>
          当前鼠标位置：(x: {x}, y: {y})
        </p>
      )
    }}
  </Mouse>,
  document.getElementById('root')

```

代码优化
 - 1 给render props模式添加props校验
 - 2 应该在组建卸载时解除mousemove 事件绑定


```js
Mouse.propTypes = {
  chidlren: PropTypes.func.isRequired
}
componentWillUnmount() {
  window.removeEventListener('mousemove', this.handleMouseMove)
}
```
### 高阶组件

#### 概率
 - 目的 ：实现状态逻辑复用
 - 采用包装的模式
 - 增强组件功能
#### 思路

 - `高阶组件`（HOC higher-order Component）`是一个函数`，接受要包装的组件，返回增强后的组件
 - 高阶组件内部创建`一个类组件`，在这个类组件中`提供复用的逻辑状态`代码。通过prop将复用的状态传递给被包装的组件WrappedComponent

 ```js
 const EnhanceedComponent = withHOC(WrappedComponent)
 ```
 ```js
// 高阶组件内部创建的类组件
class Mouse extends React.Component {
  render () {
    return <WrappedComponent {...this.state} />
  }
}
 ```

 #### 使用步骤
  - 1.创建一个函数，名称约定`以with开头`
  - 2.指定函数参数，参数应该以大写字母开头（作为要渲染的组件）
  - 3.在函数内部创建一个类组件，`提供复用的状态逻辑代码`，返回
  - 4.在该组件中，渲染参数组件，同时将状态prop传递给参数组件
  - 5.调用高阶组件，传入要增强的组件，通过返回值拿到增强后的组件，并将其渲染到页面中

```js
function withMouse(WrappedComponent) {
  class Mouse extends React.Component{}
  return Mouse
}

// Mouse组件中的render方法
return <WrappedComponent {...this.state } />

// 创建组件
const MousePosition = withMouse(Position)

// 渲染组件
<MousePosition />
```

#### 设置displayName
  - 使用高阶组件存在的问题：得到的两个组件名称相同
  - 原因：默认情况下，React使用组件名称作为 displayName
  - 解决方式：为 高阶组件 设置 displayName 便于调试时区分不同的组件
  - displayName的作用：用于设置调试信息（React Developer Tools信息）
  - 设置方式

```js
    Mouse.displayName = `WithMouse${getDisplayName(WrappedComponent)}`
    function getDisplayName(WrappedComponent) {
    return WrappedComponent.displayName || WrappedComponent.name || 'Component'}
```

#### 传递props
  - 问题：props丢失
  - 原因：高阶组件没有往下传递props
  - 解决方式：渲染 WrappedComponent 时，将 state 和 this.props 一起传递给组件
  - 传递方式
```js
  <WrappedComponent {...this.state} {...this.props} />
```
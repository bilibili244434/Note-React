# React 

### setState()的说明

- 更新数据
  - `setState()`是`异步`更新数据
  - 使用该语法时，后面的setState()不要依赖前面的setState()
  - 可以多次调用setState()，只会触发一次重新渲染

```js
  this.setState = { count: 1}
  this.setState({
    count:this.setState +1
  })

  console.log(this.state.count) // 1

```

- 推荐语法
  - 推荐：使用`setState((state.props) => {})`
  - 参数state：表示最新的state
  - 参数props：表示最新的props

```js
  this.setState( (state,props) => {
    return {
      count :state.count +1
    }
  })

  console.log(this.state.count) //1
```
- 第二个参数
  - 场景：在状态更新（页面完成重新渲染）后立即执行某个操作
  - 语法：`setState(update,[callback])`

```js
  this.setstate(
    (state,props) => {},
    {}=>{console.log('状态更新后执行')}
  )
```

### 组件更新机制

  - `setState()`的两个作用， 1.修改state 。2 更新组件（UI）
  - 过程：父组件重新渲染时，也会重新渲染子组件时，但只会渲染当前组件子树（当前组件及其所有子组件）

### 组件性能优化

#### 减轻state

  - `减轻state`只存储跟组件渲染相关的数据
  - 不做渲染的数据不需要放在state中，比如定时器
  - 对于这种需要多个方法中用到的数据，应该放在this中

#### 避免不必要的重新渲染

 - 组件更新机制：父组件更行引起子组件的更新。当子组件没有任何变化时也会重新渲染

 - 解决方式：使用`钩子函数 shouldComponentUpdate(nextProps，nextState)`
 - 作用：通过返回值决定该组件是否重新渲染，返回true表示重新渲染，false表示不重新渲染
 - 触发时机: 更新阶段的钩子函数，组件重新渲染前执行

 ```js
class Hello extends Component {

  shouldComponentUpdate() {
  // 根据条件，决定是否重新渲染组件
  return false
  }

  render() {…}
}
 ```

 #### 纯组件
  - 纯组件：PureComponent 与 React.Component 功能相似
  - 区别：pureComponent 内部自动实现了shouldComponentUpdate钩子，不需要手动
  - 原理 ： 纯组件内部通过分别对比前后两次的props和state的值，来决定是否重新渲染组件

```js
class Hello extends React.PureComponent {
  render() {
    return (
    <div>纯组件</div>
    )
  }
}
```
  - 说明： 纯组件内部对比的是`shallow compare`（浅层对比）
  - 对于值类型来说，比较两个值是否相同

```js
let number = 0
let newNumber = number
newNumber = 2
console.log(number === newNumber) // false
state = { number: 0 }
  setState({
  number: Math.floor(Math.random() * 3)
})
// PureComponent内部对比：
最新的state.number === 上一次的state.number // false，重新渲染组件
```

  - 对于引用类型来说，只比较对象的引用（地址）是否相同
  - 注意：`state或Props中属性值为引用类型时，应该创建新数据，不要直接修改原数据`
  例如

```js
// 正确！创建新数据
const newObj = {...state.obj, number: 2}
setState({ obj: newObj })
// 正确！创建新数据
// 不要用数组的push / unshift 等直接修改当前数组的的方法
// 而应该用 concat 或 slice 等这些返回新数组的方法
this.setState({
list: [...this.state.list, {新数据}]
}
```

### 虚拟DOM 和 Diff 算法

  - React 更新视图的思想，只要state变化就重新更新渲染视图
  - 理想状态是：`部分更新`，只需要更新变化的地方

  - 方法：`虚拟DOM配合Diff算法`


#### 一个组件内部更新机制

- 只要想让状态发生变化，就调用 setState()，只要调用 setState()，就会执行组件的 render 方法。来重新渲染组件
- 注意：render 重新执行，不代表把整个组件重新渲染到页面中。而实际上，React内部会使用 *虚拟DOM* 和 *Diff 算法*来做到 **部分更新**。
  - 部分更新：只将变化的地方重新渲染到页面中，这样，DOM更新降到了最低

### 虚拟DOM
- 虚拟DOM（Virtual DOM）
- 本质上就是一个普通的Js对象，他是用来描述你希望页面中看到的内容（UI）

### Diff 算法的说明 - 1

- 如果两棵树的根元素类型不同，React 会销毁旧树，创建新树

```js
// 旧树
<div>
  <Counter />
</div>

// 新树
<span>
  <Counter />
</span>

执行过程：destory Counter -> insert Counter
```

### Diff 算法的说明 - 2

- 对于类型相同的 React DOM 元素，React 会对比两者的属性是否相同，只更新不同的属性
- 当处理完这个 DOM 节点，React 就会递归处理子节点。

```html
// 旧
<div className="before" title="stuff"></div>
// 新
<div className="after" title="stuff"></div>
只更新：className 属性

// 旧
<div style={{color: 'red', fontWeight: 'bold'}}></div>
// 新
<div style={{color: 'green', fontWeight: 'bold'}}></div>
只更新：color属性
```

### Diff 算法的说明 - 3

- 1 当在子节点的后面添加一个节点，这时候两棵树的转化工作执行的很好

```js
// 旧
<ul>
  <li>first</li>
  <li>second</li>
</ul>

// 新
<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>

执行过程：
React会匹配新旧两个<li>first</li>，匹配两个<li>second</li>，然后添加 <li>third</li> tree
```

- 2 但是如果你在开始位置插入一个元素，那么问题就来了：

```js
// 旧
<ul>
  <li>1</li>
  <li>2</li>
</ul>

// 新
<ul>
  <li>3</li>
  <li>1</li>
  <li>2</li>
</ul>

执行过程：
React将改变每一个子节点，而非保持 <li>Duke</li> 和 <li>Villanova</li> 不变
```
### key 属性

> 为了解决以上问题，React 提供了一个 key 属性。当子节点带有 key 属性，React 会通过 key 来匹配原始树和后来的树。

```js
// 旧
<ul>
  <li key="2015">1</li>
  <li key="2016">2</li>
</ul>

// 新
<ul>
  <li key="2014">3</li>
  <li key="2015">1</li>
  <li key="2016">2</li>
</ul>

执行过程：
现在 React 知道带有key '2014' 的元素是新的，对于 '2015' 和 '2016' 仅仅移动位置即可
```

- 说明：key 属性在 React 内部使用，但不会传递给你的组件
- 推荐：在遍历数据时，推荐在组件中使用 key 属性：`<li key={item.id}>{item.name}</li>`
- 注意：**key 只需要保持与他的兄弟节点唯一即可，不需要全局唯一**
- 注意：**尽可能的减少数组 index 作为 key，数组中插入元素的等操作时，会使得效率底下**

## 虚拟DOM的真正价值

- 虚拟DOM的真正价值： 虚拟DOM让React代码摆脱了浏览器的限制（束缚），只要能够运行JS的地方，就可以执行 React 代码。
- 所以，使用 React 能够实现跨平台应用。
- 也可以将 React 看做是： 面向虚拟DOM编程

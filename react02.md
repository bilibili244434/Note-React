# React

## 创建组件

### 使用函数创建组件

- 函数组件

- 1.组件名首字母必须大写
- 2.必须要有返回值
		- 2.1 可以返回null，表示什么都不渲染
		 - 2.2 可以返回JSX

```js
function Hello() {
	return <div>组件</div>
}

const Hello = () => <div>组件</div>

```

### 使用Class创建组件

- 类组件
	 - 1 必须提供render方法
	 - 2 render 方法必须有返回值，要求与 函数组件的要求相同

```js
class Hello extends React.Component {
	render () {
		return (
			<div>组件</div>
		)
	}
}
```

## 抽离组件

- 讲组件抽离到独立的JS文件中时，只要用到了JSX 就需要导入React
- JSX -> React.creatElement()

## 事件绑定 和 事件对象

- on + 事件名称
- 命名约定：驼峰命名法
- 通过 事件处理程序的参数 e 就可以获取到事件对象
	- 注意：如果在处理事件程序中获取到 事件对象 的属性或方法，就需要调用
	`e.persist()`


```js
<button onClick={(e) => {}}></button>
```

### 绑定事件说明

- 事件处理程序是一个函数，所以，不能调用！！！

```js
<button onClick={(e) => {}}></button>
<button onClick={this.handleClick}></button>

```

## 有状态组件和无状态组件

- 状态（state） 即数据

- 函数组件 又叫做 ：无状态组件
	-没有状态，也就无法让状态变化，也就无法让页面动起来。因此，只能负责页面展示（静态）
	- 还可以叫做：木偶组件
	- 职责 ：给函数组件传递什么数据，函数组件就负责展示什么数据

- 类组件 要比 函数组件 功能 更加强大

- 如何选择使用哪种组件？
  - 如果一个页面没有变化，仅仅是 静态结构 的展示，此时，就使用：函数组件
  - 如果一个页面是有变化（ 计数器中的数值改变了 ），也可以说是需要跟用户进行交互，此时，就使用：类组件

## 类组件为什么要继承 React.Component

- 因为继承了 React.Component 以后，那么，类组件中就可以使用像 `setState()` 这样的方法了


## 事件处理程序中 this 指向问题

- 1 箭头函数

```js
class Hello extends React.Component {
  handleClick() {
    this....
  }

  render() {
    return (
      <button onClick={() => this.handleClick()}>按钮</button>
    )
  }
}
```

- 2.bind

```js
class Hello extends React.Component {
	constructor() {
		super()

		// use bind 

		this.handleClick = this.handleClick.bind(this)
	}


handleClick() {
	this....
}

render() {
	return (
		<button onClick = { this.handleClick}>按钮</button>
	)
 }
}
```

- 3 箭头函数形式的实例方法
```js
class Hello extends React.Component {
	// 使用箭头函数
	handleClick = () => {
		this....
	}

	render () {
		return (
		<button onClick = { this.handleClick}>按钮</button>
		)
	}
}

```


## 受控组件


- 1 HTML 中的表单元素是有自己的可变状态的，这个可变状态指的是：可以输入的内容
  - 对于 input 来说，就是 value 值
- 2 但是，React 中要求 可变化的状态 要保存在 state 中，并且，只能通过 setState() 这个方法来修改
- 3 这样，就产生了一个冲突（ 也就是 表单元素 自身可变化的特点 与 React 中可变的 state 冲突了 ），如何解决？？？
- 4 解决方式： 让 React 中的state 来 接管 表单元素可输入的状态，也就是 表单元素的 value

```js
// 让 表单元素 的value值（可变化的状态），变为 react 中的state
// 这样，这个表单元素的值，就受到 react 状态的控制了
<input value={this.state.txt} />
```

- 我们把其值受到 React 控制的表单元素，称为：受控组件

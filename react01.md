# React

## React是什么

- React是一个JS库，用来构建用户界面（写HTML，构建web应用）
- 从 MVC 的角度来看，相当于 视图层 V（View） 的内容。

## React特点

1.声明式![react01-1](F:\myStudy\fighting\React\react01-1.png)

而页面如何渲染，以及数据如何渲染和更新都不需要关心，都由react负责处理。

- 2 基于组件（组件化）
- 组件都是组件化的开发模式
- 3 学习一次，随处使用（Web 、 安卓/ios、vr ...）

## React的使用

1.下载并引入

npm i react react-dom

引入![react01](F:\myStudy\fighting\React\react01.png)

2.创建react元素

3.渲染react元素

```js
import React from 'react
import ReactDOM from 'react-dom'

const title = React.creatElement('h1',null,'Hello React')
ReactDom.render(title,document.getElementById('root'))
```

## NPX

1.使用脚手架出师项目的对比：

​	原来（不使用npx）：首先安装全局脚手架的包，在通过脚手架包中提供的命令来初始化项目

​	现在（使用npx）：直接通过npx来调用脚手架提供的命令来初始化项目

2.npx的原理：临时安装脚手架包，使用脚手架包提供的命令来初始化项目，移除临时安装的脚手架的包

3.npx是npm v5.2中提供的命令

## 使用脚手架初始化项目的过程

1.

```js
npx create-react-app my-app
```

npx react 脚手架名称 项目名称

2.

```js
cd my-app
```

3.

```js
yarn start 或者 npm start
```

## JSX 的注意点

- 特殊属性：

  - class --> className

- 属性命名遵循 驼峰命名法

- 可以使用：

  - `<div></div>`
  - `<div />`

- 推荐使用 () 包裹 JSX 结构

- ```js
  // 命令式：
  // const title = React.createElement('h1', null, 'Hello React')
  // 声明式：
  const title = <h1>Hello React</h1>
  
  ```

## 在JSX中使用JS的数据

语法：`{ JS表达式 }`

- {} 中允许出现任意的 JS 表达式
- JSX 自身也是一个合法的 JS 表达式

## React的条件渲染

1.使用if/else 来实现

```js
const loadData = () => {
if(isloading) {
	return <div>loading.....</div>
	}
return <div>加载完成后的列表</div>
}

const h1 = <div>{loadData()}</div>
```



2.使用三元表达式

```js
const loadData = （）=> {
	return isloading ? <div>loading...</div> : <div>加载完成后的列表结构</div>
}
```


# React 

## React 路由基础

### React 路由介绍
现代前端应用大多是`SPA`（单页应用程序）,也就是只有一个HTML页面的应用程序，因他的用户 体验更好，对服务器压力小，

  - 前端路由的功能：让用户从一个视图导航到另一个视图（页面）
  - 前端路由是一套映射规则，在React中，是`URL路径`与`组件`的对应关系
  - 使用React路由简单来说，就是配置`路径`和`组件`


### 路由的基本使用

#### 使用步骤
  - 安装： yarn add react-router-dom
  - 导入路由的三个核心组件： `Router`/`Route`/`Link`

```js
 import { BrowserRouter as Router, Route, Link } from 'react-router-dom'
```

  - 使用`Router组件`包裹整个应用（重要）

```js
  <Router>
    <div className="App">
      // … 省略页面内容
    </div>
  </Router>

```

  - 使用`Link组件`作为导航菜单（路由入口）
```js
  <Link to= "/first">页面一</Link>
```

  - 使用`Router组件`配置路由规则和要展示的组件（路由出口）

```js
  const First = () => <p>页面一的内容</p>
  <Router>
    <div className="App">
      <Link to="/first">页面一</Link>
      <Route path="/first" component={First}></Route>
    </div>
  </Router>
```

#### 常用组件说明

  - Router组件：包裹整个应用，一个React应用只需要`使用一次`
  - 两种常用Router：HashRouter 和BrowserRouter
  - HashRouter：使用URL的哈希值实现（localHost：3000/#/first）
  - (推荐)BroswerRouter：使用H5的history API 实现（localhost：3000/first）
  - Link组件： 用于指定导航链接（a标签）
```js
// to 属性：浏览器地址栏中的pathname（location.pathname）

<Link to='/first'>页面一</Link>
```

  - Route组件：指定路由展示组件相关信息

```js
// path属性：路由规则
// component属性：展示的组件
// Route组件写在哪里，渲染出来就在哪里

<Route path="/first" component={First}></Route>

```

### 路由的执行过程
  - 1 点击Link（a标签），修改了浏览器地址栏的url
  - 2 React路由监听到地址栏url。
  - 3 React路由内部遍历所有Route组件，使用路由规则（path）和pathname进行匹配
  - 4 当路由规则（path）能够匹配地址栏中的pathname时，就展示该Route组件的内容

localhost:3000/first -> pathname:/first


### 编程式导航

   - 编程式导航：通过JS代码来实现页面跳转
   - history 是React路由提供的，用于获取浏览器历史记录的相关信息
   - push（path） ：跳转到某个页面，参数path表示要跳转的路径
   - go（n）：前进或后退到某个页面，参数n表示前进或后退的页面数量（比如：-1，表示后退到上一页）

```js
class Login extends Component {
  handleLogin= () => {
    this.props.history.push('/home)
  }

  render() {..}
}
```

### 默认路由 

 - 默认路由:便是进入页面时就会匹配的路由
 - 默认路由path为：/

```js
<Route path='/' component = {Home}>
```

### 匹配模式

#### 模糊匹配模式

   - path 代表Route组件的path属性
   - pathname 代表Link组件的同属性（也就是 location.pathname）

#### 精确匹配

   - 给Route组件添加exact属性，让其变为精确匹配模式
   - 精确模式：只有当path 和pathname完全匹配时才会展示该路由

```js 
<Route exact path='/' component =... />
```

React 路由的一切都是组件，可以向组件一样思考路由

### 嵌套路由（针对好租客项目）

- 有两个组件，分别是：
  - Home组件
  - News组件

- 这两个组件的关系：嵌套关系（News组件 嵌套在Home组件中展示的）

- 要想展示News组件，首先保证Home组件是展示的

  - 因为News是展示在Home组件中的，要想展示子组件，必须保证父组件先展示出来
- 因此 我们要写一个pathname，既能够匹配Home组件的路由规则，又能匹配News组件的路由规则

```js
假设：

    Home 组件的路由规则： /home
    News 组件的路由规则： /news

    Home 组件的路由规则： /home
    News 组件的路由规则： /home/news

```

- 总结：子路有的路由规则需要以路由规则开头

- 因为在React 路由实际上就一个组件，所以我们可以按照组件的路由方式来思考

- 变化的地方，都可以使用路由中的Route组件来配置，不变的地方，直接写死HTML结构


### 路由重定向

- 需求： 当访问默认路由时（/）时候。能够自动跳转/home路由
- 需要使用： 路由的重定向功能

```js
<Route exact path='/' render={() => <Redirect to='/home'>} />
```


### load 事件
```js 
// 页面内容加载完成后，会触发这个事件
window.onload = function() {}

// 图片也有load事件

img.onload = function() {}

// $(function() {})

```
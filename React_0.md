# 
Virtual Dom
JSX
Components
Composition
数据绑定？

## React编程： 声明式 & 函数式
### 声明式
命令式关注 how, 而声明式关注 what
### 函数式
函数是一等公民
函数式组件 -> Hooks
纯函数


## 组合与继承
组合-》将复杂的任务拆分为多个耦合度低的简单的子任务-》处理各个子任务 将他们重新组合得到任务解决方案
包含关系（子组件具体内容不可知） props.children
特例关系 props 定制渲染一般组件

## JSX
函数 React.createElement(component, props, ...children) 的语法糖
通用表达式不能作为 React 元素类型
Props 默认值为 “True”


## React.Component 
### React.PureComponent && React.Component

class组件

- setState()
    异步，调用 setState()后立即读取 this.state 会有bug。解决方案：使用 componentDidUpdate 或者 setState 的回调函数
    【实例！！！】
    Q2: state & setState
- forceUpdate()

Class属性

### 生命周期

#### 挂载
##### constructor()
功能：
- 初始化内部`state`；
- 为事件处理函数绑定实例；
```
constructor(props) {
  super(props);
  this.state = { counter: 0 }; 
  this.handleClick = this.handleClick.bind(this);
}
```

##### static getDerivedStateFromProps()
##### render()
    纯函数，（此时不会直接与浏览器交互）
##### componentDidMount()
    组件挂载后立即调用
    可以在此处直接调用 setState(), 在浏览器更新屏幕之前进行渲染，用户不会看到中间状态
    场景应用：实例化请求/添加订阅

废弃方法：
componentWillMount()

 Q1：[state依赖props的状态如何处理/props&state](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)

#### 更新
- static getDerivedStateFromProps()
- shouldComponentUpdate()
    `shouldComponentUpdate(nextProps,nextState)`
    当 props / state 发生变化时，shouldComponentUpdate() 会在**渲染执行之前**被调用，返回值默认为 true（默认行为： state 每次发生变化组件都会重新渲染）
    调用`forceUpdate()`时不会调用该方法
- render()
    shouldComponentUpdate()返回 false则不会再调用render()
- getSnapshotBeforeUpdate()
    应用于 UI 处理
- componentDidUpdate()
    更新后会被立即调用（首次渲染时不执行）
    ```
    componentDidUpdate(prevProps, prevState, snapshot)
    ```
    snapshot 为 getSnapshotBeforeUpdate()函数的返回值， 默认为 undefined

废弃方法：
componentWillUpdate()

componentWillReceiveProps()

#### 卸载
componentWillUnmount()
在组件销毁 之前直接调用
使用场景：取消订阅/取消网络请求

#### 错误处理
static_getDerivedStateFromError()
componentDidCatch()

#### 其他API
setState()
    异步，调用 setState()后立即读取 this.state 会有bug。解决方案：使用 componentDidUpdate 或者 setState 的回调函数
    【实例！！！】
    Q2: state & setState



forceUpdate()

class属性：
defaultProps
displayName()

实例属性：
props
state

## Refs
1、refs 的使用场景：—— 在典型数据流之外强制修改子组件
- 管理焦点，文本选择/媒体播放
- 触发强制动画
- 集成第三方 DOM 库

2、创建: React.createRef()
3、访问: ref.current
ref 的值取决于
- 节点为 html 元素： 接收底层 DOM 作为 current 属性
- 节点为自定义 class 组件： 接收组件的挂载实例
- **函数组件通常不能使用 ref 属性：函数组件没有实例**
    如果要在函数组件中使用 ref，你可以使用 forwardRef（可与 useImperativeHandle 结合使用），或者可以将该组件转化为 class 组件（Q）
    可以在函数组件内部使用 ref 当它指向一个 DOM元素或 class 组件时
4、ref 转发
允许组件接收 ref 并将其向下传递给子组件
React.forwardRef
...
5、回调 ref
...

## Fragments


## 高阶组件
参数为组件，返回值为新组件的函数
组件： props->UI
高阶组件：组件->组件

横切关注点问题

纯函数，不修改原始组件，而是使用组合的方式将组件包装在容器中实现功能。
传递与自身无关的props
refs不会被传递

## render props



## Hook
状态逻辑的共享
**关注点分离**： Hook 允许我们按照代码的用途进行分离
解决 class 中 生命周期经常包含不相关的逻辑，但又把相关逻辑分离到几个不同的方法中的问题，将不相关的代码进行分离，（可读性似乎也变强了:)更容易进行代码的维护。

### State Hook
在函数组件中添加内部 state

### Effect Hook
JavaScript 闭包机制
useEffect 每次渲染后都会执行：保证一致性，避免更新逻辑处理的遗漏而导致的bug
-》控制 useEffect：传入第二参数使得该特定值未发生改变时跳过 Effect 进行性能优化；第二参数传入一个空数组（[]）时可构建只运行一次的 effect
每一个 effect 都可以返回一个清除函数（可选的清除机制）

### Hook API

基础 Hook
- useState
    函数式更新
    Q：比较useState与class setState区别
    Q：[object is比较](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is#Description)
- useEffect
- useContext

额外
- useReducer
- useCallback
- useMemo
- useRef
    每次渲染时返回同一个 ref 对象
    *变更 .current属性不会引发重新渲染





**todo**: 
- hook api
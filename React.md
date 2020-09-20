

## 组合与继承

包含关系 props.children
特例关系

## React.Component 
class组件

- setState()
    异步，调用 setState()后立即读取 this.state 会有bug。解决方案：使用 componentDidUpdate 或者 setState 的回调函数
    【实例！！！】
    Q2: state & setState
- forceUpdate()

Class属性

### 生命周期
#### 挂载
- **constructor()**
    初始化内部`state`；
    为事件处理函数绑定实例；
- **static getDerivedStateFromProps()**
- **render()**
    纯函数，（此时不会直接与浏览器交互）
- **componentDidMount()**
    组件挂载后立即调用
    可以在此处直接调用 setState(), 在浏览器更新屏幕之前进行渲染，用户不会看到中间状态



 Q1：[state依赖props的状态如何处理/props&state](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)

#### 更新
- static getDerivedStateFromProps()
- shouldComponentUpdate()
- render()
- getSnapshotBeforeUpdate()
- componentDidUpdate()

#### 卸载

#### 错误处理

## 高阶组件
参数为组件，返回值为新组件的函数
组件： props->UI
高阶组件：组件->组件

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
- useEffect
- useContext
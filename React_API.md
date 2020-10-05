## class 组件 API

### 生命周期

#### 挂载
执行顺序：
constructor() -> static getDerivedStateFromProps() -> render() -> componentDidMount()
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

`static getDerivedStateFromProps(props,state)`
该方法会返回一个对象来更新 state， 若返回 null 则不更新任何内容。

使用场景：
    适用于罕见的实例，**即当 state 的值在任何时候都取决于 props**
    【派生状态】：
    派生状态会导致代码冗余，并使组件难以维护需要关注的替代方案：
    - 执行副作用： componentDidUpdate()
    - 在 prop 更改时重新计算某些数据： memoization 
    - 在 prop 更改时“重置”某些 state ： 组件完全受控/使用 key 使组件完全不受控

Attention：
- 在 render 之前被调用，且在初始挂载阶段和后续更新时均会被调用。
- 和被废弃的 `componentWillReceiveProps`相比，该方法会在每次渲染前被触发。而后者仅在父组件重新渲染时触发，而不是内部调用 ssetState 时。

##### render()
纯函数，（此时不会直接与浏览器交互）
##### componentDidMount()
在组件挂载后立即调用
可以在此处直接调用 setState(), 在浏览器更新屏幕之前进行额外的渲染，用户不会看到中间状态
场景应用：实例化请求/添加订阅

废弃方法：
componentWillMount()

 Q1：[state依赖props的状态如何处理/props&state](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)

#### 更新
执行顺序：
static getDerivedStateFromProps() -> shouldComponentUpdate() -> render() -> getSnapshotBeforeUpdate() -> componentDidUpdate()

##### static getDerivedStateFromProps()

##### shouldComponentUpdate()
`shouldComponentUpdate(nextProps,nextState)`
根据该方法的返回值来判断组件的输出是否受当前 state / props 更改的影响。当 props / state 发生变化时，shouldComponentUpdate() 会在**渲染执行之前**被调用，返回值默认为 true（默认行为： state 每次发生变化组件都会重新渲染）
调用`forceUpdate()`时不会调用该方法

##### render()
shouldComponentUpdate()返回 false则不会再调用render()

##### getSnapshotBeforeUpdate()
`getSnapshotBeforeUpdate(prevProps, prevState)`
 在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。
 该方法的返回值将作为参数传递给 componentDidUpdate()

应用于 UI 处理

##### componentDidUpdate()
更新后会被立即调用（首次渲染时不执行）
```
componentDidUpdate(prevProps, prevState, snapshot)
```
snapshot 为 getSnapshotBeforeUpdate()函数的返回值， 默认为 undefined
若 shouldComponentUpdate() 返回值为 false ，则本方法不会被调用。
当组件更新后，可以在此处对 DOM 进行操作，如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求。
```
componentDidUpdate(prevProps) {
    if(this.props.userID !== prevProps.userID) {
        this.fetchData(this.props.userID);
    }
}
```

废弃方法：
componentWillUpdate()
componentWillReceiveProps()

#### 卸载
componentWillUnmount()
在组件销毁之前直接调用
使用场景：执行必要的清理操作，如取消订阅/取消网络请求

#### 错误处理
##### static_getDerivedStateFromError()
会在后代组件抛出错误后被调用。它将抛出的错误作为参数，并返回一个值以更新 state。
本方法会在**render阶段**被调用，不允许出现副作用。
##### componentDidCatch()
`componentDidCatech(error,info)`
本方法在**commit阶段**被调用，允许执行副作用。

Error boundaries
#### 其他 APIs
- setState()
- forceUpdate()

#### 废弃方法
挂载阶段： 
componentWillMount()
更新阶段：
componentWillUpdate()
componentWillReceiveProps()

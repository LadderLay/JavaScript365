## Hook
状态逻辑的共享
不更改组件结构的情况下复用状态逻辑
**关注点分离**： Hook 允许我们按照代码的用途进行分离
解决 class 中 生命周期经常包含不相关的逻辑，但又把相关逻辑分离到几个不同的方法中的问题，将不相关的代码进行分离，（可读性似乎也变强了:)更容易进行代码的维护。

### State Hook
在函数组件中添加内部 state

```
function Example() {
    const [count, setCount] = useState(0);
}
```

### Effect Hook
执行 副作用 操作
JavaScript 闭包机制

```
useEffect(() => {
    function A() {
        //订阅操作
        //...
    }
}
    return function B() {
        //清除操作 【可选的清除机制】
        //...
    }
)
```

useEffect 每次渲染后都会执行：保证一致性，避免更新逻辑处理的遗漏而导致的bug。
-》控制 useEffect：传入第二参数使得该特定值未发生改变时跳过 Effect 进行性能优化；第二参数传入一个空数组（[]）时可构建只运行一次的 effect

effect 的清除阶段在每次重新渲染时都会执行

### Hook API

基础 Hook
- useState
    函数式更新
    Q：比较useState与class setState区别
    Q：[object is比较](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is#Description)
- useEffect
    Q： useEffect & old state/props!

- useContext

额外
- useReducer
- useCallback
返回一个 memoized回调函数

- useMemo
- useRef
    每次渲染时返回同一个 ref 对象
    *变更 .current属性不会引发重新渲染


#### useEffect
- []
- infinite refetching loop
- get old state or prop
- useEffect & componentdidmount




#### Reading note
components
top-down data flow
**why hooks?**

在React中，由于逻辑是有状态的，并且不能提取到函数、组件紧密，我们通常很难进一步分离复杂的组件。（举例：）
Hook使得这些问题能够迎刃而解。通过Hook，我们能将组件内部的逻辑组织为可重用的独立单元。

应用场景：
1、Huge components 难以重构和测试的大型组件
2、Duplicated logic 不同组件和生命周期之间重复的逻辑
3、Complex patterns，比如 高阶组件， render props

```
//class 写法
class Test extends React.Component {
    componentDidMount() {
        //订阅操作
    }
    componentWillUnmount() {
        //取消订阅操作
    }
    ...
}

```

```
//hook 写法
function Test() {
    useEffect(()=> {
        //订阅操作

        return ()=> {
        //取消订阅操作
        }
    })
}

```

class组件之间往往难以分享不可见的逻辑



多个effect可以实现关注点分离，使得解决特定领域问题代码从业务代码中独立出来。关注点分离使得在后续的代码维护和开发时，当你修改某一关注点的部分代码时，无需知道其他部分的细节对这些部分进行相应的修改






**todo**: 
- hook api
- 数组解构的语法
- useState & this指向的实现
- 什么是 关注点
- useEffect: infinite loop / old state and props
- [] 的问题
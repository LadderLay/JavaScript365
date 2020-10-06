# Class Vs Hooks
```
//class写法
class Example extends React.Component {

    componentDidMount() {
        //订阅操作...
    }

    componentWillUnmount() {
        //取消订阅...
    }

}

```

```
//hook写法
function Example() {
    useEffect(() => {
        //订阅操作... 
    }
    return () => {
        //取消订阅...
    }
    )
}
```
Hook 允许我们按照代码的用途进行分离。
Hook 提供了一种方案，解决 class 中 生命周期经常包含不相关的逻辑，但又把相关逻辑分离到几个不同的方法中的问题，将不相关的代码进行分离，更容易进行代码的维护。

【关注点分离】

# Why hooks?

在React中，由于逻辑是有状态的，并且不能提取到函数、组件，我们通常很难进一步分离复杂的组件。  
Hook使得这些问题能够迎刃而解。通过Hook，我们能将组件内部的逻辑组织为可重用的独立单元。

应用场景：  
1、Huge components 难以重构和测试的大型组件  
2、Duplicated logic 不同组件和生命周期之间重复的逻辑  
3、Complex patterns，比如 高阶组件， render props  

Hook 在组件内部而不是仅在组件之间应用 Hook 原理（明确的数据流和组合）。和 class 时代提供的解决方案相比（render props , 高阶组件）， Hook使你**在无需修改组件结构的情况下复用状态逻辑**，同时也不会引入不必要的嵌套。


# Hook 简介
## State Hook
在函数组件中添加内部 state

```
function Example() {
    const [count, setCount] = useState(0);
}
```

## Effect Hook
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

多个effect可以实现关注点分离，使得解决特定领域问题代码从业务代码中独立出来。关注点分离使得在后续的代码维护和开发时，当你修改某一关注点的部分代码时，无需知道其他部分的细节对这些部分进行相应的修改

## Hook APIs
### useState
`const [state, setState] = useState(initialState);`  


### useEffect
使用 useEffect 完成副作用操作。  

赋值给 useEffect 的函数会在**组件渲染到屏幕之后**执行。
```
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```  
render -> `<p>You clicked 0 times</p>.`  
effect -> `() => { document.title = 'You clicked 0 times' }`


Render: 
- each render has its own props and state
- each render has its own event handles
- each render has its own effects
  
**every function** inside the component render (including event handlers, effects, timeouts or API calls inside them) captures the props and state of the render call that defined it.  
  
Inside the scope of a single render, props and state stay the same. 

**1、effect 清理机制：**  
假设有 props:{id: 10}, {id: 20}  
  x
通常的错误理解： 
- cleans up the effect for {id: 10}
- renders UI for {id: 20}
- run the effect for {id: 20}
事实上：  
- renders UI for {id: 20}
- cleans up the effect for {id: 10}
- run the effect for {id: 20}

React 中 每一个 render 有独立的 props/state， 同样，effect 清除机制不会去读取最新的 props, 而是会读取对应 render 的 props/state 数据。
```
// First render, props are {id: 10}
function Example() {
  // ...
  useEffect(
    // Effect from first render
    () => {
      ChatAPI.subscribeToFriendStatus(10, handleStatusChange);
      // Cleanup for effect from first render
      return () => {
        ChatAPI.unsubscribeFromFriendStatus(10, handleStatusChange);
      };
    }
  );
  // ...
}

// Next render, props are {id: 20}
function Example() {
  // ...
  useEffect(
    // Effect from second render
    () => {
      ChatAPI.subscribeToFriendStatus(20, handleStatusChange);
      // Cleanup for effect from second render
      return () => {
        ChatAPI.unsubscribeFromFriendStatus(20, handleStatusChange);
      };
    }
  );
  // ...
}
```

**2、useEffect 第二参数：**  
- 传入第二参数使得该特定值未发生改变时跳过 Effect 进行性能优化；  
- 第二参数传入一个空数组（[]）时可构建只运行一次的 effect


### useReducer

### useCallback

### useMemo

### useLayoutEffect
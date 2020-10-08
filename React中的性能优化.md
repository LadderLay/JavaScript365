## shouldComponentUpdate
默认行为是返回 true 让 React 组件执行更新。
```
shouldComponentUpdate(nextProps, nextState) {
    return true;
}
```

可以手动编写该方法返回 false 来跳过后续的渲染过程，但不是建议的写法。官方文档更建议使用 内置的 PureComponent。 需要注意的是。即使返回 false 也不会影响子组件在 state 更改时重新渲染。

## pureComponent
React.PureComponent 与 React.Component 类似，两者的区别在于 React.Component 并未实现`shouldComponentUpdate()`，而 React.PureComponent 中以**浅层对比 prop 和 state** 的方式来实现了该函数。
如比较的对象包含复杂的数据结构，则可能产生错误的比对结果。
需要注意的是，React.PureComponent 中的 shouldComponentUpdate() 将跳过所有子组件树的 prop 更新。因此，需所有子组件也都是“纯”组件.
## memo
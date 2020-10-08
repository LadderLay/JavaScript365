## 引言——横切关注点：
维基的定义： 

部分关注点“横切”程序代码中的数个模块，即在多个模块中都有出现，它们即被称作“横切关注点“。  
横切关系不能自然得适应面向对象/面向过程得编程理念，导致出现代码过于分散、代码冲突的问题。（举例：日志功能的实现。）  

而高阶组件和 Render Prop 正是 React 给出的针对横切关注点的解决方案。

【**面向切面的程序设计**：将横切关注点和与业务代码进一步分离，来提高代码的模块化程度。】

【补充：mixins 方案及其的缺陷】

## 高阶组件（HOC）
`const EnhancedComponent = higherOrderComponent(WrappedComponent);`
定义：  
高阶组件是**参数为组件，返回值为新组件**的函数。  
高阶组件是 React 中用于**复用组件逻辑**的一种技巧。在 React 中我们可以通过使用 HOC 来解决横切关注点相关的问题。  
高阶组件使得我们可以进行抽象，在一个地方定义逻辑，并且在许多组件共享该逻辑。

需要注意的是，高阶组件是没有副作用的纯函数。高阶组件不会修改传入的组件，也不会使用继承来复制行为。相反，HOC 通过将组件包装在容器组件中来组成新组件。

[一个简化的 Example](https://codesandbox.io/s/hoc001-4gwtr?file=/src/App.js)

注意：  
- 不在 render 中使用 HOC
- 复制静态方法。将 HOC 应用于组件时，原始组件将使用容器组件进行包装，此时新组件将丢失原始组件的静态方法。
- ref 不会被传递。解决方案：使用 React.forwardRef
   
【引申： 设计模式 装饰者模式】






## Render Props
定义：   
Render Prop 指的是一种在 React组件之间使用一个值为**值为函数**的 prop 共享代码的技术。具有 render prop 的组件接受一个**返回 React 元素**的函数，并在组件内部通过调用此函数来实现自己的渲染逻辑。

具体来说， render prop 是一个用于告知组件需要渲染什么内容的函数 prop，它使得我们可以在组件之间共享行为。

render **prop**  
 任何被用于告知组件需要渲染什么内容的函数 prop 在技术上都可以被称为 “render prop”.

注意：带 render prop 的常规组件可以实现大多数 HOC

[Demo](https://codesandbox.io/s/a-simple-demo-for-renderprop-73fg9?file=/src/MouseTracker.js)

和 HOC 相比，render props 的构建是动态的。

**Attention：**   
render prop 和 pureComponent 同时使用时的问题

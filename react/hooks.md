# react-hooks 基本介绍

## 使用规则
1. react 16.8版本后支持
2. hooks 只能在函数式组件中使用 **functional component**
3. hooks 只能在函数组件的顶层中使用，不能在循环遍历中或者多层嵌套函数中等使用
---

## 官方提供的基本hooks

- [useState](#useState)
- [useEffect](#useEffect)
- [useContext](#useContext)
- useCallback
- useRef
- useReducer
- useLayoutEffect
- useMemo
- useImperativeMethods
---

### useState

描述: 返回一个state，和一个更改state的函数

基本用法
```
const [state, setState] = React.useState(initialValue)
```
上面👆代码 初始化了一个`initialValue`的state值 并返回了一个数组, 数组的第一个为state值，第二个为一个函数 用来更改state的值  类似于`class component`中的 state和setState

- useState 可以接收一个值为参数，也可以接收一个函数为参数 取返回值为初始state
- setState支持传入state 或者一个回调函数，不支持第二个参数

**一个简单的例子, 每次点击按钮 `count` 的值都会加1**
```
function App(){
    const [count, setCount] = React.useState(0)
    return (
        <div>
            <h1>count的值为:{count}</h1>
            <button onClick={()=>setCount(count+1)}>increment</button>
        </div>
    )
}
```
---

### useEffect

描述:  useEffect 允许你使用副作用,在每次render之后执行, 允许你进行 事件订阅，事件监听等等操作

- useEffect 有两个参数，第一个为函数，第二个参数为数组 或者不传
- useEffect第一个参数为函数，允许返回一个函数 返回的函数相当于 `componentWillUnmount` 可以做一些销毁工作
- 当useEffect没有第二个参数的时候，相当于实现了`componentDidMount` `componentDidUpdate`
- 当第二个参数为空数组时，相当于 `componentDidMount`
- 第二个参数数组可以依赖一些数据，如state,props，检测到数据的更新才会调用
- 如果在useEffect里面更新state 注意不要死循环 一直更新数据，要检查第二个参数所依赖的数据

**完整例子**
```
function App(){
    const [count, setCount] = React.useState(0)
    React.useEffect(() => {
        console.log('componentDidMount')
        return () => {
            console.log('componentWillUnmount')
        }
    },[])

    React.useEffect(() => {
        console.log('count 更新了')
    },[count])

    React.useEffect(()=>{
        console.log('componentDidUpdate')
    })

    return (
        <div>
            <h1>count的值为:{count}</h1>
            <button onClick={()=>setCount(count+1)}>increment</button>
        </div>
    )
}
```
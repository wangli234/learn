# react-hooks 基本介绍

## 使用规则
1. react 16.8版本后支持
2. hooks 只能在函数式组件中使用 **functional component**
3. hooks 只能在函数组件的顶层中使用，不能在循环遍历中或者多层嵌套函数中等使用
---

## 官方提供的基本hooks

- [useState](#useState)
- useEffect
- useContext
- useCallback
- useRef
- useReducer
- useLayoutEffect
- useMemo
- useImperativeMethods
---

### useState

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
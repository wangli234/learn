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
- [useCallback](#useCallback)
- [useRef](#useRef)
- [useReducer](#useReducer)
- [useLayoutEffect](#useLayoutEffect)
- [useMemo](#useMemo)
- [useImperativeMethods](#useImperativeMethods)

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

**一个简单的例子, 每次点击按钮 `count` 的值都会加1，两个按钮的行为是相同的**
```
function App(){
    const [count, setCount] = React.useState(0)
    return (
        <div>
            <h1>count的值为:{count}</h1>
            <button onClick={()=>setCount(count+1)}>increment</button>
            <button onClick={()=>setCount((prevCount)=>prevCount+1)}>increment2</button>
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

---

### useContext

描述: useContext 是 React.createContext的简单封装，本文不对 `context api` 多做介绍

`context` 本身是为了解决多层级组件数据传递/交互的问题而诞生的 

```
// 简单的例子 可以看出如果传递层次比较深 数据交互会很麻烦 
// 传统方式
function Button({children, theme}){
    return <button theme={theme}>{children}</button>
}

function Page({theme}){
    return (
        <div>
            <Button theme={theme}>按钮</Button>
        </div>
    )
}

class App extends React.Component {
    state = {
        theme: 'dark'
    }
    render() {
        return (
            <Page theme={this.state.theme} />
        )
    }
}

// 使用 react context
const themeContext = React.createContext('dark')

function Button({children}){
    return <themeContext.Consumer>
        {theme => (
            <button theme={theme}>{children}</button>
        )}
    </themeContext.Consumer>
}


function Page(){
    return (
        <div>
            <Button>按钮</Button>
        </div>
    )
}

class App extends React.Component {
    state = {
        theme: 'dark'
    }
    render() {
        return (
            <themeContext.Provider value={this.state.theme}>
                <Page theme={this.state.theme} />
            </themeContext.Provider>
        )
    }
}


// 使用 hooks
使用hooks 的方式 和 context api 差不多  只不过 hooks只能在 函数式组件内部 使用, 免去了context 包装器嵌套的语法

const themeContext = React.createContext('dark')

function Button({children}){
    const theme = React.useContext(themeContext)
    console.log('theme', theme)
    return <button>{children}</button>
}

function Page(){
    return <Button>按钮</Button>
}

function App() {
    return <Page />
}

```

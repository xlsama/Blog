# React中的传参和读参

举个🌰

在 `componentDidMount` 生命周期中创建定时器

## **将定时器中获取值时的代码放到当前文件中**

```jsx
import React, { Component } from 'react'
import './App.css'

class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      count: 0,
    }
  }

  componentDidMount() {
    this.tick()
  }

  tick() {
    setInterval(() => {
      // 这里可以读取到最新的state中的值
      console.log(this.state.count)
    }, 1000)
  }

  add = () => {
    // 让count+1
    this.setState(state => ({ count: state.count + 1 }))
  }

  render() {
    return (
      <div>
        <button onClick={this.add} className="mybutton">
          添加
        </button>
      </div>
    )
  }
}

export default App

```

![image-20210402110303877](https://i.loli.net/2021/05/12/oKVyZr4zBCMJ3f6.png)

## **将定时器中获取值时的代码放到其他文件中**

```js
// tick.js
export default function tick(count) {
  setInterval(() => {
    // 拿到的永远是第一次传入的值
    console.log(count)
  }, 1000)
}

```

```jsx
import React, { Component } from 'react'
import './App.css'
import tick from './tick'

class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      count: 0,
    }
  }

  componentDidMount() {
    // this.tick()
    
    // 传入count
    tick(this.state.count)
  }

  // tick() {
  //   setInterval(() => {
  //     console.log(this.state.count)
  //   }, 1000)
  // }

  add = () => {
    this.setState(state => ({ count: state.count + 1 }))
  }

  render() {
    return (
      <div>
        <button onClick={this.add} className="mybutton">
          添加
        </button>
      </div>
    )
  }
}

export default App

```

![image-20210402111952747](https://i.loli.net/2021/05/12/RBrPvw3zFM9AT7x.png)

![image-20210402112019799](https://i.loli.net/2021/05/12/UsE2dgb1eWTILNn.png)

总结: `componentDidMount` 中拿到的是初始化时的值，如果想要在组件挂载以后执行的函数中获取state或者props中的值，则必须重新从props或者state中去拿，不能用之前赋值的变量。
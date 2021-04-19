---
title: react通信
date: 2021-04-19
tags:
 - react
categories:
 - react
---

react组件间的通信跟vue有些相似性，却更加灵活。下面说说它们各级是如何实现通信的吧！

> 父组件向子组件通信

```jsx
import React, { Component } from 'react'

class Child extends Component {
    constructor(props) {
        super(props);
        console.log(this)
    }
    render() {
        // 通过对象的解构赋值，得到传过来的name和age
        const {name,age} = this.props;
        return (
            <div>
            	//使用得到的name,age
                <h2 style={{color:'green'}}>{name}, {age}</h2>
            </div>
        );
    }
}

export default class ParentChild extends Component {
    render() {
        return (
            <div>
            	/**
            	* 在子组件标签中添加属性，属性会被加到子组件中的this.props属性上，
            	* 这样子组件就可以使用父组件传递过来的值
            	*/
                <Child name = "XQ" age = {18}/>
            </div>
        )
    }
}
```



> 子组件向父组件通信

```jsx
import React, { Component } from 'react'

class BtnClick extends Component {
    render() {
        console.log(this)
        // 父组件传过来一个对所需修改变量进行修改的函数，子组件中接收这个函数
        const {add} = this.props;
        return (
            <div>
                //点击一下调用函数，在子组件中改变父组件state中的值
                <button onClick = {add}>+1</button>
            </div>
        );
    }
}

export default class ChildtoParent extends Component {
    constructor() {
        super();
        this.state = {
            counter:0
        }
    }
    render() {
        const {counter} = this.state;
        return (
            <div>
               <h2>当前计数:{counter}</h2> 
               <BtnClick add = {e=>this.add()}></BtnClick>
            </div>
        )
    }

    add() {
        console.log('11')
        this.setState({
            counter:this.state.counter+1
        })
    }
}
```




---
title: hello react！
date: 2021-04-12
tags:
 - react
categories:
 - react
---

Hello React!

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app"></div>

    <script src="https://unpkg.com/react@17/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js" crossorigin></script>
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

    <script type="text/babel">
        class App extends React.Component {
            constructor(){
                super();
                console.log(this)
                this.state = {msg:'hello vue'}
            }

            render() {
                return (
                    <div>
                        <h2>{this.state.msg}</h2>
                        <button onClick = {this.btnClick}>点击按钮变框架</button>
                    </div>
                )
            }

            btnClick = ()=> {
                this.setState({
                    msg:'hello react'
                })
            }

        }

        ReactDOM.render(<App/>,document.getElementById('app'));
    </script>
</body>
</html>
```


---
title: react样式写法
date: 2021-04-23
tags:
 - react
categories:
 - react
---

react中的样式一直是react的一个不太友好的地方，由于形式的不统一，大大小小出现了许多种样式的表达方式，下面主要说下面四种。

> 内联样式

在react中，我们通过`{}`来表示js表达式，在元素标签内部，使用对象来表示元素样式。具体表示如下：

```jsx
import React, { PureComponent } from 'react'

export default class Profile extends PureComponent {
    render() {
        return (
            <div style = {{color:"red"}}>
                profile
            </div>
        )
    }
}

```

> 外部样式

就跟我们平常写代码一样，写一个样式，通过 `import 文件路径` 来引入，但是由于react写的是单页面应用，又分成了各种组件，这样写很容易在没有发觉时就重用了类名，使用起来并不方便。

> css-modules

为了避免上面的问题，引入了css-modules，也就是把css模块化。在react中，webpac已经帮我们将module设置成了true，也就是说，我们可以直接使用这种引入样式的方式。只是css文件的后缀名要为`.module.css` 具体例子如下 :

```jsx
//在Profile组件中
import React, { PureComponent } from 'react'
import profileStyle from './style.module.css'

export default class Profile extends PureComponent {
    render() {
        return (
            <div>
                <h2 className = {profileStyle.htitle}>我是profile页面</h2>
            </div>
        )
    }
}


//在同级中的style.module.css中
.htitle{
    color: red;
}
```

> css in js

使用`styled-components`库，将css样式包装成一个组件，再利用组件包裹相应需要加样式的内容，就可以改变样式。使用这种方式，可以动态加载样式。

首先要安装库：`yarn add styled-components`

[styled-components官方文档](https://styled-components.com/docs/basics)

具体例子如下：

```jsx
//Home组件
import React, { PureComponent } from 'react'
import {HomeWappered} from './style'

export default class Home extends PureComponent {
    render() {
        return (
            <HomeWappered>
                <h2>打印测试</h2>
            </HomeWappered>
        )
    }
}

//同级style.js
import Styled from 'styled-components'

export const HomeWappered = Styled.div`
    color:red;
`
```


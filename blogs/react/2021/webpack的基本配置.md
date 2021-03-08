---
title: jsx书写规范
date: 2021-03-08
tags:
 - react
categories:
 - react
---

作为一名前端工程师，掌握`webpack`是必须的，因为现在都是模块化编程，有一个打包工具是十分重要的，但`webpack`的配置我总是忘记，故在这里记录一下它的具体用法。

下面，我将从零开始，一步步的记录`webpack`的用法。

> `webpack`安装

`webpack`现在已经不推荐全局安装，这是一个开发时依赖，使用以下命令安装webpack 以及webpack-cli

```js
npm install webpack webpack-cli -D 
```



> `webpack`配置

`webpack`的配置需写在`webpack.config.js`文件中，在根目录中创建这样一个文件即可。

`webpack`使用`CommonJS`规范，主要有五个配置项，基本形式如下

```js
const path = require('path');

module.exports = {
    //入口起点，表示webpack从这里开始打包
    entry:'',
    
    //打包文件的输出路径
    output:{
        //输出的文件名
        filename:'',
        //输出的文件所在的路径
        path:path.resolve(__dirname,'dist')
    },
    
    //loader的配置
    module:{
        //详细的loader配置
        rules:[
            {
                test:/匹配的正则/,
                //当使用多个loader时，要使用的loader放在数组中
                //！！！注意顺序，从右往左，下面具体会提到
                use:[]
            },
            {
                test://,
                //当使用一个loader时，只用loader属性即可
                loader:''，
            }
            
        ]
    },
    
    //插件的配置
    plugins:[
    	//插件的详细配置
    ],
    
    //模式，分为两种模式
    //1.开发者模式   参数为development
    //2.生产者模式   参数为production
    mode:'development'
    //mode:'production'
}
```



> 打包样式资源

在我们使用ES6模块化时，通过import引入css样式是不起作用的，这个时候，就要使用神奇的`webpack`了，它的引入主要添加两个loader，配置具体代码如下：

```js
 module:{
        rules:[
            {
                //只设置了css的文件，如果需要less文件，再写一个匹配less文件的对象即可
                //但是需要在最右边加上less-loader,其他的文件也一样
                test:/\.css$/,
                //从右往左顺序执行
                //css-loader编程CommonJs模块加载到js中，里面的内容是样式字符串
                //style-loader 创建style标签，将js资源插入进行，添加到head中生效
                use:['style-loader','css-loader']
            }
            
        ]
 }
```



> 打包html资源

```js
//在这里需要使用插件plugin，plugin与loader的使用方式不同
//loader： 1.npm / yarn下载包  2.使用
//plugin：1.下载包 2.引入 3.使用


//引入！！！
const htmlWebpackPlugin  = require('html-webpack-plugin');

...
plugin:[
    //自动在dist（打包文件夹）中创建一个空的html，引入打包的所有资源
    new htmlWebpackPlugin()
    
    /*
    如果需要有结构的html文件，那需要使用template属性
    new htmlWebpackPlugin({
    	//需要src文件下的index.html文件
    	template:'./src/index.html'
    }) 
    
    */
]
```



> 打包图片文件

```js
 module:{
        rules:[
            {
                //问题：默认处理不了html中的图片，因为打包后图片名称改变
                //直接引入html中的图片名字与打包后的文件不符
                
                
                test:/\.(jpg|png|gif)$/,
                //需要下载url-loader  file-loader
                loader:'url-loader',
                options:{
                    //图片大小小于8kb,就会被base64处理
                    //优点：减少请求数量（减轻服务器压力）
                    //缺点：图片体积更大（文件请求速度变慢）
                    limit:8 * 1024,
                    
                    //问题：因为url-loader使用es6模块化解析，而html-loader使用CommonJS解析
                    //出现[object Moudle]
                    //解决：关闭url-loader es6解析
                    esModule:false,
                    
                    //给图片重命名，文件名太长占内存
                    //[hash:10]取图片hash的前十位
                    //[ext]：文件原有的拓展名
                    name:'[hash:10].[ext]'
                    
                }
            },
            {
                //解决问题：默认处理不了html中的图片，因为打包后图片名称改变
                //直接引入html中的图片名字与打包后的文件不符
                test:/\.html$/,
                //处理html的img图片
                loader:'html-loader'，
                
            }
            	
            
        ]
 }
```



> 打包其他资源

```js
module:{
        rules:[
            {
                //排除css,js,html文件，如果要排除其他类型的文件，也可以加
                exclude:/\.(css|js|html)$/,
                loader:'file-loader'
            }
            
        ]
 }
```



到这儿，`webpack`的基本配置就写完了！！！看了看时间，还写了蛮久的，希望以后温故知新，加油加油！
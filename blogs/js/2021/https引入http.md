---
title: 在https页面引入http资源的问题
date: 2021-05-14
tags:
 - js
categories:
 - js
---

浏览器默认是不允许在https里面调用http资源的。

### 为什么在https页面发送不了http请求？

有人说是跨域问题，真的是这么咩？

说起跨域问题就要知道浏览器的同源策略：

> 同源策略 1、协议相同；  2、域名相同； 3、端口相同

尽管HTTPS访问HTTP确实不符合同源策略中的协议相同，但是在现代浏览器里，即使是域名相同的请求，也是会出现Mixed Content报错，而不是跨域报错。

这也很好理解，毕竟混合内容的安全策略是在浏览器端断定的，而是否能跨域要看服务器返回的Response头，请求都被浏览器block掉了，也就不存在是否跨域的问题。

#### 那什么是混合内容？

> **混合内容：初始 HTML 内容经过安全的 HTTPS 链接加载，但其余资源（例如，图像、视频、样式表、脚本）则经过不安全的 HTTP 链接加载。由于页面经过 HTTPS 加载的初始请求是安全的，可是又加载了不安全的HTTP内容，所以称之为混合内容。**

## 解决方式

### 相对协议

如果你的网站同时准备了 https 资源和 http 资源，那么，可以使用相对协议可以帮助你实现当网站引入的都是 http 资源，网站域名更换为 https 后的无缝切换。

具体使用方法为：

```html
<img src="//domain.com/img/logo.png">
```

简而言之，就是将URL的协议（http、https）去掉，只保留//及后面的内容。这样，在使用https的网站中，浏览器会通过https请求URL，否则就通过http发送请求。

> 附注：如果是浏览本地文件，浏览器通过[file://协议发送请求，导致请求失败，因此本地测试最好是搭建一个本地服务器。](https://www.oschina.net/action/GoToLink?url=file%3A%2F%2F协议发送请求，导致请求失败，因此本地测试最好是搭建一个本地服务器。)



[HTML5 Boilerplate](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fhtml5boilerplate.com%2F) 使用相对协议请求 Google CDN 中的 jQuery ，使用方式为：

```html
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.js"></script>
<script>!window.jQuery && document.write(unescape('%3Cscript src="js/libs/jquery-1.4.2.js"%3E%3C/script%3E'))</script>
```

上面的例子中除了引用 Google CDN 中的文件外，还添加了一个本地 jQuery 链接，以便连接 Google CDN 失败后，使用本地副本。代码判断过程为：

1. 首先检查 jquery 对象是否存在，如果存在，证明 Google CDN 运行正常；
2. 如果不存在，则说明连接 Google CDN 失败，引入本地 jQuery 库。

> 在国内相对特殊的网络环境中，Google CDN 请求缓慢甚至失败常有，因此不推荐引用 Google CDN 托管的各种库。即便有备用链接，浏览器还是会多次尝试请求 Google CDN 中的文件，这个过程可谓漫长，严重影响页面载入速度。

### 使用 iframe

使用 iframe 的方式引入 http 资源，比如在 https 里面播放优酷的视频，我们可以先在一个 http 的页面里播放优酷视频，然后将这个页面嵌入到 https 页面里就可以了。

另外一个典型的例子是在 https 页面里通过 Ajax 的方式请求 http 资源，Chrome 是不允许直接 Ajax 请求 http 的。如果两个页面的内容都可以控制的话，当前窗口可以 iframe 窗口进行通信。

```html
<div class="video_content">
    <iframe height=148 width=220 src="http://player.youku.com/embed/XMTU4MTY4OTg5Mg==" frameborder=0 allowfullscreen></iframe>
</div>
```

## 其他用法

这个小技巧同样适用于 CSS ：

```css
.omg { background: url(//websbestgifs.net/kittyonadolphin.gif); }
```

> 附注：<link>或[@import](https://my.oschina.net/u/3201731)引入样式表时使用相对协议，IE7、IE8 会下载文件两次。

## 参考文献

[HTTPS的页面发送不了HTTP请求？——关于混合内容](https://www.shangmayuan.com/a/571dff5ac01a43a989e081bd.html)

[What is mixed content?](https://web.dev/what-is-mixed-content/#_11)

[https 页面中引入 http 资源的解决方式](https://qdmana.com/2021/03/20210314164143126w.html)


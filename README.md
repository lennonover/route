# route

## 前端路由

前端路由有2种实现方式，一种是html5推出的historyapi，另一种hash路由，就是常见的 # 号，这种方式兼容性更好。

## History API

两个新增的API history.pushState 和 history.replaceState

API 都接收三个参数，分别是

- 状态对象（state object） — 一个JavaScript对象，与用pushState()方法创建的新历史记录条目关联。无论何时用户导航到新创建的状态，popstate事件都会被触发，并且事件对象的state属性都包含历史记录条目的状态对象的拷贝。
- 标题（title） — FireFox浏览器目前会忽略该参数，虽然以后可能会用上。考虑到未来可能会对该方法进行修改，传一个空字符串会比较安全。或者，你也可以传入一个简短的标题，标明将要进入的状态。
- 地址（URL） — 新的历史记录条目的地址。浏览器不会在调用pushState()方法后加载该地址，但之后，可能会试图加载，例如用户重启浏览器。新的URL不一定是绝对路径；如果是相对路径，它将以当前URL为基准；传入的URL与当前URL应该是同源的，否则，pushState()会抛出异常。该参数是可选的；不指定的话则为文档当前URL。

相同之处是两个 API 都会操作浏览器的历史记录，而不会引起页面的刷新。

不同之处在于，pushState会增加一条新的历史记录，而replaceState则会替换当前的历史记录。

>注意:这里的 url 不支持跨域，当我们把 www.baidu.com 换成 baidu.com 时就会报错。

## hash

也是本文用到的。我们经常在 url 中看到 #，这个 # 有两种情况，一个是我们所谓的锚点，比如典型的回到顶部按钮原理、Github 上各个标题之间的跳转等，路由里的 # 不叫锚点，我们称之为 hash，大型框架的路由系统大多都是哈希实现的。

同样我们需要一个根据监听哈希变化触发的事件 —— hashchange 事件

我们用 window.location 处理哈希的改变时不会重新渲染页面，而是当作新页面加到历史记录中，这样我们跳转页面就可以在 hashchange 事件中注册 ajax 从而改变页面内容。

## 功能

- 切换页面

  通过 window 的监听事件 hashchange 来监听hash的变化，然后捕获到具体的hash值进行操作

- 异步加载js

- 异步传参

  在我们动态引入单独模块的js之后，我们可能需要给这个模块传递一些单独的参数。这里借鉴了一下jsonp的处理方式，我们把单独模块的js包装成一个函数，提供一个全局的回调方法，加载完成时候再调用回调函数。

  ```javascript
  SPA_RESOLVE_INIT = function(transition) {
      document.getElementById("content").innerHTML = '<p style="color:#F8C545;">当前异步渲染列表页'+ JSON.stringify(transition) +'</p>'
      console.log("首页回调" + JSON.stringify(transition))
  }
  ```


## 使用方法

- 注册路由
```javascript
 spaRouters.map('/name',function(transition){
		//异步加载js
		spaRouters.asyncFun('name.js',transition)
		//或者同步执行回调
		spaRouters.syncFun(function(transition){},transition)
	})
```
- 初始化     
```javascript
 spaRouters.init()
```

- 跳转

@

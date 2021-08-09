  

##  一、计算机网络相关

  #### 1-1 介绍一下websocket以及它的应用
  1. websocket是一种建立在TCP协议上的全双工通信协议，它基于HTTP协议，但与HTTP协议只能由客户端发起request，服务端返回response的模式不同，websocket中，客户端和服务器任何一方都可以随时将数据推送至另一方。
  2. WebSocket和Http协议一样都属于应用层的协议，它们的底层都是基于TCP协议的。
  3. websocket可以应用的场景有：社交订阅、体育实况更新、股票基金报价、多媒体聊天等



  #### 1-2 介绍一下HTTP和HTTPS
  HTTP和HTTPS都是为实现网络通信而产生的网络协议，它的中文全称为`超文本传输协议`。
  HTTP协议一般运用于B/S架构，它基于TCP/IP协议簇来传递各种类型的数据，包括HTML、图片、查询结果等。

  > 一般http中存在如下问题：
    - 请求信息明文传输，容易被窃听截取。
    - 数据的完整性未校验，容易被篡改
    - 没有验证对方身份，存在冒充危险

为了解决以上问题，出现了HTTPS。

HTTPS 协议（HyperText Transfer Protocol over Secure Socket Layer）：一般理解为HTTP+SSL/TLS，通过 SSL证书来验证服务器的身份，并为浏览器和服务器之间的通信进行加密。其中SSL(安全套接字层)位于 TCP/IP 协议与各种应用层协议之间，为数据通讯提供安全支持。

浏览器使用HTTPS的流程：

1. 客户端通过URL访问服务器建立SSL连接。
2. 服务端收到客户端请求后，会将网站支持的证书信息（证书中包含公钥）传送一份给客户端。
3. 客户端的服务器开始协商SSL连接的安全等级，也就是信息加密的等级。
4. 客户端的浏览器根据双方同意的安全等级，建立会话密钥，然后利用网站的公钥将会话密钥加密，并传送给网站。
5. 服务器利用自己的私钥解密出会话密钥。
6. 服务器利用会话密钥加密与客户端之间的通信。



#### 1-3 强缓存和协商缓存的区别

​	首先讲一下什么是浏览器缓存：一种保存浏览器请求的资源副本，并在下次请求时直接使用该副本的技术。

​	强缓存和协商缓存，是两种不同的浏览器缓存方案。

​	强缓存：直接在响应头中指定该资源的过期时间，典型实现方案有：expires和cache-control：max-age两个

​					其中expires规定了一个资源过期的绝对时间，超过了这个时间，浏览器则必须向服务区重新请求资源。

​					由于expires响应头的绝对时间的设置是在服务器端，而生效是在浏览器端，如果浏览器端人为修改了时间，则会导致缓存

​					时间判断出错。由此，http/1.1引入了cache-control:max-age这个响应头。该字段规定的是一个以秒为单位的时间段，超过

​					了这个时间段，则资源过期。

协商缓存：由于强缓存中存在无法判断服务端资源是否在过期时间内被修改的情况，协商缓存应运而生。其主要思想，是对获取的资源					做标识，再下次请求资源时带上这个标识，由服务端判断资源是否被修改(修改了则返回200，未修改则返回304not 					modified)。

​					协商缓存一般通过http请求和响应的两对标识实现，他们分别是：last-modified和if-modified-since、e-tag和if-none-match

##### 【last-modified和if-modified-since】的原理：

		1. 客户端首次请求资源，服务区返回资源，并在响应头带上last-modified标识，表示该资源上一次修改的时间
		2. 客户端第二次请求该资源，则会在请求头带上if-modified-since字段，其值则为上一次响应中last-modified字段的值
		3. 服务区接收第二次请求，利用if-modified-since字段的值，和服务器上资源的最后修改时间做比较，如果一致，则表示资源没有变化，返回304，若不一致，则表示资源有了变化，返回200和新资源
		4. 浏览器若收到304，则会从缓存中加载资源，若收到200则正常加载新资源



##### 【E-tag和if-none-match】的原理

#### 1-4 304状态码的意思，和302的区别

​	304 状态码的意思是not modified(资源未修改)，在浏览器缓存机制中用来通知客户端使用缓存来加载数据，而不是直接接收服务端数据。

​	302状态码的意思是临时重定向，表示请求的资源的url被临时更改到了另一个地址，服务区返回302状态码时，会同时返回一个location字段，用来描述重定向以后的地址。

#### 1-5 HTTP/2、HTTP/3分别做了怎样的设计调整



#### 1-6 一个完整的HTTP请求过程

##### 整个流程

域名解析 —> 与服务器建立连接 —> 发起HTTP请求 —> 服务器响应HTTP请求，浏览器得到html代码 —> 浏览器解析html代码，并请求html代码中的资源（如js、css、图片） —> 浏览器对页面进行渲染呈现给用户

#### 1-7 说一下DNS域名解析的详细规则

1. 首先，浏览器会去自身的DNS缓存中查找，没有则进行下一步
2. 然后，浏览器去本地操作系统的DNS缓存中查找，没有则下一步
3. 第三步，去本地的host文件里查找，没有则下一步
4. 第四步，请求本地域名服务器(可以理解为网络运营商服务器)请求，没有则开始向根域名进行迭代请求
5. 从根域名开始迭代请求，获取顶级域名的服务器IP地址
6. 请求顶级域名服务器的IP地址，获得该域名下的二级域名的IP地址
7. 再请求二级域名的IP地址，获得二级域名下的三级域名Ip地址，以此类推，找到最终的IP地址

  #### 1-8 介绍一下什么是CDN，和它的使用场景

  CDN全称Content Delivery Network,中文译为`内容发布网络`

#### 1-9  解释一下TCP三次握手

TCP 三次握手的意义：client和server双方都能够确定能够向对方传送、接收信息。

第一次握手：client向server发送syn包，server由此确定client的发送能力正常，自己的接收能力正常

第二次握手：server向client返回syn包+ack包，client由此确定server的接收、发送能力正常，自己的发送、接收能力正常

第三次握手：client向server发送ack包，server由此确定client的接收能力正常，自己的发送能力正常



#### 1-10 讲一下cookie



##  二、HTML&CSS

#### 2-1 聊一下浏览器标准模式和怪异模式（盒模型）

首先解释一下什么是盒模型：网页开发中，CSS在描述一个块的样式时，所遵循的一种模式

盒模型中，一个块由content,padding,border,margin组成。

盒模型又分为 W3C盒模型（标准模式）和 IE盒模型（怪异模式）两种。

其中，标准盒模型的计算公式为：元素的宽高 = 盒模型中content的宽高
     怪异盒模型的计算公式为：元素的宽高 = 盒模型中content的宽高 + padding + border
这样一来，标准盒模型中 盒子的大小 = 元素宽高 + padding + border + margin
         怪异盒模型中 盒子的大小 = 元素宽高 + margin
	 
触发怪异盒模型的方法：

1. 当我们不给HTML指定docType时，部分IE浏览器会自动触发怪异盒模型
2. CSS中有个属性box-sizing,指定为border-box时浏览器表现为怪异盒模型，指定为content-box时表现为标准盒模型

怪异盒模型的应用场景： 当子元素有固定的padding或border，而其宽度需要保持为一个百分比时


#### 2-2 画一条 0.5px 的线

##### 方法一： 使用transform:scale()

```html
<div class='half-px'></div>  
<style>
  .half-px{
    width:200px;
    height:1px;
    background-color:#000;
    transform:scale(0.5)
  }
</style>
```
##### 方法二：使用meta标签指定initial-scale
```html
  <meta name='viewport'  content="width=device-width,initial-scale=0.5">
```

##### 方法三：使用canvas指定lineWidth为0.5

```html
<canvas id='drawing'></canvas>
<script>
  let drawing = document.getElementById('drawing');
  let context = drawing.getContext('2d')
  context.lineWidth = 0.5
  context.beginPath();
  context.moveTo(10,10);
  context.lineTo(10,60);
  context.stroke();
</script>
  
```

##### 方法四：使用svg指定stroke-width为0.5
```html
<svg xmlns="http://www.w3.org/2000/svg" width="200px" height="200px">
  <line x1='0' y1='0' x2='100' y2='100' style="stroke:#f00;stroke-width:0.5"></line>
</svg>
```



#### 2-3 介绍下 BFC 及其应用

BFC即块级格式上下文，描述的是DOM里一块隔离了的独立容器，容器内的元素无论如何翻江倒海，都不会影响容器外的元素的布局。

BFC常见的特性：

1. BFC使得文档流中的两个元素的margin重叠
2. BFC下的容器能够包裹浮动元素（清除浮动）
3. BFC能够阻止元素被浮动元素覆盖

触发BFC的条件：

1. body根元素
2. 浮动元素，设置了float除none之外的属性的元素
3. 绝对定位元素：position:absolute/fixed
4. 设置了overflow除visible之外的属性的元素
5. display为inline-block、table-cell和flex的元素

#### 2-4 浏览器的渲染原理

#### 2-5 介绍下重绘和回流（Repaint & Reflow），以及如何进行优化？

#### 2-6 简述伪类和伪元素，CSS3新增了哪些伪类？

#### 2-7 分析比较 opacity: 0、visibility: hidden、display: none 优劣和适用场景







## 三、Javascript基础

### 3-1 作用域与闭包、this与执行上下文



### 3-2 原型与继承

##### 3-2-1 ES5/ES6 的继承除了写法以外还有什么区别？



### 3-3 单线程、异步与事件机制

##### 3-3-1 JavaScript为什么引入异步？它有哪些使用场景？

##### 3-3-2 setTimeout、Promise、Async/Await 的区别





### 3-4 promise、async/await、generator函数

##### 3-4-1 简述一下 Generator 函数

##### 3-4-2 Async/Await 如何通过同步的方式实现异步



### 3-5 JS-web-API

##### 3-5-1 DOM是哪种数据结构

DOM本质是一种树结构

##### cookie、localStorage和sessionStorage的区别

1. 从容量上看：cookie只有4kb大小，而localStorage和sessionStorage可以存储5M
2. 从API易用性上看：cookie的API比较简陋，存取cookie不方便，而localStorage和sessionStorage API比较完善
3. 从设计初衷上看：cookie本身是为了解决服务端和浏览器通信问题而生的，它会随着http请求发送给后端，而localStorage和sessionStorage不会发送给后端，它们的出现是为了实现浏览器存储。

### 3-6 JavaScript中的常用API

##### 3-6-1 class关键字定义类和普通构造函数定义类的区别

1. class不像构造函数，没有变量提升
2. class中挂载在原型上的方法是不可枚举的，而构造函数的prototype上添加的方法是可枚举的
3. 构造函数可以不使用new关键字，作为普通函数使用，而class定义的类必须加上new，作为构造器使用
4. class在继承中有两条原型链：子类的proto指向父类，子类的prototype的proto指向父类的prototype
5. ES5中的原生构造函数无法实现继承，ES6中使用class语法可以实现继承

##### 3-6-2 如何实现浏览器内多个标签页之间的通信？



##### 3-6-3 说说for，for-in ，for-of和forEach的区别



##### 3-6-4 介绍下 Set、Map、WeakSet 和 WeakMap 的区别？



##### 3-6-5 聊聊Blob和ArrayBuffer









## 四、设计模式

#### 4-1 什么是设计模式？设计模式如何解决复杂问题？



#### 4-2 介绍下观察者模式和订阅-发布模式的区别，各自适用于什么场景

观察者模式中，发布者和观察者之间彼此知道对方的存在

发布-订阅模式中，发布者和订阅者是相互隔离的，通过事件中心这个中介来进行通信

## 五、性能优化

#### 5-1 什么是防抖和节流？有什么区别？如何实现？

防抖和节流的使用场景，都是在事件的回调函数被频繁触发时，为了节约性能，使其延缓执行的一种手段。

防抖的概念，是指触发高频事件后，回调函数在n秒内只执行一次，如果n秒内事件触发了多次，则重新计算时间

节流的概念，是指高频事件触发后，每隔n秒种，只执行一次函数。（这种手段稀释了函数的执行周期）

实现：

```javascript
function debounce(fn,delay){
  let timer;
  return function(){
    let _this = this
    let args = arguments
    if(timer){
      clearTimeout(timer)
    }
    timer = setTimeout(()=>{
      fn.apply(_this,args)
    },delay)
  }
}

function test(){
  console.log('test')
}
let debounceFn = debounce(test,5000)
someBtn.onclick=function(){
  debounceFn()
}
```

防抖的使用场景：当在频繁操作后只需在最后执行回调操作(输入框监听内容变化、窗口大小resize)

节流的使用场景：隔一段时间需要执行一次回调操作（如滚动加载，高频点击按钮）

#### 5-2 页面的可用性时间的计算

#### 5-3 简述懒加载，懒加载和预加载的区别？

#### 5-4 谈谈DNS解析优化中的方法

1. 减少DNS查找



## 六、网络安全

#### 6-1 什么是 CSRF 攻击？如何防范 CSRF 攻击？

CSRF攻击原意为跨站请求伪造(cross site request forgery),当用户登录正常网站A后，A网站会向用户的浏览器发送一个cookie用以保存用户信息，如果此时，用户又登录了黑客提供的B网站，而B网站在用户不知情的情况下向A网站发送了请求，此时这个请求会携带上A网站的cookie，让A网站的服务器以为是用户自发的请求，从而达到非法目的。

防范CSRF攻击的方法：

1. 验证请求头中的refer字段，若refer与当前请求的站点不相同，则被怀疑为CSRF攻击
2. 使用token来进行身份验证
3. 给cookie设置samesite属性，只有请求的URL和当前站点一致时，cookie才被发送

#### 6-2 什么是XSS攻击？简述XSS的三种类型

XSS攻击又名跨站脚本攻击(Cross site scripting)，主要攻击方式为向页面注入恶意攻击代码，当用户浏览该页面时，内部的恶意代码会执行从而达到攻击的目的。

当页面被注入了恶意脚本时，它可以实现：

- 获取页面cookie
- 监听用户行为
- 更改DOM结构、在页面内生成广告或表单
- 劫持流量实现恶意跳转

XSS有三种类型：

1. 存储型XSS，黑客通过某种手段将恶意脚本保存至站点数据库，当用户访问站点时，服务器将恶意脚本和正常页面一起返回，浏览器执行了恶意脚本，从而实现攻击
2. 反射型XSS，黑客通过在URL参数中注入恶意脚本，诱导用户点击这个URL，浏览器读取URL，并执行了其中的恶意脚本，从而实现攻击
3. DOM型XSS，

#### 6-3 什么是SQL注入攻击？

SQL注入是一种常见的数据库攻击手段，它的实现方式，是黑客在表单中提交一段恶意的sql语句到服务器，服务器执行了恶意sql,达到了攻击的目的。



#### 6-4 什么是DDOS攻击

```txt
DOS攻击又名服务拒绝攻击(Denial of service),其原理是发送大量合法请求到服务器，使服务器进入无法响应的状态
```

而DDOS攻击则是DOS攻击的升级版，全名为Distribute Denial of Service(分布式服务拒绝攻击)，攻击者借由公共网络，将大量计算机设备联合起来进行攻击





## 七、框架底层原理

#### 7-1 Vue的生命周期？mounted和created的区别？

Vue的生命周期有beforeCreate,created,beforeMount,mounted,beforeUpdate,updated,beforeDestroy,destroyed

其中created是在vue实例构建完成后调用，mounted是在该vue实例的数据挂在到dom上以后调用

#### 7-2 父子组件的生命周期调用顺序？

1. 加载渲染过程 `父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted`
2. 组件更新过程 `父beforeUpdate->子beforeUpdate->子updated->父updated`
3. 销毁过程 `父beforeDestroy->子beforeDestroy->子destroyed->父destroyed`



#### 如何理解MVVM

MVVM是一种架构模式，其中M代表model数据，在vue中就是data数据；V代表view视图，vue中即DOM元素，vm代表viewModel，  vue中就是vue的一个实例，它控制着model数据和view视图之间的数据变动，当view发送改变时，vm实例中的domListener监听并响应对应事件，使数据model发生对应改变，而model的变化，则通过directive指令表达出来，在视图中呈现。



#### vue中的v-for为何要使用key



#### 说一下vue的响应式原理

vue的响应式是通过Object.defineProperty和观察者模式来实现的。vue会深度遍历传入的data，对每个data的属性调用Object.defineProperty，在其中设置setter和getter来劫持数据，在getter中收集依赖，将依赖该数据的观察者添加至发布者的subs数组中，在setter中触发依赖，通知依赖该数据的所有观察者去更新视图。

#### Vue如何监听数组的变化

vue会改写数组数据的原型proto为一个新对象，这个新对象的原型是Array.prototype,在这个新对象上重写了数组的push\pop\splice等方法，在触发这些方法后，首先去通知视图更新，然后再调用老原型上的对应方法更新数据。

#### 描述组件渲染和更新的过程？



#### 双向数据绑定v-model的实现原理





## 八、 前端工程化

#### 8-1 module、chunk和bundle分别是什么，有何区别？

#### 8-2 loader和plugin的区别？

#### 8-3 webpack如何实现懒加载？

#### 8-4 webpack常见性能优化

#### 8-5 babel-runtime和babel-polyfill的区别





## 主观题

#### 聊一下对前端行业对认识

前端行业至少两次技术跨越，第一次是在1998年AJax技术出现以后，前端从纯静态网页发展为富交互的动态页面，第二次是在2009年nodejs的出现，

 


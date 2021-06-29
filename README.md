  

##  一、计算机网络相关

  #### 1-1 介绍一下websocket以及它的应用
  1. websocket是一种建立在TCP协议上的全双工通信协议，它基于HTTP协议，但与HTTP协议只能由客户端发起request，服务端返回response的模式不同，websocket中，客户端和服务器任何一方都可以随时将数据推送至另一方。
  2. WebSocket和Http协议一样都属于应用层的协议，它们的底层都是基于TCP协议的，调用的WebSocket的send函数在实现中最终都是通过TCP的`系统接口`进行传输的。
  3. websocket可以应用的场景有：社交订阅、体育实况更新、股票基金报价、多媒体聊天等

  #### 1-2 介绍一下什么是CDN，和它的使用场景
  CDN全称Content Delivery Network,中文译为`内容发布网络`

  #### 1-3 介绍一下HTTP和HTTPS
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



#### 1-4 强缓存和协商缓存的区别

​	首先讲一下什么是浏览器缓存：一种保存浏览器请求的资源副本，并在下次请求时直接使用该副本的技术。

​	强缓存和协商缓存，是两种不同的浏览器缓存方案。

​	强缓存：直接在响应头中指定该资源的过期时间，典型实现方案有：expires和cache-control：max-age两个

​					其中expires规定了一个资源过期的绝对时间，超过了这个时间，浏览器则必须向服务区重新请求资源。

​					由于expires响应头的绝对时间的设置是在服务器端，而生效是在浏览器端，如果浏览器端人为修改了时间，则会导致缓存

​					时间判断出错。由此，http/1.1引入了cache-control:max-age这个响应头。该字段规定的是一个以秒为单位的时间段，超过

​					了这个时间段，则资源过期。

协商缓存：由于强缓存中存在无法判断服务区端资源是否在过期时间内被修改的情况，协商缓存应运而生。其主要思想，是对获取的资源					做标识，再下次请求资源时带上这个标识，由服务区判断资源是否被修改(修改了则返回200，未修改则返回304not 					modified)。

​					协商缓存一般通过http请求和响应的两对标识实现，他们分别是：last-modified和if-modified-since、e-tag和if-none-match

##### 【last-modified和if-modified-since】的原理：

		1. 客户端首次请求资源，服务区返回资源，并在响应头带上last-modified标识，表示该资源上一次修改的时间
		2. 客户端第二次请求该资源，则会在请求头带上if-modified-since字段，其值则为上一次响应中last-modified字段的值
		3. 服务区接收第二次请求，利用if-modified-since字段的值，和服务器上资源的最后修改时间做比较，如果一致，则表示资源没有变化，返回304，若不一致，则表示资源有了变化，返回200和新资源
		4. 浏览器若收到304，则会从缓存中加载资源，若收到200则正常加载新资源



##### 【E-tag和if-none-match】的原理

#### 1-5 304状态码的意思，和302的区别

​	304 状态码的意思是not modified(资源未修改)，在浏览器缓存机制中用来通知客户端使用缓存来加载数据，而不是直接接收服务端数据。

​	302状态码的意思是临时重定向，表示请求的资源的url被临时更改到了另一个地址，服务区返回302状态码时，会同时返回一个location字段，用来描述重定向以后的地址。

#### 1-6 HTTP/2、HTTP/3分别做了怎样的设计调整



#### 1-7 一个完整的HTTP请求过程

##### 整个流程

域名解析 —> 与服务器建立连接 —> 发起HTTP请求 —> 服务器响应HTTP请求，浏览器得到html代码 —> 浏览器解析html代码，并请求html代码中的资源（如js、css、图片） —> 浏览器对页面进行渲染呈现给用户

#### 1-8 说一下域名解析的详细规则

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

#### 2-4 浏览器的渲染原理

#### 2-5 介绍下重绘和回流（Repaint & Reflow），以及如何进行优化？

#### 2-6 简述伪类和伪元素，CSS3新增了哪些伪类？

#### 2-7 分析比较 opacity: 0、visibility: hidden、display: none 优劣和适用场景







## 三、Javascript基础

### 3-1 作用域与闭包、this与执行上下文



### 3-2 原型与继承



### 3-3 单线程、异步与事件机制

##### 3-3-1 JavaScript为什么引入异步？它有哪些使用场景？



### 3-4 promise、async/await、generator函数



### 3-5 深浅拷贝



### 3-6 JavaScript中的常用API

##### 3-6-1 class关键字定义类和普通构造函数定义类的区别

1. class不像构造函数，没有变量提升
2. class中挂载在原型上的方法是不可枚举的，而构造函数的prototype上添加的方法是可枚举的
3. 构造函数可以不使用new关键字，作为普通函数使用，而class定义的类必须加上new，作为构造器使用
4. class在继承中有两条原型链：子类的proto指向父类，子类的prototype的proto指向父类的prototype
5. ES5中的原生构造函数无法实现继承，ES6中使用class语法可以实现继承

##### 3-6-2 如何实现浏览器内多个标签页之间的通信？



## 四、设计模式

#### 4-1 什么是设计模式？设计模式如何解决复杂问题？



#### 4-2 介绍下观察者模式和订阅-发布模式的区别，各自适用于什么场景

#### 



 


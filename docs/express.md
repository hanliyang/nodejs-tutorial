原生的 http 模块在某些方面表现不足以应对我们的开发需求，所以我们就需要使用框架来加快我们的开发效率，框架的目的就是提高效率，让我们的代码更高度统一。
在 Node 中，有很多 Web 开发框架，我们这里以学习 `Express` 为主。

## Express 介绍

- Express 是一个基于 Node.js 平台，快速、开放、极简的 web 开发框架。


- 作者：[tj](https://github.com/tj)
  - [tj 个人博客](http://tjholowaychuk.com/)
  - 知名的开源项目创建者和协作者
  - Express、commander、ejs、co、Koa...
  - 已经离开 Node 社区，转 Go 了
  - [知乎 - 如何看待 TJ 宣布退出 Node.js 开发，转向 Go？](https://www.zhihu.com/question/24373004)
- 丰富的 API 支持，强大而灵活的中间件特性
- Express 不对 Node.js 已有的特性进行二次抽象，只是在它之上扩展了 Web 应用所需的基本功能
- 有很多[流行框架](http://expressjs.com/en/resources/frameworks.html)基于 Express


- [Express 官网](http://expressjs.com/)
- [Express 中文文档（非官方）](http://www.expressjs.com.cn/)
- [Express GitHub仓库](https://github.com/expressjs/express)



## 起步

### 安装

> 参考文档：http://expressjs.com/en/starter/installing.html

```shell
# 创建并切换到 myapp 目录
mkdir myapp
cd myapp

# 初始化 package.json 文件
npm init -y

# 安装 express 到项目中
npm i express
```

### Hello World

> 参考文档：http://expressjs.com/en/starter/hello-world.html

```javascript
// 0. 加载 Express
const express = require('express')

// 1. 调用 express() 得到一个 app
//    类似于 http.createServer()
const app = express()

// 2. 设置请求对应的处理函数
//    当客户端以 GET 方法请求 / 的时候就会调用第二个参数：请求处理函数
app.get('/', (req, res) => {
  res.send('hello world')
})

// 3. 监听端口号，启动 Web 服务
app.listen(3000, () => console.log('app listening on port 3000!'))
```

### 基本路由

> 参考文档：http://expressjs.com/en/starter/basic-routing.html

路由（Routing）是由一个 URI（或者叫路径标识）和一个特定的 HTTP 方法（GET、POST 等）组成的，涉及到应用如何处理响应客户端请求。

每一个路由都可以有一个或者多个处理器函数，当匹配到路由时，这个/些函数将被执行。

路由的定义的结构如下：

```javascript
app.METHOD(PATH, HANDLER)
```

其中：

- `app` 是 express 实例
- `METHOD` 是一个 [HTTP 请求方法](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)
- `PATH` 是服务端路径（定位标识）
- `HANDLER` 是当路由匹配到时需要执行的处理函数

下面是一些基本示例。

Respond with `Hello World!` on the homepage:

```javascript
// 当你以 GET 方法请求 / 的时候，执行对应的处理函数
app.get('/', function (req, res) {
  res.send('Hello World!')
})
```

Respond to POST request on the root route (`/`), the application’s home page:

```javascript
// 当你以 POST 方法请求 / 的时候，指定对应的处理函数
app.post('/', function (req, res) {
  res.send('Got a POST request')
})
```

Respond to a PUT request to the `/user` route:

```javascript
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user')
})
```

Respond to a DELETE request to the `/user` route:

```typescript
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user')
})
```

For more details about routing, see the [routing guide](http://expressjs.com/en/guide/routing.html).

## 处理静态资源

> 参考文档：http://expressjs.com/en/starter/static-files.html

```javascript
// 开放 public 目录中的资源
// 不需要访问前缀
app.use(express.static('public'))

// 开放 files 目录资源，同上
app.use(express.static('files'))

// 开放 public 目录，限制访问前缀
app.use('/public', express.static('public'))

// 开放 public 目录资源，限制访问前缀
app.use('/static', express.static('public'))

// 开放 publi 目录，限制访问前缀
// path.join(__dirname, 'public') 会得到一个动态的绝对路径
app.use('/static', express.static(path.join(__dirname, 'public')))
```

## 使用模板引擎

> 参考文档：
>
> - [Using template engines with Express](http://expressjs.com/en/guide/using-template-engines.html)

我们可以使用模板引擎处理服务端渲染，但是 Express 为了保持其极简灵活的特性并没有提供类似的功能。

同样的，Express 也是开放的，它支持开发人员根据自己的需求将模板引擎和 Express 结合实现服务端渲染的能力。

### 配置使用 art-template 模板引擎

> 参考文档：
>
> - [art-template 官方文档](https://aui.github.io/art-template/)

这里我们以 [art-template](https://github.com/aui/art-template) 模板引擎为例演示如何和 Express 结合使用。



安装：

```shell
npm install art-template express-art-template
```

配置：

```javascript
// 第一个参数用来配置视图的后缀名，这里是 art ，则你存储在 views 目录中的模板文件必须是 xxx.art
// app.engine('art', require('express-art-template'))

// 这里我把 art 改为 html
app.engine('html', require('express-art-template'))
```

使用示例：

```javascript
app.get('/', function (req, res) {
  // render 方法默认会去项目的 views 目录中查找 index.html 文件
  // render 方法的本质就是将读取文件和模板引擎渲染这件事儿给封装起来了
  res.render('index.html', {
    title: 'hello world'
  })
})
```

如果希望修改默认的 `views` 视图渲染存储目录，可以：

```javascript
// 第一个参数 views 是一个特定标识，不能乱写
// 第二个参数给定一个目录路径作为默认的视图查找目录
app.set('views', 目录路径)
```

### 其它常见模板引擎

JavaScript 模板引擎有很多，并且他们的功能都大抵相同，但是不同的模板引擎也各有自己的特色。

大部分 JavaScript 模板引擎都可以在 Node 中使用，下面是一些常见的模板引擎。

- ejs
- handlebars
- jade
  - 后改名为 pug
- nunjucks

## 解析表单 post 请求体

> 参考文档：
>
> - [GitHub - body-parser](https://github.com/expressjs/body-parser)

在 Express 中没有内置获取表单 POST 请求体的 API，这里我们需要使用一个第三方包：`body-parser`。

安装：

```shell
npm install --save body-parser
```

配置：

```javascript
var express = require('express')
// 0. 引包
var bodyParser = require('body-parser')

var app = express()

// 配置 body-parser
// 只要加入这个配置，则在 req 请求对象上会多出来一个属性：body
// 也就是说你就可以直接通过 req.body 来获取表单 POST 请求体数据了
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))
// parse application/json
app.use(bodyParser.json())
```

使用：

```javascript
app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  // 可以通过 req.body 来获取表单 POST 请求体数据
  res.end(JSON.stringify(req.body, null, 2))
})
```

## 使用 Session

> 参考文档：https://github.com/expressjs/session

安装：

```shell
npm install express-session
```

配置：

```javascript
// 该插件会为 req 请求对象添加一个成员：req.session 默认是一个对象
// 这是最简单的配置方式，暂且先不用关心里面参数的含义
app.use(session({
  // 配置加密字符串，它会在原有加密基础之上和这个字符串拼起来去加密
  // 目的是为了增加安全性，防止客户端恶意伪造
  secret: 'itcast',
  resave: false,
  saveUninitialized: false // 无论你是否使用 Session ，我都默认直接给你分配一把钥匙
}))
```

使用：

```javascript
// 添加 Session 数据
req.session.foo = 'bar'

// 获取 Session 数据
req.session.foo
```

提示：默认 Session 数据是内存存储的，服务器一旦重启就会丢失，真正的生产环境会把 Session 进行持久化存储。

---

## 路由

> 参考文档：
>
> - [Routing](http://expressjs.com/en/guide/routing.html)

一个非常基础的路由：

```javascript
var express = require('express')
var app = express()

// respond with "hello world" when a GET request is made to the homepage
app.get('/', function (req, res) {
  res.send('hello world')
})
```

### 路由方法

```javascript
// GET method route
app.get('/', function (req, res) {
  res.send('GET request to the homepage')
})

// POST method route
app.post('/', function (req, res) {
  res.send('POST request to the homepage')
})
```

### 路由路径

This route path will match requests to the root route, /.

```javascript
app.get('/', function (req, res) {
  res.send('root')
})
```

This route path will match requests to /about.

```javascript
app.get('/about', function (req, res) {
  res.send('about')
})
```

This route path will match requests to /random.text.

```javascript
app.get('/random.text', function (req, res) {
  res.send('random.text')
})
```

Here are some examples of route paths based on string patterns.

This route path will match acd and abcd.

```javascript
app.get('/ab?cd', function (req, res) {
  res.send('ab?cd')
})
```

This route path will match abcd, abbcd, abbbcd, and so on.

```javascript
app.get('/ab+cd', function (req, res) {
  res.send('ab+cd')
})
```

This route path will match abcd, abxcd, abRANDOMcd, ab123cd, and so on.

```javascript
app.get('/ab*cd', function (req, res) {
  res.send('ab*cd')
})
```

This route path will match /abe and /abcde.

```javascript
app.get('/ab(cd)?e', function (req, res) {
  res.send('ab(cd)?e')
})
```

Examples of route paths based on regular expressions:

This route path will match anything with an “a” in the route name.

```javascript
app.get(/a/, function (req, res) {
  res.send('/a/')
})
```

This route path will match butterfly and dragonfly, but not butterflyman, dragonflyman, and so on.

```javascript
app.get(/.*fly$/, function (req, res) {
  res.send('/.*fly$/')
})
```

#### 动态路径

```
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }
```

定义动态的路由路径：

```javascript
app.get('/users/:userId/books/:bookId', function (req, res) {
  res.send(req.params)
})
```

### 路由处理方法

### app.route()

### express.Router

Create a router file named router.js in the app directory, with the following content:

```javascript
const express = require('express')

const router = express.Router()

router.get('/', function (req, res) {
  res.send('home page')
})

router.get('/about', function (req, res) {
  res.send('About page')
})

module.exports = router
```

Then, load the router module in the app:

```javascript
const router = require('./router')

// ...

app.use(router)
```

## 中间件

> 参考文档：
>
> - [Writing middleware for use in Express apps](http://expressjs.com/en/guide/writing-middleware.html)
> - [Using middleware](http://expressjs.com/en/guide/using-middleware.html)

Express 的最大特色，也是最重要的一个设计，就是中间件。一个 Express 应用，就是由许许多多的中间件来完成的。

为了理解中间件，我们先来看一下我们现实生活中的自来水厂的净水流程。

![自来水厂净水过程](./media/water-middleware.jpeg)

在上图中，自来水厂从获取水源到净化处理交给用户，中间经历了一系列的处理环节，我们称其中的每一个处理环节就是一个中间件。这样做的目的既提高了生产效率也保证了可维护性。

### 一个简单的中间件例子：打印日志

```javascript
app.get('/', (req, res) => {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
  res.send('index')
})

app.get('/about', (req, res) => {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
  res.send('about')
})

app.get('/login', (req, res) => {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
  res.send('login')
})
```

在上面的示例中，每一个请求处理函数都做了一件同样的事情：请求日志功能（在控制台打印当前请求方法、请求路径以及请求时间）。

针对于这样的代码我们自然想到了封装来解决：

```javascript
app.get('/', (req, res) => {
  // console.log(`${req.method} ${req.url} ${Date.now()}`)
  logger(req)
  res.send('index')
})

app.get('/about', (req, res) => {
  // console.log(`${req.method} ${req.url} ${Date.now()}`)
  logger(req)
  res.send('about')
})

app.get('/login', (req, res) => {
  // console.log(`${req.method} ${req.url} ${Date.now()}`)
  logger(req)
  res.send('login')
})

function logger (req) {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
}
```

这样的做法自然没有问题，但是大家想一想，我现在只有三个路由，如果说有10个、100个、1000个呢？那我在每个请求路由函数中都手动调用一次也太麻烦了。

好了，我们不卖关子了，来看一下我们如何使用中间件来解决这个简单的小功能。

```javascript
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
  next()
})

app.get('/', (req, res) => {
  res.send('index')
})

app.get('/about', (req, res) => {
  res.send('about')
})

app.get('/login', (req, res) => {
  res.send('login')
})

function logger (req) {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
}
```

上面代码执行之后我们发现任何请求进来都会先在服务端打印请求日志，然后才会执行具体的业务处理函数。那这个到底是怎么回事？

### 中间件的组成

![中间件的组成](../media/express-mw.png)

中间件函数可以执行以下任何任务：

- 执行任何代码
- 修改 request 或者 response 响应对象
- 结束请求响应周期
- 调用下一个中间件

### 中间件分类

- 应用程序级别中间件
- 路由级别中间件
- 错误处理中间件
- 内置中间件
- 第三方中间件

#### 应用程序级别中间件

不关心请求路径：

```javascript
var app = express()

app.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})
```

限定请求路径：

```javascript
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})
```

限定请求方法：

```typescript
app.get('/user/:id', function (req, res, next) {
  res.send('USER')
})
```

多个处理函数：

```javascript
app.use('/user/:id', function (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})
```

多个路由处理函数：

```javascript
app.get('/user/:id', function (req, res, next) {
  console.log('ID:', req.params.id)
  next()
}, function (req, res, next) {
  res.send('User Info')
})

// handler for the /user/:id path, which prints the user ID
app.get('/user/:id', function (req, res, next) {
  res.end(req.params.id)
})
```

最后一个例子：

```javascript
app.get('/user/:id', function (req, res, next) {
  // if the user ID is 0, skip to the next route
  if (req.params.id === '0') next('route')
  // otherwise pass the control to the next middleware function in this stack
  else next()
}, function (req, res, next) {
  // render a regular page
  res.render('regular')
})

// handler for the /user/:id path, which renders a special page
app.get('/user/:id', function (req, res, next) {
  res.render('special')
})
```



#### 路由级别中间件

创建路由实例：

```javascript
var router = express.Router()
```

示例：

```javascript
var app = express()
var router = express.Router()

// a middleware function with no mount path. This code is executed for every request to the router
router.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})

// a middleware sub-stack shows request info for any type of HTTP request to the /user/:id path
router.use('/user/:id', function (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})

// a middleware sub-stack that handles GET requests to the /user/:id path
router.get('/user/:id', function (req, res, next) {
  // if the user ID is 0, skip to the next router
  if (req.params.id === '0') next('route')
  // otherwise pass control to the next middleware function in this stack
  else next()
}, function (req, res, next) {
  // render a regular page
  res.render('regular')
})

// handler for the /user/:id path, which renders a special page
router.get('/user/:id', function (req, res, next) {
  console.log(req.params.id)
  res.render('special')
})

// mount the router on the app
app.use('/', router)
```

另一个示例：

```javascript
var app = express()
var router = express.Router()

// predicate the router with a check and bail out when needed
router.use(function (req, res, next) {
  if (!req.headers['x-auth']) return next('router')
  next()
})

router.get('/', function (req, res) {
  res.send('hello, user!')
})

// use the router and 401 anything falling through
app.use('/admin', router, function (req, res) {
  res.sendStatus(401)
})
```



#### 错误处理中间件

```javascript
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

#### 内置中间件

- [express.static](http://expressjs.com/en/4x/api.html#express.static) serves static assets such as HTML files, images, and so on.
- [express.json](http://expressjs.com/en/4x/api.html#express.json) parses incoming requests with JSON payloads. **NOTE: Available with Express 4.16.0+**
- [express.urlencoded](http://expressjs.com/en/4x/api.html#express.urlencoded) parses incoming requests with URL-encoded payloads. **NOTE: Available with Express 4.16.0+**

官方支持的中间件列表：

- https://github.com/senchalabs/connect#middleware

#### 第三方中间件

### 中间件应用

#### 输出请求日志中间件

>  功能：实现为任何请求打印请求日志的功能。

`logger.js` 定义并导出一个中间件处理函数：

```javascript
module.exports = (req, res, next) => {
  console.log(`${req.method} -- ${req.path}`)
  next()
}

```

`app.js` 加载使用中间件处理函数：

```javascript
app.use(logger)
```

#### 统一处理静态资源中间件

> 功能：实现 express.static() 静态资源处理功能

`static.js` 定义并导出一个中间件处理函数：

```javascript
const fs = require('fs')
const path = require('path')

module.exports = function static(pathPrefix) {
  return function (req, res, next) {
    const filePath = path.join(pathPrefix, req.path)
    fs.readFile(filePath, (err, data) => {
      if (err) {
        // 继续往后匹配查找能处理该请求的中间件
        // 如果找不到，则 express 会默认发送 can not get xxx
        return next()
      }
      res.end(data)
    })
  }
}

```

`app.js` 加载并使用 static 中间件处理函数：

```javascript
// 不限定请求路径前缀
app.use(static('./public'))
app.use(static('./node_modules'))

// 限定请求路径前缀
app.use('/public', static('./public'))
app.use('/node_modules', static('./node_modules'))
```



## 错误处理

> 参考文档：
>
> - [Error handling](http://expressjs.com/en/guide/error-handling.html)

## 常用 API

> 参考文档：
>
> - [4.x API](http://expressjs.com/en/4x/api.html)

### express

- express.json
- express.static
- express.Router
- express.urlencoded()

### Application

- app.set
- app.get
- app.locals

### Request

- req.app
- req.query
- req.body
- req.cookies
- req.ip
- req.hostname
- Req.method
- req.params
- req.path
- req.get()

### Response

- res.locals
- res.append()
- res.cookie()
- res.clearCookie()
- res.download()
- res.end()
- res.json()
- res.jsonp()
- res.redirect()
- res.render()
- res.send()
- res.sendStatus()
- res.set()
- res.status()

### Router

- router.all()
- router.METHOD()
- router.use()
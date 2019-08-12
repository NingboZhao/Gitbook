# 数据科学知识模型

# 模拟服务端数据

$$\left(\frac{x d x}{d y}-\frac{y d y}{d x}\right)^{2}$$

----

\begin{equation}
\left(\frac{x d x}{d y}-\frac{y d y}{d x}\right)^{2}
\end{equation}


在前面的章节中，我们设置了代理，于是所有的 HTTP 请求都可以先到达本地开发服务器，再被转发。在实际的开发中，后端的服务不一定马上可用，这就需要本地服务器另外一个能力：模拟数据（mock）。设置代理是 mock 的前提。

一个 ajax 请求发送到本地开发服务器后，我们可以设置：如果请求满足某个规则，则不转发这个请求，而是直接返回一个「假」结果给浏览器。在实际的开发中，我们常常先和服务端的同学商定 http 请求的接口接受什么参数，返回什么结果，然后先用 mock 数据来模拟，自己和自己「联调」。等待服务端同学开发好了，再解除 mock，用真实数据「联调」。

[]()<a name="515719f9"></a>
## 模拟正常返回数据

设置模拟数据时需要在工程根目录下的 `mock` 子目录中的建立文件。首先在工程中增加 mock 目录，并在其中创建文件 `puzzlecards.js`（取其他名字也可以，名字这里不需要）。如果想 mock 掉我们在上一个章节中的向 `/dev/random_joke` 的 ajax 调用，需要写入以下内容到文件，

```js
const random_jokes = [
  {
    setup: 'What is the object oriented way to get wealthy ?',
      punchline: 'Inheritance',
  },
  {
    setup: 'To understand what recursion is...',
    punchline: "You must first understand what recursion is",
  },
  {
    setup: 'What do you call a factory that sells passable products?',
    punchline: 'A satisfactory',
  },
];

let random_joke_call_count = 0;

export default {
  'get /dev/random_joke': function (req, res) {
    const responseObj = random_jokes[random_joke_call_count % random_jokes.length];
    random_joke_call_count += 1;
    setTimeout(() => {
      res.json(responseObj);
    }, 3000);
  },
};
```

如果你不断地刷新页面，会发现每次拿到的数据是不同的。并且由于 setTimeout 的存在使得卡片的更新变慢了。

![](https://cdn.nlark.com/lark/0/2018/png/58329/1533281128902-083962d8-ced4-498c-8cbb-2f9fcfe01303.png#align=left&alt=image.png&width=747)

我们通过这个例子解释一下怎么写 mock 数据。

首先，整个文件需要 export 出一个 js 对象。对象的 key 是由

```text
<Http_verb> <Resource_uri>
```

构成的，值是 function，当一个 ajax 调用匹配了 key 后，与之对应的 function 就会被执行。函数中我们调用 res.json 就可以给浏览器返回结果。函数中可以使用 setTimeout 来模拟异步调用服务时的时延。

[]()<a name="a8919c0b"></a>
## 模拟出错

利用 res.status 也可以模拟 http 请求出错。例如，我们把文件中的 export default 块替换成下面的内容，

```js
export default {
  'get /dev/random_joke': function (req, res) {
    res.status(500);
    res.json({});
  },
};
```

在 dva model 中我们加入简单的错误捕获:

```js
import { message } from 'antd';

// ... 原有逻辑不修改

  try { // 加入 try catch 捕获抛错
    const puzzle = yield call(request, endPointURI);
    yield put({ type: 'addNewCard', payload: puzzle });

    yield call(delay, 3000);

    const puzzle2 = yield call(request, endPointURI);
    yield put({ type: 'addNewCard', payload: puzzle2 });
  } catch (e) {
    message.error('数据获取失败'); // 打印错误信息
  }
```

于是可以看到出错状况下的页面：

![](https://cdn.nlark.com/lark/0/2018/png/58329/1533282626015-6abd2a20-4263-4c68-a983-63f9820b1049.png#align=left&alt=image.png&width=747)

> 在每一个调用点做打印错误信息很麻烦，这里只是为了展示 mock 出错场景。在实际的开发中，一般会统一处理 http 请求错误时的信息提示。


[]()<a name="c15ede39"></a>
## 简单数据模拟

刚才的模拟中，mock 具备动态改变、延时返回等能力，如果你不需要这个能力，也可以简单地使用对象。

```js
export default {
  'get /dev/random_joke': {
    setup: 'What is the object oriented way to get wealthy ?',
    punchline: 'Inheritance',
  },
};
```

# 深入理解 umi

本文介绍 umi 框架的一些深入概念，帮助大家理解背后的运行机制。

下图是 umi 的架构图，我们试着从此图来一步步理解 umi 。

![](https://gw.alipayobjects.com/zos/rmsportal/hawpVKfSgndqDucTgjJQ.png#width=600)

[]()<a name="a6ddd5ae"></a>
## 基于路由

举个 SPA 的例子，比如我们访问 `/users`，会由 `./src/pages/users.js` 决定具体渲染什么，按我们的理解，这其中 `/users` 是路由，`./src/pages/users.js` 是路由组件，他们俩组成了一个路由配置，然后多个路由配置又形成了一个完整的应用。不难发现，在这个应用里，路由即入口。

umi 是基于路由的，所以具备了管理入口的能力。你甚至可以简单地理解为 umi = 路由 + webpack，当然我们在此基础上做了很多额外的工作。然后，管理了入口之后，能做的事情就很多了。

比如：

- 开发时按需编译
- 运行时按需加载，做 code-splitting
- 智能提取公共代码，加速用户访问，通常是被 路由数/2 引用的模块才被提取到公共代码中
- 服务端渲染
- 基于路由的埋点
- 基于约定，如果 `./src/pages/404.js` 存在则添加为 fallback 路由
- ...

这里有很大的想象空间。

[]()<a name="977355ff"></a>
## 从源码到上线的生命周期管理

市面上的框架基本都是从源码到构建产物，很少会考虑到各种发布流程，而 umi 则多走了这一步。

下图是 umi 从源码到上线的一个流程。

![](https://gw.alipayobjects.com/zos/rmsportal/NKsqmTAttwTzYVMJMcnB.png#width=600)

umi 首先会加载用户的配置和插件，然后基于配置或者目录，生成一份路由配置，再基于此路由配置，把 JS/CSS 源码和 HTML 完整地串联起来。用户配置的参数和插件会影响流程里的每个环节。

举个例子，比如以下目录：

```text
+ src
  + layouts/index.js
  + pages
    - a.js
    - b.js
    - 404.js
```

会生成路由配置如下：

```js
{
  component: 'layouts/index.js',
  routes: [
    { path: '/a', exact: true, component: 'pages/a.js' },
    { path: '/b', exact: true, component: 'pages/b.js' },
    { component: 'pages/404.js' },
  ],
}
```

以及可运行的代码：

```js
const routes = {
  component: require('layouts/index.js'),
  routes: [
    { path: '/a', exact: true, component: require('pages/a.js') },
    { path: '/b', exact: true, component: require('pages/b.js') },
    { component: require('pages/404.js') },
  ],
};

export default () =>
  <Router history={window.g_history}>
    { renderRoutes(routes) }
  </Router>
```

另外，HTML 也是一个很重要的环节，因为他才是真正的入口，js 和 css 都是在 HTML 里发起的。HTML 在 umi 里是一等公民，这意味着我们可以通过插件来调整他的输出，以便和各个后台系统对接，以及打通各种发布流程。

举个例子，我们要配置 webpack externals 掉 react 来优化构建产物，通常我们要做两步：

1. 配置 `externals: { react: 'window.React' }`
2. 在 html 里引入 [https://unpkg.com/react@16.4.1/cjs/react.production.min.js](https://unpkg.com/react@16.4.1/cjs/react.production.min.js)

这里的问题是一个功能需要在两个地方维护并且一一对应。然后在 umi 里，由于 HTML 具备插件的能力，所以可以做到你只需要完成第一步，然后 umi 自动帮你做第二步。

最后，通过 umi 可以把各种部署方式封装成插件，实现同一份源码，装载不同的插件，就可以部署到不同平台。比如 umi + umi-plugin-deploy-offline 可以部署为离线包；umi + umi-plugin-deploy-chair 可以部署到 chair 系统。

[]()<a name="6e06d1df"></a>
## 插件机制

关于 umi 的插件机制你可以阅读 umi 的文档[《插件开发》](https://umijs.org/zh/plugin/develop.html)来了解更多。
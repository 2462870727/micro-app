<p align="center">
  <a href="https://micro-zoe.github.io/micro-app/">
    <img src="https://zeroing.jd.com/micro-app/media/logo.png" alt="logo" width="200"/>
  </a>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@micro-zoe/micro-app">
    <img src="https://img.shields.io/npm/v/@micro-zoe/micro-app.svg" alt="version"/>
  </a>
  <a href="https://www.npmjs.com/package/@micro-zoe/micro-app">
    <img src="https://img.shields.io/npm/dt/@micro-zoe/micro-app.svg" alt="downloads"/>
  </a>
  <a href="https://github.com/micro-zoe/micro-app/blob/master/LICENSE">
    <img src="https://img.shields.io/npm/l/@micro-zoe/micro-app.svg" alt="license"/>
  </a>
  <a href="https://gitter.im/microzoe/micro-app">
    <img src="https://badges.gitter.im/microzoe/micro-app.svg" alt="gitter">
  </a>
  <a href="https://travis-ci.com/github/micro-zoe/micro-app">
    <img src="https://api.travis-ci.com/micro-zoe/micro-app.svg?branch=master" alt="travis"/>
  </a>
  <a href="https://coveralls.io/github/micro-zoe/micro-app?branch=master">
    <img src="https://coveralls.io/repos/github/micro-zoe/micro-app/badge.svg?branch=master" alt="coveralls"/>
  </a>
</p>

English｜[简体中文](./README.zh-cn.md)｜[Documentation](https://micro-zoe.github.io/micro-app/)｜[Discussions](https://github.com/micro-zoe/micro-app/discussions)｜[Gitter](https://gitter.im/microzoe/micro-app)

# 📖Introduction
micro-app is a micro front-end framework launched by JD Retail. It renders based on webcomponent-like and realizes the micro front-end from component thinking, it aiming to reduce the difficulty of getting started and improve work efficiency. 

It is the lowest cost framework for accessing micro front-end, only one line of code needs to be changed, and even nothing needs to be changed to access the micro front-end, and provides a series of perfect functions such as JS sandbox, style isolation, element isolation, preloading, resource address completion, plugin system, data communication and so on.

The micro-app has nothing to do with the technology stack, and can be used in any front-end framework.

# Getting Start
The micro front-end is divided into base application and micro application. We list the modifications required for base application and micro application respectively, and introduce the use of micro-app in detail.

## base application
> The base application takes the vue framework as an example

1、Install
```bash
yarn add @micro-zoe/micro-app
```

2、import at the entrance
```js
// main.js
import microApp from '@micro-zoe/micro-app'

microApp.start()
```

3、Assign a route to the micro application
```js
// router.js
import Vue from 'vue'
import VueRouter from 'vue-router' // vue-router@3.x
import MyPage from './my-page.vue'

Vue.use(VueRouter)

const routes = [
  {
    // 👇 Non-strict matching, /my-page/xxx will be matched to the MyPage component
    path: '/my-page/*', 
    name: 'my-page',
    component: MyPage,
  },
]

export default routes
```

4、Use components in `my-page` pages
```html
<!-- my-page.vue -->
<template>
  <div>
    <h1>micro application</h1>
    <!-- 👇 name is the application name, globally unique, url is the html address -->
    <micro-app name='app1' url='http://localhost:3000/' baseroute='/my-page'></micro-app>
  </div>
</template>
```

> Please refer to [Routing Chapter](https://micro-zoe.github.io/micro-app/docs.html#/zh-cn/route) for the relationship between url and micro application routing

## micro application
> The micro application takes the react framework as an example

1、Add basename for route(If the base application is history route and the micro application is hash route, it is not necessary to set the baseroute, this step can be skipped)

```js
// router.js
import { BrowserRouter, Switch, Route } from 'react-router-dom'

export default function AppRoute () {
  return (
    // 👇 the micro application can get the baseroute issued by the base application through window.__MICRO_APP_BASE_ROUTE__
    <BrowserRouter basename={window.__MICRO_APP_BASE_ROUTE__ || '/'}>
      <Switch>
        ...
      </Switch>
    </BrowserRouter>
  )
}
```

2、Set cross-domain support in the headers of webpack-dev-server.
```js
devServer: {
  headers: {
    'Access-Control-Allow-Origin': '*',
  },
},
```

Then micro front-end can be rendered normally, and the react micro application is embedded in the vue base application. The effect is as follows：

![image](https://img10.360buyimg.com/imagetools/jfs/t1/188373/14/17696/41854/6111f4a0E532736ba/4b86f4f8e2044519.png)

The above lists the usage of react and Vue framework. They can be combined freely. For example, the base application is react, the micro application is Vue, or the base application is Vue, the micro application is react, or both the base application and the micro application are react and Vue. The micro-app has no restrictions on the front-end framework, and any framework can be used as a base application to embed any type of micro application of the framework.

More detailed configuration can be viewed [Documentation](https://micro-zoe.github.io/micro-app/docs.html#/zh-cn/start).

# 🤝 Contribution
If you're interested in this project, you're welcome to mention pull request, and also welcome your "Star" ^_^

### development
1、Clone
```
git clone https://github.com/micro-zoe/micro-app.git
```

2、Install dependencies
```
yarn bootstrap
```

3、Run project
```
yarn start
```

For more commands, see [DEVELP](https://github.com/micro-zoe/micro-app/blob/master/DEVELOP.md)

# FAQ
<details>

  <summary>What are the advantages of micro-app?</summary>
  It is easy to use and low invasive. It only needs to change a small amount of code to access the micro front-end, and provides rich functions at the same time.

</details>
<details>
  <summary>How compatible?</summary>
  The micro-app relies on two newer APIs, CustomElements and Proxy.

  For browsers that do not support CustomElements, they can be compatible by introducing polyfills. For details, please refer to: [webcomponents/polyfills](https://github.com/webcomponents/polyfills/tree/master/packages/custom-elements)。

  However, Proxy is not compatible for the time being, so the micro-app cannot be run on browsers that do not support Proxy.

  Browser compatibility can be viewed: [Can I Use](https://caniuse.com/?search=Proxy)

  The general is as follows:
  - desktop: Except IE browser, other browsers are basically compatible.
  - mobile: ios10+、android5+
</details>

<details>
  <summary>Must micro applications support cross-domain?</summary>
  yes!

  If it is a development environment, you can set headers in webpack-dev-server to support cross-domain.

  ```js
  devServer: {
    headers: {
      'Access-Control-Allow-Origin': '*',
    },
  }
  ```

  If it is a production environment, you can support cross-domain through [Configuration nginx](https://segmentfault.com/a/1190000012550346).

</details>

<details>
  <summary>Does it support vite?</summary>
  
  Yes, please see [adapt vite](https://micro-zoe.github.io/micro-app/docs.html#/zh-cn/advanced?id=_2%e3%80%81%e9%80%82%e9%85%8dvite) for details.
</details>

# Contributors
<a href="https://github.com/micro-zoe/micro-app/graphs/contributors"><img src="./.github/contributors.svg" /></a>
<!-- opencollective is inaccurate  -->
<!-- <a href="https://github.com/micro-zoe/micro-app/graphs/contributors"><img src="https://opencollective.com/micro-app/contributors.svg?width=890&button=false" /></a> -->

# License
[MIT License](https://github.com/micro-zoe/micro-app/blob/master/LICENSE)

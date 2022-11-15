# vue-router

> ==注意：==

1. 在 Vue2 和 Vue3 中使用 vue-router 的方式，有些地方的用法是不同的
2. vue-router@3 和 vue-router@4 也有所不同，具体查看官网

## 动态组件

> 定义

1. 效果和路由有点相似，根据需求动态显示不同的组件
2. 但是没办法像路由一样编程式导航如：`push` , `replace`，也没有浏览器路径的语义引导
3. 动态组件适合小规模的局部 tab

> 示例：

~~~vue
<template>
    <div>
        <!-- 点击触发切换组件方法 changeTab -->
        <ul @click='changeTab'> 
            <li>Tab1</li>
            <li>Tab2</li> 
        </ul>
        <!-- :is="tab"此处相当于router-link效果-->
        <component :is="tab"></component> <!-- component 标签相当于 router-view 出口 -->
    </div>
</template>

<script>
import tab1 from 'path/to/tab1.vue' //相比vue-router要引入很多文件  麻烦
import tab2 from 'path/to/tab2.vue'
//其实vue-router也引入文件，但是在.vue文件里面引入那么多就很难看

export default {
    components: {
        tab1, tab2
    },
    data() {
        tab:'tab1' //默认显示tab1
    },
    methods: {
        // 切换自己想要展示组件
        changeTab() {
            // 改变 tab 的值，让其显示不同组件
        }
    }
}
</script>
~~~

## vue-router

### router，routes，route 区别

- `route`：当前激活的路由的信息对象

  每个对象都是局部的，可以获取当前路由的 path, name, params, query 等属性，例如：

  - `$route.name`：字符串，当前路由的名字

  - `$route.path` ：字符串，当前路由对象的路径，不带参数

    如：192.168.0.1/index?page=1，path 为 /index

  - `$route.fullPath` ：字符串，是路由全地址，包括连接携带的参数

    如：192.168.0.1/index?page=1，fullPath 为 /index?page=1

  - `meta` ：路由元信息 也就是每个路由身上携带的信息
  - `children` ：嵌套路由，也就是路由子组件，和 `routes` 配置路由信息一样，是一个路由配置数组

  - `$route.params` ：对象，包含路由中的动态片段和全匹配片段的键值对

  - `$route.query` ：对象，包含路由中查询参数的键值对

    例如，对于 /index?id=1 ，会得到 $route.query.id == 1

  - `$route.matched`：返回一个数组，包含当前路由的所有嵌套路径片段的 **路由记录** 

    路由记录就是 `routes` 配置数组中的对象副本 (还有在 `children` 数组)

- `router`：全局的 router 实例，包含了很多属性、方法等

  - `router.currentRoute` ：当前路由对应的路由信息对象

  - `router.mode` ：路由的使用模式

    - `hash` ： 

      1. 默认为 hash 模式
      2. 通过 window.onhashchange 监听 hash 的改变，借此实现无刷新跳转的功能
      3. url 后面的`#` 就是 hash 符号，中文名为哈希符或者锚点，在 hash 符号后的值称为 hash 值
      4. hash 是 url 的锚点，代表的是网页中的一个位置，仅仅会改变 # 后面部分，浏览器只会滚动对应的位置，而不会重新加载页面
      5.  hash 值是用来指导浏览器动作的，对服务器没有影响，它不会被包括在 http 请求中，它传给后端的一直是网址的域名部分，故也不会重新加载页面
      6. 每改变一次 hash （ window.location.hash）,都会在浏览器的访问历史中增加一个记录，同这样你就可以使用浏览器的后退前进页面按钮啦！

    - `history` ：

      1. 通过调用 window.history 对象上的一系列方法来实现页面的无刷新跳转

      2. 路径直接拼接在端口号后面，后面的路径也会随着http请求发给服务器

      3. 前端的 URL 必须和向发送请求后端 URL 保持一致，否则会报404 错误

      4. 新的 URL 可以是与当前URL同源的任意 URL，也可以与当前URL一样

      5. 使用 history 模式 生产环境会存在问题：

         虽然 History 模式可以丢掉不美观的 #，也可以正常的前进后退，但是刷新 f5 后，此时浏览器就会访问服务器，如果 nginx 没有匹配到当前url，例如 nginx 只配置了 域名的首页，其他页面例如：login 登录页就没有配置，此时就会出现 404 的页面

         生产环境 刷新 404 的解决办法：

         可以在 `nginx` 做代理转发，在 `nginx` 中配置按顺序检查参数中的资源是否存在，如果都没有找到，让  `nginx` 内部重定向到项目首页

  - `this.$router.push()` ：切换路由，有历史记录

  - `this.$router.replace()` ：替换路由，没有历史记录

  - `router.beforeEach((to, from, next) => {} )` ：全局前置守卫

    三个参数分别对应：`去哪儿`，`从哪儿来`，`下一步`

  - ……

- `routes`：指在 `src/router/index.js` 创建的路由实例的配置项，用来配置多个 页面路由对象

------

### 路由传参方式

> 路由传参有三种传参方式：（便于路由发生跳转A->B时把A路由组件的一些数据传递给B）

1. **params参数：路由需要占位，属于URL当中一部分，需要在路由配置中的 path 后面加上 `/:参数名`**

   指定 params 参数可传可不传：在配置路由时在path的 `/:参数名` 后面加上问号 `?` 即可

   params 参数可传可不传，但如果传递是空串：使用 undefined 可以解决 params 参数可传可不传，

   ~~~JS
   this.$router.push({
     name:"search",
     params: {
         keyword: '' || undefined,
     },
   })
   ~~~

2. query参数：路由不需要占位，写法类似于 Ajax 当中 query 参数，是 k=v 键值对形式

3. props参数：

   在路由配置中定义参数，有三种写法：

   ~~~JS
   {
        path:"/search/:keyword?",
        component:Search,
        meta:{show:true},
        name:"search", 
        //1、布尔值写法：（只能传params）
        props:true,
            
       //2、对象写法：(额外给路由组件传递一些props)
        props：{a:1,b:2}
       
        //3、函数写法：（最常用，它可以将params参数、query参数，通过props传递给路由组件）
        props: ($route) => {
          return {keyword:$route.params.keyword,k:$route.query.k};
        }
   }
   ~~~

   在组件中接收参数：

   ~~~JS
   <script>
     export default {
       props: ['keyword','a','b']
     }
   </script>
   ~~~

> 编程式跳转时push | replace()方法的参数有三种格式：(一般能用对象就用对象写法)

1. 字符串：

- params 传参：

  ~~~JS
  this.$router.push('/search/' + this.keyword); 
  // URL:localhost:8080/#/search/abc
  ~~~

- query 传参：

  ~~~JS
  this.$router.push('/search/' + 'k=' + this.keyword); 
  // URL:localhost:8080/#/search/k=abc
  ~~~

- params 传参和 query 传参混合使用：

  ~~~JS
  this.$router.push('/search/' + this.keyword + '?k=' + this.keyword.toUpperCase());
  //URL:localhost:8080/#/search/abc?k=ABC
  ~~~

2. 模板字符串：

   ~~~JS
   this.$router.push(`/search/${this.keyword}?k=${this.keyword.toUpperCase()}`);
   //URL:localhost:8080/#/search/abc?k=ABC
   ~~~

3. 对象：

   注意：对象传参的形式可以是 `name、params` 、 `name、query` 和 `name、params、query`

   **但 path 不能与 params 结合传参**

   ~~~JS
   this.$router.push({
     name:'search',
     params:{
       keyword: this.keyword
     }
   })
   ~~~

> 获取路由传递过来的参数：

模板里的写法：

- `{{$route.params.keyword}}`

- `{{$route.query.k}}`

脚本里的写法:

- `this.$route.params.keyword`

- `this.$route.query.k`

> 区别：

1. query 在 name 和 path 两种传递方式下都是有效的； params 只有name 传参是有效的；
2. query 传参在浏览器地址中显示参数，params 不在浏览器中显示参数；
3. query 传参强制刷新后参数还在； params 强制刷新后参数丢失；
4. params 是路由的一部分，必须要有；query 是拼接在 url 后面的参数，没有也没关系

### 基本路由配置

1. 在 `src/router/index.js` 配置路由信息

   ~~~JS
   import Vue from 'vue'
   import VueRouter from 'vue-router'
   import HomeView from '../views/HomeView.vue'
   
   Vue.use(VueRouter)
   
   const routes = [
     {
       path: '/',
       name: 'home',
       component: HomeView
     },
     {
       path: '/about',
       name: 'about',
       component: () => import('../views/AboutView.vue'),
       meta: { requiresAuth: true }
     }
   ]
   
   const router = new VueRouter({
     routes
   })
   
   export default router
   ~~~

2. 在 程序入口 `main.js` 导入

   ~~~JS
   +import router from './router'
   
   new Vue({
     +router,
     render: h => h(App)
   }).$mount('#app')
   ~~~

### 动态路由

以 **左侧菜单栏** 为例，不同的用户，权限不同，返回的 **左侧菜单栏** 也不同，例如，管理员和普通用户

> 1、搭建一个新项目

技术栈：`vue2` + `vue-router` + `vuex` + `element-ui`

> 2、配置项目依赖

1. 下载 npm：

   `npm install element-ui nprogress normalize.css sass-loader`

2. 配置 vue.config.js （类似于 配置 webpack 的 webpack.config.js 文件）

   ~~~JS
   'use strict'
   const path = require('path')
   
   function resolve(dir) {
     return path.join(__dirname, dir)
   }
   
   // All configuration item explanations can be find in https://cli.vuejs.org/config/
   module.exports = {
     publicPath: '/',              // 部署应用包时的基本 URL,用法和 webpack 本身的 output.publicPath 一致
     outputDir: 'dist',            // 构建输出目录（打包位置）
     assetsDir: 'static',          // 放置生成的静态资源（js，css，img，fonts）的（相对于outputDir）的目录
     lintOnSave: false,            // 是否校验语法
     productionSourceMap: false,   // 如果你不需要生产环境的 source map，可以将其设置为 false 以加速生产环境构建
     devServer: {
       port: 8888,
       open: true,
     },
     configureWebpack: {           // 绝对路径
       resolve: {
         alias: {
           '@': resolve('src')
         }
       }
     }
   }
   ~~~

> 3、实现思路

1. 前端在本地写好路由表，以及每个路由对应的角色，也就是哪些角色可以看到这个菜单 / 路由。

2. 登录的时候，向后端请求得到登录用户的角色（管理者，普通用户）

3. 利用路由守卫者(`router.beforeEach`)，根据取到的用户角色，跟本地的路由表进行对比，过滤出用户对应的路由，并利用路由进行菜单渲染
4. 我们将储存在将 `storage` 中的 `token` 作为用户是否登录的标志，如果当前 `storage` 中有 `token`，表明当前系统已被登录
5. 将系统所有页面分为两类，需要登录才能查看的页面，不需要登录的 `login.vue`，`register.vue`……
6. 前端每次跳转路由时，在 `beforeEach`中实现以下逻辑：
   - 前往登录页面
   - 前往非登录页面
     - 用户无需登录就能访问的页面
     - 用户需要登录就能访问的页面
       - 如果没登录，就跳转登录页
       - 已经登录，判断用户是否拥有查看页面的权限，有权限就直接跳转，否则就弹出无权限的提醒

7. 不引入后端，用 `Mock`模拟端口数据， 作为服务请求的响应
8. 结合 localStorage 和 Vuex 做 状态管理，这里就是用来 管理 用户 的一些状态

> 4、这里手动编写 mock 端口数据和逻辑处理

后端返回数据方式一：**把所有的路由和权限业务处理都放在了前端**

~~~JS
{
    "code": 200,
    "data": {
        "name": "管理员",
        "avatar": "https://sf3-ttcdn-tos.pstatp.com/img/user-avatar/ccb565eca95535ab2caac9f6129b8b7a~300x300.image",
        "desc": "管理员 - admin",
        "username": "admin",
        "password": "654321",
        "token": "rtVrM4PhiFK8PNopqWuSjsc1n02oKc3f",
        "routes": [
            {
                "id": 1,
                "name": "/",
                "path": "/",
                "component": "Layout",
                "redirect": "/index",
                "hidden": false,
                "children": [
                    {
                        "name": "index",
                        "path": "/index",
                        "meta": {
                            "title": "index"
                        },
                        "component": "index/index"
                    }
                ]
            },
            { 
                "id": 2, 
                "name": "/form", 
                "path": "/form", 
                "component": "Layout", 
                "redirect": "/form/index", 
                "hidden": false, 
                "children": [
                    {
                        "name": "/form/index", 
                        "path": "/form/index", 
                        "meta": { "title": "form"}, 
                        "component": "form/index"
                    }
                ]
            },
        ]
    }
}
~~~

后端返回数据方式二：**把所有的路由和权限业务处理都放在了前端**

~~~JS
//代码位置：router/index.js
 {
    path: '',
    component: layout, //整体页面的布局(包含左侧菜单跟主内容区域)
    children: [{
      path: 'main',
      component: main,
      meta: {
        title: '首页', //菜单名称
        roles: ['user', 'admin'], //当前菜单哪些角色可以看到
      }
    }]
  }

~~~

1. 所有路由都写在前端里面，然后在 `router` 的 `meta` 属性，写上对应 `user` 的 `role`
2. 登录的时候，再根据 后端返回的 `该用户权限`，去和 `meta` 即路由元信息进行 `过滤比对权限`
3. 把该用户角色所对应的路由处理好，渲染处理，这也是主流的一种处理方式
4. 一旦上线发布后，想要修改就需要重新打包处理，而且不能经由后台动态新增删除

**端口数据：**

~~~JS
const dynamicUser = [
    {
        name: "管理员",
        avatar: "https://sf3-ttcdn-tos.pstatp.com/img/user-avatar/ccb565eca95535ab2caac9f6129b8b7a~300x300.image",
        desc: "管理员 - admin",
        username: "admin",
        password: "654321",
        token: "rtVrM4PhiFK8PNopqWuSjsc1n02oKc3f",
        routes: [
            { id: 1, name: "/", path: "/", component: "Layout", redirect: "/index", hidden: false, children: [
                { name: "index", path: "/index", meta: { title: "index" }, component: "index/index" },
            ]},
            { id: 2, name: "/form", path: "/form", component: "Layout", redirect: "/form/index", hidden: false, children: [
                { name: "/form/index", path: "/form/index", meta: { title: "form" }, component: "form/index" }
            ]},
            { id: 3, name: "/example", path: "/example", component: "Layout", redirect: "/example/tree", meta: { title: "example" }, hidden: false, children: [
                { name: "/tree", path: "/example/tree", meta: { title: "tree" }, component: "tree/index" },
                { name: "/copy", path: "/example/copy", meta: { title: "copy" }, component: "tree/copy" }
            ] },
            { id: 4, name: "/table", path: "/table", component: "Layout", redirect: "/table/index", hidden: false, children: [
                { name: "/table/index", path: "/table/index", meta: { title: "table" }, component: "table/index" }
            ] },
            { id: 5, name: "/admin", path: "/admin", component: "Layout", redirect: "/admin/index", hidden: false, children: [
                { name: "/admin/index", path: "/admin/index", meta: { title: "admin" }, component: "admin/index" }
            ] },
            { id: 6, name: "/people", path: "/people", component: "Layout", redirect: "/people/index", hidden: false, children: [
                { name: "/people/index", path: "/people/index", meta: { title: "people" }, component: "people/index" }
            ] }
        ]
    },
    {
        name: "普通用户",
        avatar: "https://sf1-ttcdn-tos.pstatp.com/img/user-avatar/6364348965908f03e6a2dd188816e927~300x300.image",
        desc: "普通用户 - people",
        username: "people",
        password: "123456",
        token: "4es8eyDwznXrCX3b3439EmTFnIkrBYWh",
        routes: [
            { id: 1, name: "/", path: "/", component: "Layout", redirect: "/index", hidden: false, children: [
                { name: "index", path: "/index", meta: { title: "index" }, component: "index/index" },
            ]},
            { id: 2, name: "/form", path: "/form", component: "Layout", redirect: "/form/index", hidden: false, children: [
                { name: "/form/index", path: "/form/index", meta: { title: "form" }, component: "form/index" }
            ]},
            { id: 3, name: "/example", path: "/example", component: "Layout", redirect: "/example/tree", meta: { title: "example" }, hidden: false, children: [
                { name: "/tree", path: "/example/tree", meta: { title: "tree" }, component: "tree/index" },
                { name: "/copy", path: "/example/copy", meta: { title: "copy" }, component: "tree/copy" }
            ] },
            { id: 4, name: "/table", path: "/table", component: "Layout", redirect: "/table/index", hidden: false, children: [
                { name: "/table/index", path: "/table/index", meta: { title: "table" }, component: "table/index" }
            ] },
            { id: 6, name: "/people", path: "/people", component: "Layout", redirect: "/people/index", hidden: false, children: [
                { name: "/people/index", path: "/people/index", meta: { title: "people" }, component: "people/index" }
            ] }
        ]
    }
]

export default dynamicUser
~~~

**端口数据的逻辑处理：**

~~~JS
import dynamicUser from "../../mock"
import { Message } from "element-ui"

login() {
    this.$refs.userForm.validate(( valid ) => {
        if(valid) {
            let flag = !1
            window.localStorage.removeItem("userInfo")
            dynamicUser.forEach(item => {
                if(item["username"] == this.user['username'] && item["password"] == this.user['password']) {
                    flag = !0
                    Message({ type: 'success', message: "登录成功", showClose: true, duration: 3000 })
                    window.localStorage.setItem("userInfo", JSON.stringify(item))
                    // 这里用catch捕获错误，而且不打印，解释在下方
                    this.$router.replace({ path: "/" }).catch(() => {})
                }
            })
            if(!flag) Message({ type: 'warning', message: "账号密码错误，请重试!", showClose: true, duration: 3000 })
        } else return false
    })
}
~~~

> 5、前端实现

1. 路由守卫者拦截 `beforeach`,，并动态渲染出路由表

   - 在router的 `beforeEach` 里面分别有三个参数to, form和next，分别对应着去哪儿，从哪儿来，下一步
   - 根据去哪儿（to）， 判断路由指向是否在过滤的路由地址数组里，
     - 如果在，则直接进入页面，无需判断，例如登录页面, 注册页面, 找回密码等（具体看业务需求）

   ~~~JS
   
   
   ~~~

2. 在 `main.js` 里面，引入处理动态路由的文件

   ~~~JS
   import Vue from "vue"
   import App from "./App.vue"
   import router from "./router"
   import store from "./store"
   import ElementUI from "element-ui"
   import 'element-ui/lib/theme-chalk/index.css'
   +import "./router/router-config"
   
   Vue.config.productionTip = false
   Vue.use(ElementUI)
   
   new Vue({
     router,
     store,
     render: (h) => h(App),
   }).$mount("#app")
   ~~~


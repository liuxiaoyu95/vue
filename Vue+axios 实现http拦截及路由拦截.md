# Vue+axios 实现http拦截及路由拦截

#####   技术栈

- vue2.0
- vue-router
- axios

#####   拦截器

    首先我们要明白设置拦截器的目的是什么,当我们需要统一处理http请求和响应时我们通过设置拦截器处理方便很多.

    这个项目我引入了element ui框架,所以我是结合element中loading和message组件来处理的.我们可以单独建立一个http的js文件处理axios,再到main.js中引入.



#### http拦截

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](undefined)

```
  /**
   * http配置
   */
  // 引入axios以及element ui中的loading和message组件
  import axios from 'axios'
  import { Loading, Message } from 'element-ui'
  // 超时时间
  axios.defaults.timeout = 5000
  // http请求拦截器
 var loadinginstace
 axios.interceptors.request.use(config => {
   // element ui Loading方法
   loadinginstace = Loading.service({ fullscreen: true })
   return config
 }, error => {
   loadinginstace.close()
   Message.error({
     message: '加载超时'
   })
   return Promise.reject(error)
 })
 // http响应拦截器
 axios.interceptors.response.use(data => {// 响应成功关闭loading
   loadinginstace.close()
   return data
 }, error => {
   loadinginstace.close()
   Message.error({
     message: '加载失败'
   })
   return Promise.reject(error)
 })
 
 export default axios
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](undefined)

 

   这样我们就统一处理了http请求和响应的拦截.当然我们可以根据具体的业务要求更改拦截中的处理.

####  路由拦截

    我们可以通过路由拦截做什么?我认为最主要的便是对权限的控制,比如有的页面需要登录了才能进入,有些页面不同身份渲染不同.接下来简单的讲一下登录拦截.

    

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](undefined)

```
  import Vue from 'vue'
  import Router from 'vue-router'
  
  Vue.use(Router)
  
  const router = new Router({
    routes: [
      {
        path: '/',
       /*
       *  按需加载 
       */
       component: (resolve) => {
         require(['../components/Home'], resolve)
       }
     }, {
       path: '/record',
       name: 'record',
       component: (resolve) => {
         require(['../components/Record'], resolve)
       }
     }, {
       path: '/Register',
       name: 'Register',
       component: (resolve) => {
         require(['../components/Register'], resolve)
       }
     }, {
       path: '/Luck',
       name: 'Luck', 
        // 需要登录才能进入的页面可以增加一个meta属性
       meta: { 
         requireAuth: true
       },
       component: (resolve) => {
         require(['../components/luck28/Luck'], resolve)
       }
     }
   ]
 })
 //  判断是否需要登录权限 以及是否登录
 router.beforeEach((to, from, next) => {
   if (to.matched.some(res => res.meta.requireAuth)) {// 判断是否需要登录权限
     if (localStorage.getItem('username')) {// 判断是否登录
       next()
     } else {// 没登录则跳转到登录界面
       next({
         path: '/Register',
         query: {redirect: to.fullPath}
       })
     }
   } else {
     next()
   }
 })
 
 export default router
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](undefined)

转发链接: https://blog.csdn.net/luxu1990/article/details/80083088

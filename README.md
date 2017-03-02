# 子组件修改父组件

子组件注册，且在要调用父组件的 方法A 中用 $emit(方法A) 指明该方法。
```
 <template id="dialog-template" class="dialog-template">
     <span class="iconfont icon-close" @click="close"></span>
     ....
 </template>

 <script>

 Vue.component('modal-dialog', {
        props: ['show'],
        template: '#dialog-template',
        methods: {
            close: function () {
                this.$emit('close')
            }
        }
    });
  new Vue({
         el: '#app',
         data: {
             showDialog: false
         },
         methods: {
             closeDialog: function () {
                 this.showDialog = false;
             },
             openDialog: function () {
                 this.showDialog = true;
             }
         }
     });
 </script>
```
使用子组件时通过

v-on:自组件方法="父组件方法"     指定子组件要调用父组件的什么方法。

v-bind:自组件数据="父组件数据"   将子组件数据与父组件数据建立连接。
```
 <div id="app">
     <modal-dialog v-bind:show="showDialog" v-on:close="closeDialog">
     .....
     </modal-dialog>
     ...
 </div>
```
**不要再子组件修改父组件的数据，而是通过父组件的方法修改，修改后子组件会自动响应**

[vue组件教程](http://www.cnblogs.com/keepfool/p/5637834.html)

## 跨域：
接口： http://211.149.193.19:8080/api/customers

通过 CORS
```
ajaxHelper.get(vm.apiUrl, null, callback);
```
响应头如下，
```
Access-Control-Allow-Origin:*
Cache-Control:no-cache
Content-Length:2202
Content-Type:application/json; charset=utf-8
```
通过 JSONP
```
ajaxHelper.jsonp(vm.apiUrl, null, callback);
```
响应头如下
```
Cache-Control:no-cache
Content-Encoding:gzip
Content-Length:507
Content-Type:text/javascript; charset=utf-8
```
CORS 与 JSONP都需要服务端做特殊处理去支持。且返回的数据格式不一样

## OAuth登入

登入后，返回的token，userName保存在session，
```
sessionStorage.setItem('accessToken', body.access_token);
sessionStorage.getItem('userName');
```
下次请求网络时取出来
```
headers.Authorization = 'Bearer ' + sessionStorage.getItem('accessToken');
```
在进入界面时，免登入
```
sessionStorage.getItem('userName');
```
退出时，清除保存在sessionStorage的userName和accessToken
```
sessionStorage.removeItem('accessToken');
sessionStorage.removeItem('userName');
```

## vue-router2
### 步骤
- 定义组件
- 定义路由
- 创建 router 实例，然后传 `routes` 配置
- 创建和挂载根实例。

具体参考示base-vue-router

### 怎么刚进入应用就渲染某个路由组件
刚进入应用都是进入到“/”这个路由的，如果想直接进入到“/home”怎么办，这里提供两种方法。一种是利用重定向，另一种是利用vue-router的导航式编程。
#### 重定向
```
 const routes = [
        {path: '/home', component: Home},
        {path: '/about', component: About},
        {path: '/', component: Home}
    ];
```
或者
```
 const routes = [
        {path: '/home', component: Home},
        {path: '/about', component: About},
        {path: '/', redirect: '/home'}
    ];
```
#### 导航式编程
利用vue-router的导航式编程的router.push方法也可以实现上面的需求。
```
// 在创建vue实例并挂载后调用
router.push('/goods')
```
router.push方法就是用来动态导航到不同的链接的。它会向history栈添加一个新的记录，点击<router-link :to="...">等同于调用router.push(...)。
### 动态匹配路由 watch
例如从 /user/foo 导航到 user/bar，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。
复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch（监测变化） $route 对象(在定义组件对象的时候)：
```
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```
### 嵌套
```
<template id="home">
    <div>
        <div>{{msg}}</div>
        <router-link to="/home/news">Home.news</router-link>
        <router-link to="/home/message">Home.message</router-link>
        <router-view></router-view>
    </div>
</template>

const routes = [
        {
            path: '/home/',
            component: Home,
            children: [
                {path: '', component: Message},
                {path: 'message', component: Message},
                {path: 'news', component: News},
            ]
        },
        {path: '/about', component: About},
    ];
```
- children下path不要再添加'/'
- 组件定义出router-link元素的to属性路径写全



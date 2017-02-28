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
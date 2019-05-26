## Vue 

1.代码示例

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue learning</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app" @mouseover="mm" @mouseout="nn" v-bind:title="title">
        {{msg}}

        <div>
            <input v-model="content" type="text" />
            <p>{{content}}</p>
        </div>
        
        <div>
            firstname : <input type="text" v-model="firstname"> 
            lastname : <input type="text" v-model="lastname">   
            <p>{{ fullname }}</p>
            <p>{{ count }}</p>
        </div>

        <div>
            <!-- v-if对应的布尔值为 false 时，直接将该元素从 dom 中移除 -->
            <!-- v-show对应的布尔值为 false 时，仅仅是为该元素添加 display:none 的 css 属性 -->
            <p v-if="show"> use v-if </p> 
            <p v-show="show"> use v-show </p> 
            
            <button @click="show_or_hide"> toggle </button>

            <ul>
                <!-- 此处的 key 是为了提高渲染的性能， -->

                <!-- 这个 key 的取值应该取每次循环都不一样的值，像[1,1,3]这种就不行，一般是用 index 下标索引来做 key -->
                <li v-for="(item, index) of list":key="index">{{ item }}</li> 

            </ul>
        </div>
    </div>  
    
    <script type="text/javascript">
        
        new Vue({
            el:"#app", // 挂载点
            data:{  // 具体数据
                msg:'ok',
                title:"my title",
                content:"my content",
                firstname:'123',
                lastname:'456',
                count:0,
                show:true,
                list:[1,2,3,4,5]
            },
            methods:{ // 方法
                mm:function(){
                    this.msg = 'world';
                },
                nn:function(){
                    this.msg = 'hello';
                },
                show_or_hide:function(){
                    this.show = !this.show
                }
            },
            computed:{ // 计算属性
                fullname:function(){
                    return this.firstname + this.lastname;
                }
            },
            watch:{ // 侦听器
                fullname:function(){
                    this.count++ 
                }
            }
        })
    </script>
</body>
</html>
```


* {{ content }} 绑定数据，双花括号绑定数据用以展示
* v-on:click="handleclick" 绑定方法，等同于@click="handleclick"，方法在 methods 中定义
* v-bind:value="changevalue" 绑定标签属性，等同于 :value="changevalue"
* v-html 渲染html 标签，会对数据进行转义，v-text 直接渲染字符串数据，不会转义 html 标签。
* v-model 进行双向数据绑定，用在 input，textarea，select，radio，checkbox 等标签上。
* methods 定义方法，且 methods 内的所有方法**不可使用箭头函数**定义，否则无法绑定到 Vue 实例上。
* computed 计算属性 **计算结果并返回，只有当被计算的值发生改变时才会触发**，一般用于根据其他值的变化返回产出一个新值并使用。
*  watch 侦听器 **与computed的区别是，watch更加适用于监听某一个值的变化并做对应的操作，比如请求后台接口等，而computed适用于计算已有的值并返回结果**
* v-if对应的布尔值为 false 时，直接将该元素从 dom 中移除
* v-show对应的布尔值为 false 时，仅仅是为该元素添加 display:none 的 css 属性 



2. 踩坑记录，当使用docker运行 node 环境并启动 vue 的脚手架服务时，映射端口后，必须将 config.js 里的 HOST 改为 0.0.0.0，否则宿主机无法访问 docker 内部的服务。

   

3. **组件之间传输数据**:

   * 父组件往子组件传值：

     1. 父组件定义需要传输的值，父组件引用子组件，并绑定一个属性到到子组件上，之后子组件需要接收的就是这个属性。
     2. 子组件利用 props 表明需要接收的值(即父组件中绑定在子组件属性上的那个属性)，之后子组件自定义方法去处理这个接收到的值即可。
   * 子组件往父组件传值：

     1. 子组件定义需要传输给父组件的值，然后通过this.$emit 方法将该值发送给父组件，父子组件需要约定一个属性，子组件发送到该属性上，父组件监听该属性。
     2. 父组件监听属性，并自定义方法处理子组件传过来的值。
   * 非父子组件传值：
     1.  使用 vuex 记录全局状态，各组件共享该状态。

4. 箭头函数的作用域是词法作用域，在调用 axios 时，then 和 catch 中使用箭头函数，该箭头函数中使用 this 时，this 直接指向 Vue 实例。



## Axios

1. axios 默认不携带 cookie，前端使用时需设置 axios 的 withCredentials 为 true，后端也需设置接收 cookie。如 flask_cors 的 CORS(app,supports_credentials=True)



## Vuex

1. 一般在 store.js 上统一管理组件的状态，用于在各组件之间共享同一数据。

2. * state 记录数据状态。
   * mutations 用于提交改变 state 中的值，mutations 是唯一改变 state 中的值的途径。mutations 的所有操作均为同步操作。
   * actions 用于提交 commit 触发 mutations，且actions 支持异步操作。
   * getters 用于在获取 state 的值之前可进行一定的额外操作。

   ```javascript
   state:{
     'arg1':0,
     'arg2':true,
     'arg3':[1,2,3]
   },
   mutations:{
     fn1(state){
    		// dosomething   
     },
     fn2(state, params){
   		// dosomething
     }
   },
   actions:{
     fn3({ commit }){
       commit('fn1')
     }
   },
   getters:{
     arg4(state){
       return state.arg1
     }
   }
   ```

3. 在组件中通过`this.$store.commit('mutations 中定义的方法名')`来调用 mutations 中的方法，前提是 vuex 的已经被绑定在全局vue 实例上，以便可用 this.$store 的方式来调用。
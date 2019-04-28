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

                <!-- （这个注释暂时存疑）这个 key 的取值应该取每次循环都不一样的值，像[1,1,3]这种就不行 -->
                <li v-for="item of list":key="item">{{ item }}</li> 

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


* {{ content }} 绑定数据
* v-on:click="handleclick" 绑定方法，等同于@click="handleclick"
* v-bind:value="changevalue" 绑定标签属性，等同于 :value="changevalue"
* v-html 渲染html 标签，会对数据进行转义，v-text 直接渲染字符串数据，不会转义 html 标签。
* v-model 进行双向数据绑定
* computed 计算属性  watch 侦听器
* v-if对应的布尔值为 false 时，直接将该元素从 dom 中移除
* v-show对应的布尔值为 false 时，仅仅是为该元素添加 display:none 的 css 属性 



2. 踩坑记录，当使用docker运行 node 环境并启动 vue 的脚手架服务时，映射端口后，必须将 config.js 里的 HOST 改为 0.0.0.0，否则宿主机无法访问 docker 内部的服务。

   
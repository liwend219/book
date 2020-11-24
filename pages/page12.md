### Vue3.0
对比Vue2，性能提升在哪里？
1. diff算法优化，Vue2的时候每次数据更新，它都会逐个比对每个虚拟dom树节点，一样的不更改，不一样的新的替换旧的，而Vue3则会一开始就对不会进行改变的节点打上标记，在diff比对DOM树的时候跳过有标记的节点
2. proxy代替defineProperty ,v2中使用的defineProperty对数组和对象的改变不能监听到，需要使用Vue.set进行重新赋值，而v3的proxy则能够监听到对象和数组的变化
3. 包更小，使用Vue推荐的vite可以获取更快的打包速度及优化
4. v2监听对象中的某个属性值，需要手动设置深度监听，v3也不需要了

> 看下面的代码看区别：  

html:
```
    <div id="app">
        <ul>
            <li v-for="(item, index) in arr" @click="change(index)">
                {{item}}
            </li>
        </ul>
    </div>
```
js:
```
// Vue2.0 (defineProprety)
var app = new Vue({
    el: '#app',
    data: {
        arr:['aa','bb','cc'],
    },
    methods:{
        change(index) {
            //会发现视图上的值并没有改变，但是数据确实已经发生变化
            // this.arr[index] = 'haha'

            //只能使用拓展运算符重新赋值或者使用Vue.set重新赋值视图才会改变
            this.arr = [...this.arr]
            //或者
            // Vue.set(this.arr,index,'haha')
        }
    }
})
---------------------------------------------------------
// Vue3.0 (Proxy)
const myApp = {
    setup (){
        const arr = Vue.reactive([
            'aa','bb','cc'
        ])
        function change(index){
            //可以立即更新视图！这在vue2中是不行的
            arr[index] = 'haha'
        };
        return {arr, change};
    },
}
Vue.createApp(myApp).mount('#app')
```
> Vue3变量声明

```
const myApp = {
    setup (){
        var msg = '哈哈哈';
        function changeMsg(){
            msg = '嘿嘿嘿' // wtf? 视图没更新！
        }
        //是的，vue3中变量需要使用 ref,reactive进行声明才能双向绑定，以上不可以
        //正确声明如下
        const msg = ref('哈哈哈');
        function changeMsg(){
            msg.value = '嘿嘿嘿' // 实时更新了！甚至使用的const声明的也改变了msg的值，这还不知道为啥
        }
        return {msg, changeMsg};
    },
}
Vue.createApp(myApp).mount('#app')
```
**vue3中变量需要使用 ref,reactive进行声明才能双向绑定,一般ref用于普通变量声明，reactive用于对象数组声明，对象及数组使用ref声明同样不可双向绑定**



### Vue3.0
对比Vue2，性能提升在哪里？
1. `diff`算法优化，`Vue2`的时候每次数据更新，需要比对的dom树可能里面是静态动态混在一起的，它要逐个比对每个虚拟dom树节点，一样的不更改，不一样的新的替换旧的，而Vue3则只会创建一次静态节点，diff比对的时候不会比对这些节点
2. `proxy`代替`defineProperty` ,v2中使用的`defineProperty`对数组和对象的改变不能监听到，需要使用`Vue.set`进行重新赋值，而v3的`proxy`则能够监听到对象和数组的变化
3. 包更小，使用Vue推荐的`vite`可以获取更快的打包速度及优化（开发时不需要预编译，直接利用了现代浏览器大都支持es6的特性，省去了预编译转换es5的步骤，视图更新极快）
4. v2监听对象中的某个属性值，需要手动设置深度监听，v3也不需要了（proxy）

缺点在哪？
1. `setup`组合api即是优点也是缺点，全部堆在一起（当然也可摘出来模块化）
2. `ref`创建的变量，使用的时候需要加上`.value`,当然，这实际上是因为ref的底层是使用`reactive`进行包装的，包装成`proxy`的对象，既是对象，就是键值对的形式存在，于是就自动给你加上了`value`的键
3. 由于使用了`proxy`，不支持任何IE浏览器，对于某些有IE用户的公司来说，用不了

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



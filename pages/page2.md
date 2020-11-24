### this （ES5）
通常意义上，人们把this 理解成指向函数自身  

this绑定规则  
1. 默认绑定
2. 隐式绑定
3. 硬绑定
4. new绑定

#### 默认绑定
> 默认绑定，在不能应用其它绑定规则时使用的默认规则，通常是独立函数调用  

```
function sayHi(){
    console.log('Hello,', this.name);
}
var name = 'YvetteLau';
sayHi();
```
#### 隐式绑定
> 函数的调用是在某个对象上触发的，即调用位置上存在上下文对象。典型的形式为 XXX.fun().我们来看一段代码  

```
function sayHi(){
    console.log('Hello,', this.name); // Hello,YvetteLau.
}
var person = {
    name: 'YvetteLau',
    sayHi: sayHi
}
var name = 'Wiliam';
person.sayHi();
```
> 需要注意的是：对象属性链中只有最后一层会影响到调用位置  

```
function sayHi(){
    console.log('Hello,', this.name);
}
//this 指向 person2
var person2 = {
    name: 'Christina',
    sayHi: sayHi
}
var person1 = {
    name: 'YvetteLau',
    friend: person2
}
person1.friend.sayHi();
```
> 隐式绑定有一个大陷阱，绑定很容易丢失  

```
//Hi直接指向了sayHi的引用，在调用的时候，跟person没有半毛钱的关系
function sayHi(){
    console.log('Hello,', this.name); Hello,Wiliam.
}
var person = {
    name: 'YvetteLau',
    sayHi: sayHi
}
var name = 'Wiliam';
var Hi = person.sayHi;
Hi();
```
#### 显式绑定
> 就是通过call,apply,bind的方式，显式的指定this所指向的对象  

```
function sayHi(){
    console.log('Hello,', this.name);
}
var person = {
    name: 'YvetteLau',
    sayHi: sayHi
}
var name = 'Wiliam';
var Hi = person.sayHi;
Hi.call(person); //Hi.apply(person) //Hello,YvetteLau
```

#### new 绑定
> 将构造函数的作用域赋值给新对象，即this指向这个新对象
```
    function sayHi(name){
        this.name = name;
        
    }
    var Hi = new sayHi('Yevtte');
    console.log('Hello,', Hi.name);
```
#### 绑定规则优先级
new绑定 > 显式绑定 > 隐式绑定 > 默认绑定



### 箭头函数下的this

箭头函数是没有this的，他的this来自于它作用域上一层的this



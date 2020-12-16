### 原型与原型链

原型可以让所有共同原型的对象共享原型属性，原型方法，节省代码，内存空间

```
function Person(name) {
  this.name = name;
}

const person1 = new Person('liwendong')
const person2 = new Person('liwendong')

person1 === person2 // false  虽然传入属性都一样，但是实例化以后分别存在两个地址，所以不想等

Person.prototype.sayHello = function(){
    console.log(this.name)
}
person1.sayHello === person2.sayHello // true  因为这是原型方法，所以想等

```

#### `constructor`
构造函数和实例存在一个等式 

`person.constructor === Person;`  

实例的属性 `constructor` 指向构造函数  
同时，这个函数的原型的 constructor 会指向这个函数  
`Person.prototype.constructor === Person; // true`  
```
Person.prototype
{
  //constructor 即 Person
  constructor:{
    ...
  }
}
```

#### `__proto__`  
> 在 JavaScript 中，每个 JavaScript 对象（普通对象和函数对象）都具有一个属性 __proto__，这个属性会指向该对象的原型  

```
function Person() {

};
const person = new Person();

console.log(person.__proto__ === Person.prototype); // true
person.__proto__   // Person.prototype
person.__proto__.__proto__   // Object.prototype
person.__proto__.__proto__.__proto__   // null,原型链的最顶层就是null

person.name  // undefiend
//为什么是undefined ？ 
上面其实就是原型链的查找过程
实例对象.xxx -> 构造函数.prototype.xxx -> Object.prototype.xxx ,找不到就返回undefined了


```
> 原型链查找 ， 实例对象.xxx -> 构造函数.prototype.xxx -> Object.prototype.xxx 

实例查找原型用 `__proto__` , 函数查找用 `prototype`




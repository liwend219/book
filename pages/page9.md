### 变量及变量函数提升

变量： var 存在变量提示，let不存在，同作用域下var可以重复命名，let不可以

> 函数提升 > 变量提升

```
//变量提升
console.log(a) // undefind
var a = 'haha'

相当于

var a;
console.log(a) // 所以 undefind 啦
a = 'haha'

---------------

console.log(a) // error , let不存在变量提升
let a = 'haha'

```

函数提升：

> 函数是一等公民

```
var foo = 3;

function getFoo() {
  var foo = foo || 5;
  console.log(foo); // 输出 5
}

getFoo();

相当于

//我被提升到最上面啦！
function getFoo() {
  var foo;
  foo = foo || 5;
  console.log(foo);
}

var foo; //注意看，函数提升优先级大于变量提升
foo = 3;

getFoo();
```

解析
```
var a = 1;
function b() {
  a = 10;
  return;
  function a() {}
}
b();
console.log(a); // 1 

--------------

var a = 1;
function b() {
  a = 10;
  return;
}
b();
console.log(a); // 10

//不是return了就不存在提升了，有没有return都会提升，所以第一个函数的 a 是内部的 fun a 重新赋值的，并不是外面的变量a，而第二个就是外面的变量a
```




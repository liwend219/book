### 闭包
 用红宝书的一句话来说，就是：
>  ***闭包是指有权访问另外一个函数作用域中的变量的函数***

通常情况下，函数调用完即销毁，也不会保存其作用域内的所有状态，而闭包就是一直保存该函数在内存中不会被销毁，例如：
```
    function foo() {
        var a = 2;
        function bar() {
            console.log(a);
        }
        return bar;
    }
    var baz = foo();
    baz(); // 2
```
上面的例子就是闭包，函数内部返回一个函数，并且该作用域不会重置

```
    function foo() {
        var a = 2;
        function bar() {
            console.log(a++);
        }
        return bar;
    }
    var baz = foo();
    baz(); // 2
    baz(); // 3
    baz(); // 4
```
`a`在内存中指向的地址并没有随着函数执行结束而销毁，实际上，每次调用 `baz` 取到的 `a` 在内存中是同一个

### 柯里化
> 是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术

例如： 
```
//普通加法函数
function add (a,b){
    return a + b;
}
add(1, 2) // 3
---------------

//柯里化
function add (a){
    return function (b){
        return a + b;
    }
}
add(1)(2) // 3
let result = add(2)
result(3) // 5
result(4) // 6
...
```
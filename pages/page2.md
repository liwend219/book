### this （ES5）
通常意义上，人们把this 理解成指向函数自身  

看下面两个程序

```
    var name = '小明';
    function getName() {
        var name = '小李';
        console.log(this.name) // 小明
    }
    getName();

    -------

    var name = '小明';
    var obj = {
        name:'小李',
        getName:function () {
            console.log(this.name) // 小李
        }
    }
    obj.getName()
```

第一个在外部作用域调用，所以 `name` 是小明，第二个是使用 `obj` 进行调用，他的this即指向 `obj` ，所以它的 `name` 是小李


### 箭头函数下的this

箭头函数是没有this的，他的this来自于它作用域上一层的this



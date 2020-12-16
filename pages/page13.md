### typeScript

> 安装：`npm install -g typescript`

执行转换：`tsc 文件名.ts`

ts由以下几个模块组成
- 模块
- 函数
- 变量
- 语句和表达式
- 注释

> 函数：

```
//如果函数没有返回值，用 void标识
function alertName(): void {
    alert('My name is Tom');
}

// x: number => 代表 x参数必须为number类型， y: number => 代表y参数必须为number类型，
// 最后的number代表返回值必须为number类型，可省去
function sum(x: number, y: number): number {
    // return x + y + 'hello'; // error ，返回值必须为number
    return x + y;
}
function sum(x: number, y: number) {
    return x + y + 'hello'; // 成功 ，没有规定返回值类型
}
```
> 任意值：

```
//error ，string类型不可改变为number类型
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;
```
但是使用`any`类型，则可以改变类型
```
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
```
任意值访问任何存在或者不存在的属性都是可以的，包括方法
```
//全部不会报错
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();
```
未声明类型的变量默认是任意类型
```
let name;
name = 'adas';
name = 6;
===========
let name : any;
name = 'adas';
name = 6;
```
> 类型推断：

```
还是上面的例子，但是，这样是会报错的，因为类型推断出name是string类型
let name = 'adas';
name = 5
```
**如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查**

> 联合类型

```
let hello : string | number | string[] = 'hahah';
hello = 'aaa';
hello = 333;
hello = ["123",'bb']

// hello = [45,'bb'];  error,应该是字符串数组类型，这个数组里有数字
console.log(hello)
```

> 接口(interface)：

```
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```
有时我们希望不要完全匹配一个形状，那么可以用可选属性 (?)
```
interface Person {
    name: string;
    age?: number; // 可选属性
}

let tom: Person = {
    name: 'Tom'
};
```
有时候我们希望一个接口允许有任意的属性，可以使用如下方式
```
interface Person {
    name: string;
    age?: number;
    [propName: string]: any; // 任意属性
}

let tom: Person = {
    name: 'Tom',
    gender: 'male' // 任意属性
};
```
需要注意的是，**一旦定义了任意属性，那么其他属性的类型都必须是它的类型的子集。一个接口中只能定义一个任意属性**

只读属性(readonly)
```
interface Person {
    readonly id: number;
}

let tom: Person = {
    id: 89757,
};

tom.id = 9527; // error ,只读属性不可更改
```
> 数组

```
let fibonacci: number[] = [1, 1, 2, 3, 5]; //  普通定义一个数组
let fibonacci: Array<number> = [1, 1, 2, 3, 5];  // 数组泛型
------
//接口定义数组
interface NumberArray {
    [index: number]: number | string;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5, 'hello'];
```
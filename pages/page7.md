### 防抖节流  

**什么是防抖？**

> 任务频繁触发的情况下，只有任务触发的间隔超过指定间隔的时候，任务才会执行  

通俗解释： 防止某些任务重复执行，造成性能损耗和业务上的冲突，于是在任务执行之前如果又被调用，就清除之前的任务重新开始

代码实现：
1. 设置定时器
2. 设置一个闭包，返回一个方法
3. 如果反复进来，清空前面的定时器，再重新设置一遍

```
function debounce (fn) {
  let timer = null;
  return function() {
    clearTimeout(timer);

    timer = setTimeout(() => {
      fn.call(this.arguments);
    }, 1000);
  }
}
```
  

**什么是节流？**  

> 指定时间间隔内只会执行一次任务。  

通俗解释： 防止短时间内多次执行任务，造成性能损耗和业务上的冲突，于是让他规定时间内只能执行一次

代码实现：  
1. 设置一个标记
2. 设置一个闭包，返回一个方法
3. 如果重复进去的时候，标记已经动了，那就组织程序进一步运行
4. 如果定时器执行完了，设置这个标记为没动，允许下一次执行

```
function throttle (fn) {
  let timer = true;
  return function() {
    if (!timer) {
      return;
    }
    timer = false;
    setTimeout(() => {
      fn.call(this, arguments);
      timer = true;
    }, 1000);
  }
}
```
  
**防抖+节流** 

> 防抖有时候因为触发太过频繁，导致一次响应都没有

代码实现：
1. 设置 last 记录定时器开始的时间
2. 设置 timer 表示一个定时器
3. 获取当前时间 now
4. 如果当前时间 - 开始时间小于延迟时间，那么就防抖
5. 否则设置时间到了，执行函数

```
function throttle(fn, delay) {
  let last = 0, timer = null;
  return function (...args) {
    let now = new Date();
    if (now - last < delay){
      clearTimeout(timer);
      setTimeout(function() {
        last = now;
        fn.apply(this, args);
      }, delay);
    } else {
      // 这个时候表示时间到了，必须给响应
      last = now;
      fn.apply(this, args);
    }
  }
}
```
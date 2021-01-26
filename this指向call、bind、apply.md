# this指向
**1、全局变量默认挂载在window对象下**

**2、一般情况下this指向它的调用者**

**3、es6的箭头函数中，this指向创建者，并非调用者**

**4、通过call、apply、bind可以改改变this的指向**
**例：**
```
/*注：全局变量默认挂载在window对象下*/
var name = 'window';
const fun = function() {
  var name = 'cat'
  console.log(this.name) // window
  const fun1 = function() {
    console.log(this.name) // window
  }
  fun1();
}
fun();
```
```
/*注：一般情况下this指向它的调用者*/
var name = "window"
const a = {
  name: 'cat',
  fun: function() {
    console.log(this.name); // cat
    const fun1 = function() {
      console.log(this.name); // window
    }
    fun1();
  }
}
a.fun();
```
```
/*注：es6的箭头函数中，this指向创建者，并非调用者*/
var name = "window"
const a = {
  name: 'cat',
  fun: () => {
    console.log(this.name); // window
  }
}
a.fun();
```
```
/*注：通过call、apply、bind可以改改变this的指向*/
const a = {
  name: 'cat',
  fun: function(x1, x2) {
    console.log(this.name);
    console.log(x1 + x2)
  }
}
a.fun(1, 2) // cat 3
const b = {
  name: 'dog',
}
a.fun.call(b, 3, 4); // dog 7
a.fun.apply(b, [3, 4]) // dog 7
a.fun.bind(b, 3, 4)() // dog 7
```

### call、apply、bind 模拟

```
// call
Function.prototype.newCall = function(ctx, ...params) {
  if(typeof ctx === 'object') {
    ctx = ctx || window
  } else {
    ctx = Object.create(null);
  }
  const fn = Symbol();
  ctx[fn] = this;
  ctx[fn](...params);
  delete ctx[fn];
}
```
```
// apply
Function.prototype.newApply = function(ctx, params) {
  if(typeof ctx === 'object') {
    ctx = ctx || window
  } else {
    ctx = Object.create(null);
  }
  const fn = Symbol();
  ctx[fn] = this;
  if(Array.isArray(params)) {
    ctx[fn](...params);
  } else {
    ctx[fn]();
  }
  delete ctx[fn];
}
```
```
// bind
Function.prototype.newBind(ctx, ...params) {
  if(typeof ctx === 'object') {
    ctx = ctx || window;
  } else {
    ctx = Object.create();
  }
  return (...innerParams) {
    return this.call(ctx, ...params, ...innerParams);
  }
}
```
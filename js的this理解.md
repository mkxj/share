## js的this理解
1. 在构造函数执行的时候，函数体中的this都是当前类的实例

```js
function fn () {
  this.name = 'nihao'
}
```

this，就是一个指针，指向我们调用函数的对象，一般的this指的是函数运行时所在的环境

2. 一般的，谁调用它就指向谁，obj.fn,那么fn内的this指向obj，无人调用，this指向window,new的时候指向new出来的对象

```js
var obj = {
  name: 'obj1',
  fn: function () { console.log(this.name)},
  obj2: {
    name: 'obj2',
    fn2: function () {
      console.log(this.name)
    }
  }
}

var fn = obj.fn
var fn2 = obj.obj2

var fn1 = function () {
  console.log(this)
}

obj.fn() // 'obj1'
fn() // undefined
fn1() // window
fn2.fn2() // 'obj2'

```

注意：this 的值并不是由函数定义放在哪个对象里面决定，而是函数执行时由谁来唤起决定。

3. 默认情况下（非严格模式下,未使用 `use strict`），没找到直接调用者，this就指向window，在严格模式下，没找到直接调用者，this是undefined

```js
var name = 'nihao';
(function () {
  'use strict'
  console.log(this.name)
}())

(function () {
  // 'use strict'
  console.log(this.name)
}())

```

4. 箭头函数的this指向：默认指向在定义它时,它所处的对象,而不是执行时的对象, 定义它的时候,可能环境是window（即继承父级的this）,这点和普通函数有点区别

```js
var object = {
    data: [1,2,3],
    dataDouble: [1,2,3],
    double: function() {
        console.log('this inside of outerFn double()')
        console.log(this)
        return function() {
            console.log(this, 'in')
        }
    },
    doubleArrow: function() {
        console.log('this inside of outerFn doubleArrow()')
        console.log(this)
        return () => {
            console.log(this, 'in');
        }
    }
}
object.double()()
object.doubleArrow()()

```

5. 使用call,apply,bind，this指向绑定的对象(改变函数运行时的上下文)

- call：将 this 的值准确设置到你选择的一个对象fn通过逗号分隔传递多个参数给函数。如：fn.call(this, param1, param2, ...)。最后，执行函数。

- apply: 与call类似，差别在传参上有区别， fn.apply(this, [param1, param2])

- bind: 与call和apply的区别在于，在改变完函数的上下文之后，不会立即执行，而是会返回一个新的函数，供我们再调用

```js
var obj = {
  name: 'obj1',
  fn: function (a, b) {
    console.log(this.name)
    console.log(a + b)
  },
  obj2: {
    name: 'obj2',
    fn2: function () {
      console.log(this.name)
    }
  }
}

var fn = obj.fn
fn()
fn.call(obj, 1, 2)
fn.apply(obj, [1, 2])
var fn1 = fn.bind(obj, 1, 2)
fn1()

```

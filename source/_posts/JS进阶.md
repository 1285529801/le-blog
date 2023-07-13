---
title: JS进阶
---

## 1. 作用域

1. 局部作用域

   1. 函数作用域
   2. 块级作用域：有`{}`

2. 全局作用域

   1. `<script>`标签内部

   2.  `.js`文件内部内部

3. 作用域链

   本质上是底层的变量的查找机制（类似于事件冒泡）

   - 函数执行时，会优先在当前函数作用域内查找变量
   - 找不到在依次逐级查找父级作用域直到全局作用域

**在函数声明和变量声明同名同时，函数声明优先于变量声明**

## 2. 垃圾回收机制

**概念**：JS中内存的分配和回收都是自动完成的，内存不使用时就会被垃圾回收器自动回收

JS内存的生命周期：

- 内存分配
- 内存使用
- 内存回收

**注意**：

- 全局变量一般不会被回收

- 局部变量一般情况下，在不使用时就会被自动回收

**内存泄漏**：程序中分配的内存由于某种原因程序未释放或无法释放叫做内存泄漏（应该回收的内存没有被回收）

**垃圾回收算法**：

- 引用计数法
- 标记清除法

## 3. 闭包

闭包：内层函数＋外层函数的变量

触发闭包：当里层函数引用了外层函数的变量时，会触发闭包

闭包的基本格式：

```js
function a(){
	let s=10
	function b(){
        // b函数使用了外层a的变量s
		console.log(s)
	}
    return b
}
const fun = a()
fun()
```

作用：

- 将内部数据变量私有化，不会被外部直接修改
- 闭包内变量不会被垃圾回收机制回收
- 外部能够访问到函数内部的变量

问题：

- 可能会导致内存泄漏

## 4. 变量提升和函数提升

1. 变量提升

   - 在执行JS之前，会把所有`var`声明的变量提到当前作用域的最前面

   - 只提升声明，不提升赋值

   ```js
   console.log(num) // 打印出为undefined
   var num =10
   ```

   上面等同于下面

   ```js
   var num // 只声明但是为赋值
   console.log(num) // 打印出为undefined
   num = 10
   ```

   

2. 函数提升

   - 在执行JS之前，会把所有的函数声明提升到作用域的最前面

   - 只提升函数声明，不提升函数调用

   - 函数表达式不存在提升

     ```js
     fun()  // 结果为报错，因为提升的是变量而不是函数
     // 函数表达式
     var fun = function(){
         console.log('hello')
     }
     ```

     

   - **函数表达式必须先声明和赋值再调用，否则会报错**

## 5. 函数参数

##### 5.1 动态参数

`arguments`是函数内部的一个形参，但是不需要声明，直接使用，它是一个伪数组变量，只存在函数之中

使用场景：当传入方法的参数不确定时使用

```js
function getSum(){
    let sum = 0 
    for(let i=0;i<arguments.length;i++){
		sum+=arguments[i]
    }
    console.log(sum)
}
// 调用方法
getSum(1,2,3,4) // 输出10
```

**注意**：

- 在JS非严格模式下，argument和实参不会相互影响，也就是说argument的改变和实参没关系
- 在JS的严格模式下，argument和实参会相互影响，需要多加注意

##### 5.2 剩余参数

剩余参数：`...xxx` 是剩余参数，`‘xxx’`随便写，是一个真数组

使用场景：

1. 可以跟`arguments`一样使用

```js
function getSum(...arr){
    // 使用的时候不需要加‘...’
    console.log(arr)
}
// 调用方法
getSum(1,2,3,4) // 输出[1,2,3,4]
```

2. 用于获取多余的实参

```js
function getSum(a,b,...arr){
    // 使用的时候不需要加‘...’
    console.log(a)
    console.log(b)
    console.log(arr)
}
// 调用方法
getSum(1,2,3,4) 
// 输出a为1
// 输出b为2
// 输出arr为3,4
```

## 6. 箭头函数

##### 6.1 箭头函数介绍

ES6新增的函数写法

箭头函数的有点：

1. 写法更加简洁
2. 属于函数表达式，不存在函数提升

##### 6.2 箭头函数语法

- 基本语法：

```js
const fn = () =>{
	console.log('hello')
}
```

- 当只有一个形参时：

```js
// 可以省略参数的'()'
const fn = x =>{
    console.log(x)
}
```

- 当函数体只有一行时：

```js
// 1. 可以省略方法体的'{}'
const fn = x => console.log(x)
// 2. 可以省略return
const fun = x => x+2 
// 相当于 
const fun = x =>{
    return x+2
}
// 3. 可以直接返回一个对象，方法体使用'()'包裹对象
const f = name => ({a:name})
f('hello') //执行后返回的结果为一个对象，其属性a的值为‘hello’
```

##### 6.3 箭头函数的参数

箭头函数没有`arguments`动态参数，但是又`剩余参数`

##### 6.4 JS中的this

this是对象的自有变量，指向它的所属对象

关键问题：this的指向

this的指向取决于它被调用的位置

1. 默认绑定：在独立函数调用

   ```js
   function fn(){
   	console.log(this)
   }
   fn() // 打印结果为 window
   ```

   在函数调用this，指向的是调用函数的对象，在函数调用时，fn() 等价于 window.fn()，调用fn()的正是window对象，所有根据定义得出：指向window

   **注意**：在JS的[严格模式](https://www.runoob.com/js/js-strict.html)下，不允许默认绑定

   ```js
   "use strict"; // 严格模式
   function fun(){
   	console.log(this) // 打印输出为“undefined”
   }
   ```

   

2. 隐式绑定：调用位置是否上下文对象

   ```js
   function fn(){
   	console.log(this)
   }
   let obj = {
       fn
   }
   obj.fn //打印结果为obj
   ```

   obj对象中有fn方法，通过obj调用该方法，因为调用该方法的是obj，根据定义得出：指向obj

3. 显示绑定：使用call()、bind()、apply()

   前言：这个三个方法的作用

   - 可以调用函数，通过`方法.call()`调用，任何方法都能调用

   - 都是可以改变this的指向
   - 参数都是对象
   - **改变后的this都指向绑定后的对象**

   细讲:

   - call()：

     ```js
     function a(){
         console.log(this)
         console.log(this.name)
     }
     let b={
         name:'lele'
         eat(food){
            console.log("喜欢吃"+food) 
         }
     }
     a.eat.call(b,"面条") //打印结果为b “lele喜欢吃面条”
     ```

   - apply()：与call不同的点在于传参的方式，call是一个个穿，apply以数组的方式

     ```js
     function a(){
         console.log(this)
     }
     let b={
         name:'lele'
         eat(food){
            console.log("喜欢吃"+food) 
     }
     a.eat.apply(b,["鱼"]) //打印结果为b “喜欢吃鱼”
     ```

   - bind()：bind()和call()的不同点是bind()需要接收返回值，不能直接使用

     ```js
     function a(){
         console.log(this)
     }
     let b={
         name:'lele'
         eat(food){
            console.log("喜欢吃"+food) 
     }
     let fun = a.eat.bind(b,"牛肉")
     fun() //打印结果为b “喜欢吃牛肉”
     ```

4. new 绑定

   **`new` 运算符**创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。

   **`new`** 关键字会进行如下的操作：

   1. 创建一个空的简单 JavaScript 对象（即 **`{}`**）；
   2. 将步骤 1 新创建的对象作为 **`this`** 的上下文（改变this指向该新对象），并执行构造函数的代码（新对象添加属性）；
   3. 为步骤 1 新创建的对象添加属性 **`__proto__`**，将该属性指向构造函数的原型对象；
   4. 对构造函数返回值做判断
      1. 如果没有写返回值（return），默认返回新对象
      2. 如果return的是基本数据类型，忽略，而返回新对象
      3. 如果return的是引用数据（对象、数组），返回所写的数据
   
   
   ```js
   function Food(a,b,c){
   	this.a = a
       this.b = b
       this.c = c
       console.log(this) 
   }
   // 通过new得到一个新对象
   const food = new Food('牛肉','鱼肉','猪肉') // 打印为food
   
   ```

手撕**new**

```js
  function Hello(name,age){
      this.name = name;
      this.age = age;
      // return [1,23,3]
    }
    function create(fn,...arr){
      // 创建新对象
      var obj  = {};
      // 修改this指向，并指向构造函数
      var result = fn.apply(obj,arr);
      // 修改原型链(两种方法)
      // Object.setPrototypeOf(obj,fn.prototype); // 第一种
      obj.__proto__  = fn.prototype  // 第二种
      console.log('result',result);
      return result instanceof Object ? result : obj; // 判断Hello执行的返回值
    }
    var a = create(Hello,'小米1',18)
    console.log(a); // 打印Hello{ name: '小米1',age:18}
```



##### 6.5 箭头函数的this

箭头函数没有`this`，它会从自己的作用域链的上一层沿用this

注意：

- 当事件回调函数为箭头函数时，其this为window

##### 6.6 严格模式

在JS代码中加上

```js
'use strict'
```

就开启了严格模式

使用方式：

- 针对整个文件：

  在文件开头写上`'use strict'`

- 针对单个函数：

  函数内开头写上`'use strict'`

**严格模式的作用**

1. 对变量的声明严格要求：

   声明变量需要写关键字，否则报错

   ```js
   // 非严格模式
   a = 3   //  a = 3 等价于 window.a = 3
   console.log(a) // 3
   ```

   ```js
   // 严格模式
   'use strict'
   // 声明变量必须写关键字
   var a = 3
   b = 4 //会报错
   ```

2. 禁止了this的默认绑定，不允许this指向window

   ```js
   // 在函数作用域
   function fun(){
   	console.log(this) // 报错
   }
   ```

3. 禁止函数的参数重名

   ```js
   function fun(a,a,b){
   	// 该方法的声明会报错
   }
   ```

4. 动态参数`arguments`不允许赋值

   ```js
   function fun(){
   	arguments = 20 // 会报错
   }
   fun(1,2,3,4)
   ```

   

## 7. 解构赋值

##### 7.1 数组解构

数组解构是能把数组里的变量**快速批量地**赋值给**一系列变量**的简洁语法

````js
// 把arr里的所有单元值赋值给指定的一系列变量
const arr = [100,200,300] //定义一个数组
const [a,b,c] = arr // 进行解构
console.log(a,b,c)  //打印100 200 300
````

有几种情况：

- 数组的单元值少，变量多，超出的那一部分变量就未被赋值，所以未undefined

  ```js
  const [a,b,c,d] = [1,2,3]
  console.log(a) // 1
  console.log(b) // 2
  console.log(c) //3
  console.log(d) // undefined
  ```

- 数组的单元值多，变量少，那么剩余的单元值不会赋值
  解决：可以使用剩余参数

  ```js
  const [a,b,...arr] = [1,2,3,4]
  console.log(a) // 1
  console.log(b) // 2
  console.log(arr) // 数组[3,4]
  ```

- 按需求解构

  ```js
  // 当我们不需要使用第三数时，解构的第三个变量写空就行
  const [a,b, , d] = [1,2,3,4]
  console.log(a) // 1
  console.log(b) // 2
  console.log(d) // 4
  ```

- 解构多维数组

  ```js
  // 以二维数组举例
  const [a,b,[c,d]] = [1,2,[3,4]]
  console.log(a) // 1
  console.log(b) // 2
  console.log(c) // 3
  console.log(d) // 4
  ```

##### 7.2 对象解构

对象解构是能把对象里的属性**快速批量地**赋值给**一系列变量**的简洁语法

- 基本语法

```js
const obj = {
    name: 'lele',
    age: 18
}
// 解构的变量名要和对象的属性名相同
const {name , age} = obj
console.log(name,age) // 打印出lele 18
```

- 当遇到外部变量名和解构变量名有冲突时，可以重命名

```js
const obj = {
    name: 'lele',
    age: 18
}
const name = 'meimei' // 外部也有一个name
const {name:uname , age} = obj // 此时我们重新改名未uname
console.log(uname,age) // 打印出lele 18
```

- 解构数组对象

```js
const obj = [
  {
    name: 'lele',
    age: 18
  }
]
const [{name , age}] = obj
console.log(name,age) // 打印出lele 18
```

- 多级对象解构

```js
const obj = {
    name: 'lele',
    family:{
        mon:'mama',
        dad:'baba'
    }
    age: 18
  }
// 解构内部对象是需要写对象名
const {name,family:{mon,dad}, age} = obj
console.log(name,age) // 打印出lele 18
console.log(mon,dad) //  打印出mama baba
```



## 8. 匿名函数

定义：匿名函数就是没有函数名的函数

基本语法：

```js
(function(){
	console.log('hello')
})
```

如何执行匿名函数：写`()`

```js
// 这种写法会立即执行匿名，也叫做立即执行函数
// 立即执行函数就是声明一个函数并马上调用
(function(){
	console.log('hello')
})(); // 打印输出“hello”
```

匿名函数传参：

```js
(function(str){
	console.log(str)
})('hello world!'); // 打印输出“hello world!”
```

注意：匿名立即执行函数末尾需要加分号`;`

## 9 构造函数

定义：是一种特殊的函数，作用是用来**快速批量地**创建初始化对象

场景：当需要声明多个一样的函数时，我们可以把他们共有的属性封装成一个构造函数，使得创建对象更简便

约定：

- 构造函数的命名以大写字母开头
- 只能通过“new”来操作执行
- 构造函数内部无需写"return"，返回值即为创建的新对象
- 实例化时没有参数可以省略

```js
function Class(name,age,sex){
    this.name = name
    this.age = age
    this.sex = sex
}
const Lihua = new Class('李华',18,'男')
const WuHua = new Class('吴华',17,'男')
const Meier = new Class('美儿',18,'女')
```

## 10 常用的内置构造函数

引用类型：

- Object、Array、RegExp、Date等

包装类：

- String、Number、Boolean等

  ```js
  // 举例
  const str = 'hello'
  // 等价于，在底层进行封装
  const str = new String('hello')
  ```

1. Object

   作用：用于创建对象

   静态方法：

   - Object.key(obj)：用于获取实例对象的属性名，返回为数组
   - Object.value(obj)：用于获取实例对象的属性值，返回为数组
   - Objcet.assign(obj1,obj2)：可用于将obj2拷贝给obj1，也可给obj1追加上obj2中obj1没有的属性

2. Array

   作用：用于创建数组

   常用方法：

   - join()

     作用：将数组转换为字符串

     参数为数组元素分隔符，默认为‘，’

     ```js
     const arr = [1,2,3,4];
     console.log(arr.join()); // 1,2,3,4
     console.log(arr.join('')); // 1234
     console.log(arr.join('/')); // 1/2/3/4
     ```

   - push()

     作用：将一个或多个参数添加在数组**尾部**，返回添加后的数组的长度，**原数组发生改变**。

     ```js
     const arr = [1,2,3,4];
     const a = arr.push(7,8)
     console.log(a,arr); // 6 [1,2,3,4,7,8]
     ```

   - pop()

     作用：从数组**尾部**删除一个元素，返回这个被删除的元素，**原数组发生改变**。

     ```js
     const arr = [1,2,3,4];
     const b = arr.pop()
     console.log(b,arr); // 4 [1,2,3]
     ```

   - unshift()

     作用：将一个或多个参数添加在数组**头部**，返回添加后的数组的长度，**原数组发生改变**。

     ```js
     const arr = [1,2,3,4];
     const c = arr.unshift(-1,-2)
     console.log(c,arr); // 6 [-1,-2,1,2,3,4]
     ```

   - shift()

     作用：从数组**头部**删除一个元素，返回这个被删除的元素，**原数组发生改变**。

     ```js
     const arr = [1,2,3,4];
     const b = arr.shift()
     console.log(b,arr); // 1 [2,3,4]
     ```

   - slice(start,end)

     作用：截取数组元素

     参数：

     - 如果不传参数，会返回原数组；
     - 如果一个参数，从该参数表示的索引开始截取，直至数组结束，返回这个截取数组，原数组不变；
     - 两个参数，从第一个参数对应的索引开始截取，到第二个参数对应的索引结束，但包括第二个参数对应的索引上的值，原数组不改变

     ```js
     const arr = [1,2,3,4]
     const e = arr.slice()
     const f = arr.slice(2)
     const g = arr.slice(0,4)
     console.log(e,f,g);
     ```

   - splice(index,length,item1...itemX)

     作用：截断数组

     参数：

     - 一个参数，从该参数表示的索引位开始截取，直至数组结束，返回截取的 数组，原数组改变；
     - 两个参数，第一个参数表示开始截取的索引位，第二个参数表示截取的长度，返回截取的 数组，原数组改变；
     - 三个或者更多参数，第三个及以后的参数表示要从截取位插入的值。	

     ```js
     const arr1 = [1,2,3,4];
     const h = arr1.splice(2,2,345)
     console.log(h,arr1); // [3,4] [1,2,345]
     ```

   - reverse()

     作用：将素组倒序排列

     ```js
     const arr2 = [1,2,3,4]
     const i = arr2.reverse()
     console.log(i); // 4，3，2，1
     ```

   - find(callback())

     作用：查找数组中的元素，若找到返回该元素，找不到返回undefined

     ```js
     const arr3 = ['lxl','zml','zpj','lyw']
     console.log(arr3.find(item=>item == 'lxl1')); // undefiend
     console.log(arr3.find(item=>item == 'lxl')); // lxl
     ```

   - reduce(callback(prev,current),initial)

     作用：对数组中的所有元素调用指定的回调函数，该回调函数的返回值为累计结果。并且把返回值在下一次回调函数时作为参数提供String

   - 参数：回调函数中第一个参数为上一个累加的值，第二个为当前要遍历的元素，外面第三个参数为初始值，如果设定了初始值，会在一开始作为prev算入累加

     ```js
     const arr4 = [1,2,3,4,5]
     const result = arr4.reduce((prev,current)=>{
       return prev+current
     },0)
     console.log("reduce",result); // 15
     ```

     ```js
     // 如果遍历的是对象，第三个参数一定要加，设置为0即可
     const obj = [{age:1},{age:2},{age:3},{age:4}]
     const result2 = obj.reduce((prev,current)=>{
       console.log(prev,current.age);
       return prev+current.age
     },0) 
     console.log("reduce",result2); // 10
     ```

     

   作用：用于创建字符串

   常用方法：

3. Number

   作用：用于创建数字类型的变量

   常用方法：

   - toFixed(value)

     作用：设置保留几位小数（向上取整）

     ```js
     const sa = 11.115313
     console.log(sa.toFixed(2)); //11.12
     ```

     

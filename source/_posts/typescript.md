---
title: TypeScript学习
---
#### 1. TS使用

**安装TS**

```shell
> npm install -g typescript
```

**手动编译TS代码**

```shell
tsc hello.ts
```

之后就会在同级目录下面生成一个同样名字的 `.js` 文件

**自动编译TS代码**

使用 `tsconfig.json` 配置文件

```shell
tsc --init
```

使用该命令自动生成配置文件，之后：

- 修改 `outDir` 属性（确定输出路径）
- 修改 `stric:false` （关闭严格模式）

最后：终端 -> 运行任务 -> 显示所有任务 -> tsc:监视

#### 2. 类型声明

对变量、参数的类型进行定义，**作用**：能对变量的类型进行**自动检查**，如果不符合，就会报错

**声明number类型的变量**

```tsx
let a:number = 10 
```

**声明布尔类型的变量**

```tsx
let flag:boolean = true
```

**声明String类型的变量**

```tsx
function hello (str:string){ // 声明传入的形参只能是String类型的值
    console.log("hello"+str) 
}
```

**声明数组Array类型**

```tsx
let arr1:number[] = [1,2,3] // 定义数组，且元素只能是number类型的

let arr2:Array<number> = [1,2,3] // 使用“泛型”的方法
```

**声明对象类型的变量**

``` tsx
let ob:object = {} 
```

**声明任意类型的变量**

```tsx
// 当不知道给变量确定什么类型是，定义为any
let a:any = 1
a = 'hello' 
a = [1,2,3] 
```

**声明函数没有返回值**

```tsx
function hello:void (){
    console.log('hello world')
}
concole.log(hello()) // 会看到先打印 'hello world' 然后是 'undedined'
```

**联合类型声明**

一个变量可以定义为多个类型

```tsx
lef a:boolean | number | string = true
// 这样这个a就既可以为布尔值、又可以为number、又可以为string
a = 123
a = 'str'
a = false
```

**注意**：联合类型的变量没有确定为哪一个类型时，定义的变量只能使用所有类型共有的属性或方法

#### 3. 接口

接口是对**对象的约束**，描述了对象的形状

```tsx
// 接口的名称首字母要大写
interface Person {
    name:string  // 定义对象属性的类型
    age:number
    sex:String
}
// 使用接口对对象进行约束
let obj:Person = {
    name:'lxl' // 必须是string
    age:18 
    sex:'男 '
}
```

注意：

- 当接口中需要定义一些[可有可无的属性](有的对象需要有，有的对象不需要有)时，可以运用**可选属性**

```tsx
interface Person {
    name:string  
    age:number
    sex?:string //可选属性
}
let obj:Person = {
    name:'lxl' // 必须是string
    age:18 
    //sex:'男 ' 不写sex也不会报错
}
```

- 当不希望定义好的属性被修改，可使用**只读属性**

```tsx
interface Person {
    readonly name:string  
    age:number
    sex?:string //可选属性
}
let obj:Person = {
    name:'lxl' // 必须是string
    age:18 
    //sex:'男 ' 不写sex也不会报错
}
obj.name = 'zml' // 是不可以修改的！！！
```

- 接口也可以对函数进行约束

```tsx
interface Ifunc{
    // (参数1:类型,参数2:类型...):返回值类型
    (a:string,b:number):boolean
}
const Hello:Ifunc = function(a:string,b:number):boolean{
    return true
}
```

#### 4. 函数

**函数声明**

```tsx
function add(a:number,b:number):number{
    return(a+b)
}
```

**函数表达式声明**

```tsx
let add = function(a:string,b:string):number{
    return a+b
}
```

**TS中完整的写法**

```tsx
// 类似于接口
let add:(a:number,b:number)=>number = function(a:number,b:number):number{
    return a+b
}
```

#### 5. 类型断言

**断言的方式**

- 变量 as 类型

  ```tsx
  function add(a:string |number,b:number):number{
      return (a as number) + b
  }
  console.log(add(1,2)) // 3
  ```

- <类型> 变量

  ```tsx
  function add(a:string |number,b:string):string{
      return <string> a  + b
  }
  console.log(add(1,'2')) // 12
  ```

**断言的应用**

- 将一个联合类型断言为其中一个类型
- 将一个具体类型断言为`any`类型，从而访问任何属性和方法
- 将`any`断言为一个具体的类型

#### 6. 类型别名

给类型定义一个新的名称

```tsx
type s = string
let str:s = 'lxl' //s就代替了string
```

也可以代替多类型

```tsx
type all = number | string | boolean
let a:all = true
a = 1;
a = 'lxl'
a = false
```

还可用进行字符串变量字面量约束

```tsx
type Name = 'lxl' | 'zml' | 'zz'
let a:Name = 'lxl' // a只能为'lxl' | 'zml' | 'zz'的其中一个
```

#### 7. 元组

元组（Tuple）合并了不同类型的

```tsx
let Tarr = [boolean,string] = [true,'lxl'] // 定义的时候按照顺序来赋值
Tarr.push('zml')
Tarr.push(fase) //插入元素只要是其中一种类型都可以
```

#### 8. 枚举

##### 8.1 普通枚举

**基本写法**：通过使用`enum`关键字来定义枚举

```tsx
enum Day{
  one,
  two,
  three,
  four
}

console.log(Day);
```

**枚举是一个对象**，其内部是一个`key:value`的相互映射关系

编译的结果为

```tsx
var Day;
(function (Day) {
    Day[Day["one"] = 0] = "one";
    Day[Day["two"] = 1] = "two";
    Day[Day["three"] = 2] = "three";
    Day[Day["four"] = 3] = "four";
})(Day || (Day = {}));
console.log(Day);
```

内部属性的`index`默认从`0`开始，`index`是可以手动赋值的，**后面的属性的`index`会根据前一个进行`+1`**

**输出结果为**

```tsx
// 属性'one'会对应1，1又会对应'one'
{
  '0': 'one',  
  '1': 'two',  
  '2': 'three',
  '3': 'four', 
  one: 0,      
  two: 1,      
  three: 2,    
  four: 3
}
```

**注意：**如果`index`相同，后面的会覆盖前面的

```tsx
enum Day{
  one=3,
  two=1,
  three,
  four
}

console.log(Day);
```

输出为：

```tsx
// 可以看出，'four'把'one'覆盖了
{
  '1': 'two',
  '2': 'three',
  '3': 'four',
  one: 3,
  two: 1,
  three: 2,
  four: 3
}
```

所以需要尽量避免该情况

##### 8.2 常数枚举

常数枚举使用`const enum` 来定义

```tsx
const enum Arr{
  a,
  b,
  c
}
console.log(Arr.a);
console.log(Arr.b);
console.log(Arr.c);
```

编译结果为

```tsx
console.log(0 /* Arr.a */);
console.log(1 /* Arr.b */);
console.log(2 /* Arr.c */);
```

**它的特点如结果所见**，常数枚举在编译阶段会被删除，只输出结果，后面跟注释告诉结果出自哪

##### 8.3 外部枚举

外部枚举用于声明文件，使用`declare`来声明

```tsx
declare enum Obj{
  o,
  b,
  j
}
console.log(Obj.o);
```

编译结果为：

```tsx
console.log(Obj.o); //并不会输出结果
```

#### 9. 类

#### 10. 泛型

**泛型**是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性

```tsx
// T：表示任意输入类型，当传进来参数时，再去判断参数类型
// T 是任意的一个名字而已
function getArr<T>(value:T,count:number):T[]{
	const arr:T[]=[]
    for(let i=0;i<count;i++){
        arr.push(value)
    }
    return arr
}

console.log(getArr(3,3)) // [3,3,3]
```

**多个泛型参数**

```tsx
// 实现元组项的反转
function reverseArr<T,U>(arr:[T,U]):[U,T]{
    return [arr[1],arr[0]]
}
console.log(reverseArr<number,string>(12,'lxl')) // 'lxl' 12
```

**泛型约束**

规定泛型中，必须要包含某个确定的类型

```tsx
interface ILength{
    // 所有使用接口来对泛型进行约束，规定T泛型必须有length属性，且为number
    length:number
}

function getLength<T extends ILength>(X:T):number{
    return x.lentgth // 按照原有的来讲，传进来是泛型的参数不确定类型 ，不能调用一个确切类型的方法
}
```

**泛型类**

```tsx
class Person<T>{
    name:string
    age:T
    construcotr(name:string,age:T){
        this.name = name 
        this.age = age
    }
}
// 使用时指定类型为string
const p1 = new Person<string>("lxl",'18')
const p2 = new Person<number>("lxl",18)
```

**泛型接口**

```tsx
// 泛型接口
interface IArr{
    // 对函数进行约束
    <T>(value:T,count:number):T[]
}
    
let getArr:IArr = function <T>(value:T,count:number):T[]{
        const arr:T[]=[]
    for(let i=0;i<count;i++){
        arr.push(value)
    }
    return arr
    }
```


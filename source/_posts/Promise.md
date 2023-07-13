---
title: Promise技术  
---
    
### 1 什么是Promise  
  1、Promise是一门新的技术(ES6规范)  
  2、Promise是JavaScript中解决`异步编程`的新方案(旧方案是使用`回调函数`)  
  ```
  - 回调函数：()=>{}    
  - 异步编程：一个程序的执行将不再与原有的序列有顺序关系，谁执行的快，谁就先完成，不必按顺序等待执行  
  - 举例：
    1、fs 文件操作：  
    require('fs').readFile('文件url',(err,data)=>{})
    2、数据库操作  
    $.get('url',(data)=>{})
    3、定时器  
    setTimeout(()=>,限制时间)
  ```
  3、Promise是一个构造函数，返回的是一个Promise对象，可以使用该对象获取成功或者失败的结果值,能够更好的执行异步编程  
### 2 Promise的好处  
1、指定回调函数的方式更灵活  
- 旧方法：必须在异步任务前指定回调函数  
- Promise：启动异步任务=>返回Promise对象=>给Promise对象绑定回调函数  

2、支持链式调用，可以解决`回调地狱`  
  回调地狱：  
    ![回调地狱](./IMG//%E5%9B%9E%E8%B0%83%E5%9C%B0%E7%8B%B1.png)  
  回调地狱的缺点：  
- 不易阅读  
- 不便于异常处理  

### 3 Promise初体验  
```  
//创建Promise对象  
//该对象有两个参数resolve和reject，是两个方法
const p = new Promise((resolve,reject)=>{
  //进行异步操作
  //对异步操作结果进行处理
  if(){
    resolve(data);
  }
  else{
    reject(data);
  }
})

//使用该对象的then方法,处 理data数据
p.then(
  //第一个回调是resolve()，形参value是上面传来的参数data
  (value)=>{},
  第二个回调是reject()，形参reason是上面传来的参数data
  (reason)=>{})
```  
### 4 Promise的状态  
Promise有一个内置属性：`PromiseState` 有三种状态：  
- pending：未决定的  
- fulfilled(resolve)：成功  
- reject：失败  

Promise对象状态的转换只有两种,且**PromiseState只会转换一次**：
- pending => resolve  
- pending => reject  

### 5 Promise的构造函数  
1、then方法：用于处理 Promise 成功状态的回调函数  
2、catch方法：用于处理 Promise 失败状态的回调函数  
`Promise在实例化对象后，可以用then方法处理resolve()和reject(),就相当于结合then和catch变成新的then`  
3、finally方法：无论 Promise 是成功还是失败，都会执行的回调函数

### 6 Promise函数对象的方法  
这里所有的方法都是针对Promise对象，都是为了快速得到Promise对象(**不是实例化的Promise对象**)  
1、Promise.resolve()方法  
返回一个新的 Promise 对象，且它的状态为fulfilled  
2、Promise.reject()方法  
返回一个新的Promise 对象，该对象的状态为rejected  
3、Promise.all()方法  
该方法接收参数为Promise对象数组，假设有数组[p1,p2,p3]  
- 只有p1，p2，p3都状态为fulfilled,返回的Promise对象状态为fulfilled  
- 只要p1、p2、p3之中有一个被rejected，返回的Promise对象状态为rejected  

4、Promise.race()方法  
该方法接收的参数也是Promise对象数组，假设有数组[p1,p2,p3]  
- 在数组中谁先改变状态，该方法返回的对象的状态就是先改变的状态  
  **例如：p1先变成reject，那么返回的状态就为reject(race也是比赛的意思)**  

### 7 Promise关键问题  
- 改变Promise对象的状态  
  - resolve(成功)  
  - reject(失败)  
  - throw err(抛出错误)  

- Promise指定多个回调函数，都会执行吗  
当Promise对象状态改变时，Promise才会执行对应状态的回调  
  
- 改变Promise状态和指定回调函数谁先谁后  
  - 都有可能，正常情况下是指定回调在改变状态，但也可以先改变状态再指定回调(可以通过异步编程实现，比如setTimeout)  
  - 如何先改变再指定回调  
    - 在执行器中调用resolve()/reject()  
    - 延迟更长的时间才调用then()(设置时延setTimeout)  
  - 什么时候才能得到数据(就是要先改变状态才会得到数据)  
    - 如果先指定回调，当状态发生改变时，回调函数就会调用，得到数据  
    - 如果先改变的状态，当指定回调时，回调函数就会调用，得到数据  
  
- Promise.then()返回的新Promise状态由什么决定  
  - 简单表达：由then()指定的回调函数执行的结果决定  
  - 详细表达：  
    - 如果抛出异常，新Promise为reject，reason为抛出的异常  
    - 如果返回的是非Promise的任意值，新Promise为resolve，value为返回的值  
    - 如果返回一个新Promise，此Promise的结果就会成为新Promise的结果  

- Promise串联多个任务、异常击穿、中断Promise链  
  1、串联  
  Promise的then返回新的Promise，然后新的Promise的then又可以调用新的Promise的状态执行方法  
  例子：  
  ```
  const s = new Promise((resolve,reject)=>{  
      resolve('OK');  
      }).then(value => {  
      return Promise.resolve(value)  
      }).then(value => {  
      console.log(value);  
      })  
  ```  
  本例中，第一个Promise对象s调用then方法，返回的Promise是resolve，其状态是`成功`，新的Promise调用then方法，自然是能够调用`成功`的resolve，然后我又`return`了一个resolve的Promise，新的Promise又使用then方法，也是调用resolve，最终结果时输出“OK”(反正就是套娃子)  

  2、异常击穿  
  例子：  
  ```  
  const s = new Promise((resolve,reject)=>{  
      reject('error');  
      }).then(value => {  
      return Promise.resolve(value)  
      }).then(value => {  
      console.log(value);  
      }).catch(reason => {
        console.log(reason)
      })  
  ```  
  本例最开头的Promise对象返回的是reject的新Promise，所有后面的关于resolve的回调都不会调用了，最后的catch执行reject的回调，这就是`异常击穿`：当使用Promise去带哦用then方法的时候，可以在最后指定失败的回调  

  3、中断Promise链  
  使用一个状态为`pending`的Promise去中断，这是因为只有状态改变的时候才会进行回调。  
  例子：  
  ```  
  const s = new Promise((resolve,reject)=>{
            reject('error');
        }).then(value => {
            return new Promise(()=>{});
        }).then(value => {
            console.log(111);
        }).then(value => {
            console.log(222);
        }).catch(reason => {
            console.log(reason);
        })
  ```  
  本例中插入了`return new Promise(()=>{})`，返回一个状态为`pending`，使得下面的then都不会执行，结果不会打印`111`和`222`
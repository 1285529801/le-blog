---
title: AXIOS技术  
---
### 1. 什么是AXIOS  
---
Axios 是一个基于 `promise` 网络请求库，作用于`node.js` 和`浏览器`中，在服务端它使用原生 `node.js http 模块`， 而在客户端 (浏览端) 则使用 `XMLHttpRequests`  

### 2. AXIOS的特性  
---
- 从浏览器创建 <a href="https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest">XMLHttpRequests</a>
- 从 node.js 创建 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防御XSRF  

### 3. async和await  
---
- async：async是一个加在函数前的修饰符，被async定义的函数会默认返回一个Promise对象resolve的值  
- await：await 也是一个修饰符，只能放在async定义的函数内。可以理解为`等待`，await 修饰的如果是Promise对象：可以获取Promise中返回的内容（resolve或reject的参数），且取到值后语句才会往下执行；如果不是Promise对象：把这个非promise的东西当做await表达式的结果。  
- async和await结合的例子：
    ```  js
    function getSomeThing() {
                    return new Promise((resolve, reject) => {
                        setTimeout(() => {
                            resolve('获取成功')
                        }, 3000)
                    })
                }
    
            async function test() {
                    let a = await getSomeThing();
                    console.log(a)
                }
                test();  
    ```
    输出结果：“获取成功”  

### 4. AXIOS的基本实例  
---
- GET方法  
```  js
    axios({
    //请求类型
    method: 'GET',
    //url
    url: '',
    }).then(response =>{
        console.log(response)
    }).catch((e)=>{
        //输出异常
        console.log(e)
        })  
```
- POST方法  
```  
axios({
    //请求类型
    method: 'POST',
    //url
    url: '',
    }).then(response => {
        console.log(response)
    }).catch((e) => {
        //输出异常
        console.log(e)
        })
```
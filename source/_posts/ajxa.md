---
title: Ajax技术
---  
AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

AJAX 最大的<b>优点</b>是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。  
```  
1. 创建XMLHttpRequest对象  
  const xhr = new XMLHttpRequest();  
2. 初始化：设置请求方法和url  
  xhr.open('请求方法','url');  
3. 发送  
  xhr.send();  
4. 事件绑定 处理服务器返回的结果  
  xhr.onreadystatechange = function(){
    //readystate有5种状态：  
    // 0 : 未初始化
    // 1 : 正在加载 
    // 2 : 加载完毕
    // 3 : 正在交互
    // 4 : 完成  
    if(xhr.readyState === 4){
        //2XX表示成功
        if(xhr.status >=200 && xhr.status < 300){
          console.log(xhr.response) //响应体
          console.log(xhr.responseText) //返回文本
        }
        else{
            //不成功
        }
    }
  }
```
### 1. Axios 的 Ajax  
---
- 配置baseURL  
  配置这个可以简化请求时要填写的url，比如设置为‘http://127.0.0.1:8000’，在后面的请求中，就无需多写前面的‘http://127.0.0.1:8000’了，直接写后面的url。
- GET方法  
  ` axios.get('url',{`  
    `//设置传送参数`   
    `params：{}`
    `}).then((response)=>{`  
    `//处理返回的响应`  
    `console.log(response)`
    `})`  
- POST方法  
```
 axios.post('url',{  
    //设置请求体  
    数据1,  
    数据2
    },{  
    //设置传送参数   
    params：{},  
    //设置请求头  
    headers:{},
    }).then((response)=>{  
    //处理返回的响应  
    console.log(response)
    })  
```

- AXIOS通用方法  
```  
axios.get(  
  //设置请求方法  
  method:'POST'或者'GET'  
  //设置url  
  url:''`,  
  //设置传送数据 
  params：{},  
  //设置请求头  
  headers:{},  
  //设置请求体  
  data:{}  
  ).then((response)=>{  
    //处理返回的响应  
    console.log(response)
    })    
```

### 2. 解决跨域问题  
---
- **jsonp**  
  jsonp是互联网工作者自己发明的解决跨域的方法，并非官方提出：因为script本身就具有跨域属性，所以使用srcipt标签的src属性能像服务端发送请求，也是将src填上请求的url，来访问后端，就能够实现跨域请求数据。  
  html文档中  
  ![jsonp2](../image/jsonp2.png)
  服务器端中  
  ![jsonp](../image/jsonp.png)  
  需要注意的是，服务端返回的是字符串，所以我们在服务器端应该把response设置为javascript代码的字符串，前端接收到，就会执行代码，调用方法  
  
- **CORS**  
  CORS(cross-origin resource sharing),跨域资源共享，是官方给出的解决跨域的方法，CORS不需要在客户端做任何操作，完全在服务端处理。  
  CORS通过在服务端设置**响应头**    
  
  ```
   response.setHeader("Access-Control-Allow-Origin","url")  
  其中url可以选择写通配符 **“*”**  
   response.setHeader("Access-Control-Allow-Origin","*")  
  这样就可以适配所有页面  
  ```
  一般的配置是  
  ```
  response.setHeader("Access-Control-Allow-Origin","*")  
  response.setHeader("Access-Control-Allow-Headers","*")  
  response.setHeader("Access-Control-Allow-Method","*")
  ```
### 3. 取消请求  
---
使用XMLHttpRequest对象的`abort()`函数就可以取消http请求
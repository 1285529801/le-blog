---
title: JS学习
---

## 1. 语法  
---
建议在每一条语句后加上 **";"**  
### 1.0javascript的5种原始数据类型  
- Undefined  
- Null  
- Boolean  
- Number  
- String  
### 1.1字符串  
---
必须明确声明的语言称为强类型语，javascript是弱类型不需要明确声明 
字符串必须包括在引号里 **""** 或 **''** 都可以  
> var str = "happy"  
> var str = 'happy'  
### 1.2数值  

---
允许任意数值（正负数、小数），也称作 **浮点数**  
### 1.3数组  
---
声明时带长度  
> var array = Array(数组长度)  

声明时直接赋值  
> var array = Array("a","b","c")  

或者不用明确声明是数组  
> var array = ["a","b" ,"c"]  
也可以是数字  
> var array = [12,13,14]  

### 1.4 var，let，const三者的特点和区别  
---
- var的特点  
  - 存在变量提升  
  - 一个变量可多次声明，后面的声明会覆盖前面的声明  
  - 在函数中使用var声明变量的时候，该变量是局部的，而如果在函数内不使用var，该变量是全局的  
- let的特点  
  - 不存在变量提升，let声明变量前，该变量不能使用（暂时性死区）  
  - let命令所在的代码块内有效，在块级作用域内有效
  - let不允许在相同作用域中重复声明，注意是相同作用域，不同作用域有重复声明不会报错  
- const的特点  
  - const声明一个只读的变量，声明后，值就不能改变  
  - const必须初始化  
  - const并不是变量的值不能改动，而是变量指向的内存地址所保存的数据不得改动
  
## 2. DOM  
---
- XML DOM（XML Document Object Model）定义了访问和操作 XML 文档的标准方法。

- XML DOM 把 XML 文档作为树结构来查看。

- 所有元素可以通过 DOM 树来访问。可以修改或删除它们的内容，并创建新的元素。元素，它们的文本，以及它们的属性，都被认为是节点。  
### 2.1 节点  
---
<b>一份文档就是一颗节点树</b>  
节点分为：  
- 元素节点    
- 属性节点  
- 文本节点  

### 2.2 获取元素  
---
- getElementById  
该方法返回一个对象
> document.getElementById("id名称")  
- getElementsByTagname  
该方法可返回带有指定标签名的对象的集合(数组)
> document.getElementsByTagname("HTML标签")  
- getElementByClassName  
该方法返回一个具有相同类名的元素的数组  
> getElementByClassName("类名")    
- querySelectorAll  
querySelectorAll() 方法返回文档中匹配指定 CSS 选择器的所有元素，返回 NodeList 对象。NodeList 对象表示节点的集合。可以通过索引访问，索引值从 0 开始。  
> querySelectorAll( ) //参数可以是标签名、选择器名  

 **getElementsByTagName()比querySelectorAll()的区别**  
 - 虽然两者返回的都是元素集合  
    getElementsByTagName()返回HTMLCollection集合  
    querySelectorAll()返回NodeList集合  
 - 区别：querySelectorAll()是静态的，getElementsByTagName()是动态的，所以getElementsByTagName性能较好
### 2.3 获取和设置属性  
---
- getAttribute  
该方法需要通过document对象来调用，可以配合getElementById<b>等</b>方法来使用  
> var parm = document.getElementById("id名称") //创建了document对象  
parm.getAttribute("属性名称")//使用该方法  

**注意：当获取的是中间带减号的CSS属性时，要求使用驼峰命名法**  
> b比如获取font-family时:  element.style.fontFamily
- setAttribute  
也是通过document对象调用，用来修改属性值  
> var parm = document.getElementById("id名称") //创建了document对象  
parm.setAttribute("属性名称",属性值)//使用该方法  

### 2.4 nodetype属性  
---
- 元素节点的nodetype属性值为：1
- 属性节点的nodetype属性值为：2
- 文本节点的nodetype属性值为：3

### 2.5 childNodes属性  
---
```
childNodes属性用来获取 **节点树** 上任何一个元素的所有子元素，它是一个包含一个元素的全部子元素的数组
```
- firstChild描述的是一个元素的子元素的第一个子元素  
> node.firstChild === node.childNodes[0]  
- lastChild描述一个元素的子元素的最后一个子元素  
> node.lastChild === node.childNodes[node.childNodes.length-1]
### 2.7 nodeValue属性  
---
该属性描述设置或返回指定节点的节点值  
**如果您希望返回元素的文本，请记住文本始终位于文本节点中，并且您必须返回文本节点的值（element.childNodes[0].nodeValue）**  
### 2.8 createElement方法  
---
    createElement() 方法通过指定名称创建一个元素  
    > var 变量 = document.createElement("标签名")
### 2.9 appendChild方法  
---
    appendChild() 方法可向节点的子节点列表的末尾添加新的子节点。
### 2.10 createTextNode方法
---
    createTextNode() 可创建文本节点。  

三个方法配合使用  
```
function myFunction()
{  
    var h=document.createElement("H1");  
    var t=document.createTextNode("Hello World");  
    h.appendChild(t);  
    document.body.appendChild(h);  
}
```
### 2.11 insertBefore() 方法  
---
    insertBefore() 方法可在已有的子节点 前 插入一个新的子节点。  
    >  node.insertBefore(新元素,目标元素)  
    新元素：想要插入的元素  
    目标元素：想把新元素插到哪个元素之前  

  



## 3、DOM事件  
DOM事件流（event  flow ）存在三个阶段：`事件捕获阶段`、 `处于目标阶段`、 `事件冒泡阶段`。  
- 事件捕获（event  capturing）：通俗的理解就是，当鼠标点击或者触发dom事件时，浏览器会从根节点开始由外到内进行事件传播，即点击了子元素，如果父元素通过事件捕获方式注册了对应的事件的话，会先触发父元素绑定的事件。  
- 事件冒泡（dubbed  bubbling）：与事件捕获恰恰相反，事件冒泡顺序是由内到外进行事件传播，直到根节点。  
- dom标准事件流的触发的先后顺序为：先捕获再冒泡，即当触发dom事件时，会先进行事件捕获，捕获到事件源之后通过事件传播进行事件冒泡。  

---
## 4、 正则表达式  
`*`：出现零到多次  
`+`： 出现一到多次  
`?`：出现零次或者一次  
`.`: 除了\n以外的任意字符  
`{n}`: 出现n次  
`{n,}`: 出现n到多次  
`{n,m}`: 出现n到m次    

---

## 3、 数组的常用方法 

1. **join()方法**

   作用：将`数组`转换为`字符串`  

   参数：任意输入作为数组的分隔符，默认为逗号

2. **push()方法**

   作用：向数组末尾添加一个或多个元素，返回新的长度

   参数：`值`

3. **pop()方法**

   作用：删除数组最后一个元素并返回该元素

4. **shift()方法**

   作用：删除数组第一个元素并返回该元素

5. **unshift()方法**

   作用：向数组头部添加一个或多个元素，返回新的长度

   参数：`值`

6. **sort()方法**

   作用：用于给数组排序

   默认情况下，`sort()` 方法将按字母和升序将值作为字符串进行排序

7. **reverse()方法**

   作用：将数组反转

8. **concat()方法**

   作用：链接两个或多个数组

   ```js
   var arr=[1,2,3,4];
   	var arr2=[11,12,13] 
   	var arrCopy = arr.concat(arr2);
   	console.log(arrCopy); // [1, 2, 3, 4, 11, 12, 13]
   ```

9. **slice()方法**

   作用：用于数组截取

   参数：1. start(可选)：规定从哪开始截取 

   ​           2. end(可选)：规定从哪结束截取

10. **splice()方法**

    作用：在数组中添加/删除指定项目，返回被删除的项目

    语法：

    ```
    arr.splice(index , howmany , item1,.....,itemX)
    ```

    

    参数：1. **index**：必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置

    ​           2.**howmany**：必需。要删除的项目数量。如果设置为 0，则不会删除项目

    ​           3. **item1, ..., itemX**：可选。向数组添加的新项目


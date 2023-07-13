---
title: Vue学习  
---
# vue2的学习

## 1、 什么是Vue  
`Vue` 是一款用于构建用户界面的 JavaScript 框架。它基于标准 HTML、CSS 和 JavaScript 构建，并提供了一套声明式的、组件化的编程模型，帮助你高效地开发用户界面  
使用`vue-cli`创建vue项目
使用指令下载vue-cli
```shell
npm install -g @vue/cli      
```
创建项目  
```  
vue create xxx
```
启动项目  
```  
cd xxx
npm run serve
```
---
## 2、MVVM 模型  

  - M：模型(model)指data中的数据  
  - V：试图(view)指模板代码  
  - VM：视图模型(view model)指vue实例  
---
## 3、 Vue的相关指令  

- `"v-bind"` 用于绑定数据，但是是单向绑定，只能是数据只能从后台流向页面进行更新，`"v-bind"`的简写是 `":"`  
    > v-bind:value = "要绑定的数据"    
    > :value = "要绑定的数据"  

- `"v-model"` 用于绑定数据，且是双向绑定，既可以从后台流向页面，页面也可以传回后台，**v-model指令只能应用在表单类元素上**  ，由于v-model绑定的只能是“value”，所有`"v-model:value ="`可以简写为`"v-model ="`
    > v-model:value = "要绑定的数据"    
    > v-model = "要绑定的数据"    

- `"v-on"` 用于事件绑定，监听 DOM 事件，`"v-on"`的简写是 `"@"`  
    > v-on:click = "方法名" //点击时调用方法  
    > @click = "方法名" //点击时调用方法
---
## 4、 Vue中的数据代理  

  - 数据代理定义  
    通过一个对象代理另一个对象中的属性的操作（读 / 写 ）  
  - Vue数据代理的原理  
    原理图：
    ![Vue数据代理](./IMG/%E6%95%B0%E6%8D%AE%E4%BB%A3%E7%90%86.png)  

    1、Vue中的数据代理：
        通过 vm 对象来代理 data 对象中属性的操作（读 / 写 ）  
    2、Vue中数据代理的好处：
        更加方便地操作data中的数据，Vue实例的数据存在 `"_data"` 之中  
    3、基本原理：  
        （1）通过 Object.defineProperty()给 vm 添加与 data 对象的属性对应的属性描述符  
        （2）所有添加的属性都包含 getter/setter  
        （3） getter/setter 内部去操作 data 中对应的属性数据  
        （4）最终效果就是可以使用 `"vm.数据"` 实例直接操作data的数据  
---
## 5、 数据修饰符  

  - .stop：阻止事件冒泡(常用)  
  - .prevent：阻止默认事件(常用)  
  - .capture：使用事件的捕获模式  
  - .self：只有event.target是当前操作元素才触发事件  
  - .once：事件只触发一次(常用)  
  - .passive：事件的默认行为立即执行，无需等待事件回调执行完毕  
---
## 6、 键盘事件  

1、Vue中常用到的按键名：  
- 回车 => enter  
- 删除 => delete(捕获"删除"和"退格")  
- 退出 => esc  
- 空格 => space  
- 换行 => tab  
- 上 => up  
- 下 => down  
- 左 => left  
- 右 => right  
  

在事件中可以通过 `"keyup"` 或者 `"keydown"` 配合按键名称来定义当按下某个键盘时要触发的事件( `"keyup"` 是键盘按键起来时 `"keydown"` 是键盘按键按下时)  

---
## 7、 计算属性 computed  
- 计算属性的定义：要用的属性不存在，要通过已有的属性计算得来  
- 计算属性的原理：底层借助 `Object.defineproperty` 方法提供的 `getter` 和 `setter`  
- get函数什么时候会执行：  
   (1) 初次读取时会执行一次  
   (2) 当依赖的数据改变时会被再次调用  
- 优点：有缓存机制，效率更高，调试方便  
- 计算属性最终会出现在Vue实例上，直接调用即可  
- 如果计算属性要修改，那必须写set函数去响应修改，且set中要要引起计算时依赖的数据发生改变  
- 例子(完整写法)：  
    ```  
    computed: {
        fullName: {
            // getter方法
            get: function () {
            return this.firstName + ' ' + this.lastName
            },
            // setter方法
            set: function (newValue) {
            var names = newValue.split(' ')
            this.firstName = names[0]
            this.lastName = names[names.length - 1]
            }
        }
    }  
    ```
- 例子(简写写法，当计算属性只需要读取不需要修改时)：  

    ```  
    computed: {
        fullName: function () {
        // `this` 指向 vm 实例
        return this.firstName + ' ' + this.lastName
        }
    }
    ```
---
## 8、 监视属性 watch  
- 当被监视的属性变化时，回调函数自动调用，进行相关操作  

- 监视的属性必须存在，才能进行监视  

- 监视的两种写法  
  - `new Vue` 时写入 `watch` 配置  
    ```  vue
    watch: {
        //如果 `firstName` 发生改变，这个函数就会运行  
        firstName: function (val) {
        this.fullName = val + ' ' + this.lastName
        },
        //如果 `lastName` 发生改变，这个函数就会运行
        lastName: function (val) {
        this.fullName = this.firstName + ' ' + val
        }
    }
    ```
  - 通过vm.$watch监视  
    ```  vue
    vm.$watch('firstName', function (newVal, oldVal) {
        // 做点什么
    })
    ```
  
- 深度监视  
    （1） Vue中的 `watch` 默认不监测对象内部值得改变(一层)  
    （2） 配置 `deep:true` 可以监测对象内部值的改变(多层)  
    
- 立即监视

    （1） 配置 `immediate:true`，这样数据在没变化之前也会被监视
---

## 9、`computed` 和 `watch` 之间的区别  
  （1）`computed` 能完成的功能，`watch` 都能完成  
  （2）watch 能完成的功能，`computed` 不一定能完成，例如：`watch` 能进行异步操作  
  两个小原则：  
  （1）所有被Vue管理的函数，最好写成普通函数，这样 `this` 的指向才是Vue实例对象 `vm`  
  （2）所有不被Vue所管理的函数（定时器回调函数、ajax回调函数），最好写成箭头函数，这样 `this` 的指向才是Vue实例对象 `vm`  

---
## 10、条件渲染  
- `v-if`    
    写法:  
    （1）v-if = "表达式"  
    （2）v-else-if = "表达式"  
    （3）v-else = "表达式"  
    特点：  
    （1）`v-if` 适用于显示切换比较低的场景  
    （2）不展示的DOM元素在源码中会被移除，如果频繁切换，会降低效率  
    （3）`v-if` 可以和 `v-else-if` 、`v-else` 一起使用，但是结构不能被打断，且 `v-else` 后面不需要添加条件(基本常识)  
- `v-show`    
    写法：  
    （1）v-show = "表达式"  
    特点：  
    （1）`v-show` 适用于显示切换频率较高的场景  
    （2）不展示的DOM元素未被移除，仅仅是使用css样式隐藏掉(`display:none`)  
    （3）使用 `v-if` 的时候元素可能无法获取到，使用 `v-show` 时一定能获取到  

---
## 11、面试题：react、vue中的key有什么用(key的内部原理)  
- 虚拟DOM中key的作用：  
    key是虚拟DOM对象的标识，当状态的数据发生变化时，Vue会根据`【新数据】`生成`【新的虚拟DOM】`，随后Vue进行`【新虚拟DOM】`与`【旧虚拟DOM】`的差异比较，比较规则如下：  
    （1）旧虚拟DOM中找到了与新虚拟DOM相同的key：  
    - 若虚拟DOM中内容没有改变，直接使用之前的真实DOM  
    - 若虚拟DOM中内容改变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM  
---
## 12、Vue监视数据的原理
1. vue会监视data中所有层次的数据。  
2. 如何监测对象中的数据?  
- 通过`setter`实现监现，且要在new Vue时就传入要监测的数据。  
    1. 对象中后追加的属性，**Vue默认不做响应式处理**  
    2. 如需给后添加的属性做响应式，请使用如下API:
    > vue.set(target.propertyName/index,value)  
    > vm.$set(target.propertyName/index,value)  

3、如何监测数组中的数据?  
- 通过包裹数组更新元素的方法实现，本质就是做了两件事:  
    1. 调用原生对应的方法对数组进行更新。  
    2. 重新解析模板，进而更新页面。  

4.在Vue修改数组中的某个元素一定要用如下方法:
- 使用这些API:  
    > push()  //向数组的末尾添加一个或多个元素，并返回新的长度。  
    > pop()  //向数组的末尾添加一个或多个元素，并返回新的长度。
    > shift()  //用于把数组的第一个元素从其中删除，并返回第一个元素的值。  
    > unshift()  //可向数组的开头添加一个或更多元素，并返回新的长度。  
    > splice()  //向/从数组中添加/删除项目，然后返回被删除的项目  
    > sort()  //对原列表进行排序  
    > reverse()  //颠倒数组中元素的顺序  
- Vue.set()或vm.$set() **（特别注意:Vue.set()和vm.$set()不能给vm 或vm的根数据对象添加属性!!）**  
  

5.**数据劫持**：指的是在访问或者修改对象的某个属性时，通过一段代码拦截这个行为，进行额外的操作或者修改返回结果。  

---
## 13、收集表单数据  
- `<input type="text"/>`，则v-model收集的是value值，用户输入的就是value值。  
- `<input type="radio"/>`，则v-model收集的是value值，且要给标签配置value值。  
- `<input type="checkbox" />`  
    1. 没有配置input的value属性，那么收集的就是checked(勾选or未勾选，是布尔值)  
    2. 配置input的value属性:  
        1. v-model的初始值是非数组，那么收集的就是checked(勾选or未勾选，是布尔值)  
        2. v-model的初始值是数组，那么收集的的就是value组成的数组
- 备注：v-model的三个修饰符：  
  - lazy:失去焦点再收集数据  
  - number:输入字符串转为有效的数字  
  - trim:输入首尾空格过滤  

---
## 14、其他内置指令  
- `v-html`指令:  
    1. 作用:向指定节点中渲染包含html结构的内容。  
    2. 与插值语法的区别:  
        1. `v-html`会替换掉节点中所有的内容，`{{xx}}`则不会。  
        2. `v-html`可以识别html结构。  
    3. 严重注意:`v-html`有安全性问题!!!!  
        1. 在网站上动态渲染任意HTML是非常危险的，容易导致`XSS`攻击。  
        ```  
        XSS攻击通常指的是通过利用网页开发时留下的漏洞，通过巧妙的方法注入恶意指令代码到网页，使用户加载并执行攻击者恶意制造的网页程序。这些恶意网页程序通常是JavaScript，但实际上也可以包括Java、 VBScript、ActiveX、 Flash 或者甚至是普通的HTML。攻击成功后，攻击者可能得到包括但不限于更高的权限（如执行一些操作）、私密网页内容、会话和cookie等各种内容。
        
        ```
        2. 一定要在可信的内容上使用v-html，永不要用在用户提交的内容上!  
- `v-cloak`指令(没有值):  
    1. 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性 。  
    2. 使用`css`配合`v-cloak`可以解决网速慢时页面展示出`{{xxx}}`的问题。  
        ```  
        在<style>中使用属性选择器标记：[v-cloak]:display:none 在页面一开始时先隐藏元素，等到Vue加载完成渲染时，v-cloak会被去除，则元素自然会重新显示
        ```
- `v-once`指令:  
    - `v-once`所在节点在初次动态渲染后，就视为静态内容了(即使值改变了，也不会更新页面的值)。  
    - 以后数据的改变不会引起`v-once`所在结构的更新，可以用于优化性能。  
- `v-pre`指令:  
    - 跳过其所在节点的编译过程。  
    - 可利用它跳过:没有使用指令语法、没有使用插值语法的节点，会加快编译。  

---

## 15、`Vue2` 生命周期(重要！！！)  
```
生命周期:  
1.又名:生命周期回调函数、生命周期函数、生命周期钩子。  
2.是什么:Vue在关键时刻帮我们调用的一些特殊名称的函数。  
3.生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的。  
4.生命周期函数中的this指向是vm或组件实例对象。
```
常用的生命周期钩子：  
1. mounted:发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】。  
2. beforeDestroy:清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。  
3. vue从`mounted`钩子函数开始可以获取和操作DOM，此前操作DOM浏览器会`报错`  
4. vue中获取不到`DOM`的生命周期有：
    1. 创建前，钩子函数为`beforeCreate`；  
    2. 创建后，钩子函数为`created`；  
    3. 载入前，钩子函数为`beforemount`;  
5. 关于销毁Vue实例：  
    1. 销毁后借助Vue开发者工具看不到任何信息。  
    2. 销毁后自定义事件会失效,但原生DOM事件依然有效。  
    3. 一般不会在beforeDestroy操作数据。因为即便操作数据，也不会再触发更新流程了。  

Vue2生命周期示意图：  

![Vue2生命周期](./IMG/Vue2%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)  

----

## 16、组件(重要！！！)  
**组件**：用来实现局部(特定)功能效果的代码集合(html/css/js/image等)。作用：作用：`复用编码、简化项目编码、提高运行效率`。

**模块**：向外提供特定功能的js程序，一般就是一个js文件。作用:`复用js，简化js的编写,提高js运行效率`。  

1. Vue中使用组件的三大步骤:  
    1. 定义组件(创建组件)  
    2. 注册组件  
        1. 局部注册：在`vm`的`components`下填写组件名称  
        2. 全局注册：使用 `Vue.component('要注册的名', 已经定义好的组件的名字)` 或 `Vue.component('要注册的名', {1、定义data;2、定义template等配置项})`  
    3. 使用组件(写组件标签)  
2. 如何定义一个组件?(**目前写的都是非单页面组件**)  
   使用`Vue.extend(options)`创建，其中**options和new Vue(options)时传入的那个options几乎一样**，但也有点区别;  
   例子：创建一个student组件  
    ```  
    const student = Vue.extend({
        template:`
        <div>
            <h2>hello {{name}}</h2>
        </div>
        `,
        data(){
            return{
                name:'tom'
            }
        }
    })
    ```
   区别如下:  
    1. `el` 不要写,因为所有的组件都要经过一个 `vm` 的管理，由 `vm` 中的 `el` 决定服务哪个容器。  
    2. `data` 必须写成函数，为什么?——避免组件被复用时，数据存在引用关系。  

        data(){  
            return{   
                a:'',  
                b:''  
            }  
        }  

备注：使用 `template` 可以配置组件结构。  

3. 几个注意点:  
    1. 关于组件名:  
        一个单词组成:  
        第一种写法(首字母小写): school  
        第二种写法(首字母大写): School  
        多个单词组成:  
        第一种写法(kebab-case命名):my-school
        第二种写法(CamelCase命名): MySchod1(需要Vue脚手架支持)  
        **备注:**  
        (1).组件名尽可能回避HTML中已有的元素名称，例如:h2、H2都不行。  
        (2).可以使用name配置项指定组件在开发者工具中呈现的名字。  
    2. 关于组件标签:
    第一种写法:<school></school>  
    第二种写法:<school/>  
    备注:不用使用脚手架时,<school/>会导致后续组件不能渲染。  
    3. 一个简写方式:  
        const school = Vue.extend(options）可简写为: const school = options  
  
4. `VueComponent`:  
    1. 组件本质是一个名为 `VueComponent` 的构造函数，且不是程序员定义的，是Vue.extend生成的。  
    2. 我们只需要写<school/>或<school></school>，Vue解析时会帮我们创建组件的实例对象，即Vue帮我们执行的：`new VueComponent(options)`。  
    3. 特别注意：每次调用 `Vue.extend`，返回的都是一个全新的`VueComponent` ！！！  
    4. 关于this指向：  
        1. 组件配置中：  
        data函数、methods中的函数、watch中的函数、computed中的函数它们的this均是【`VueComponent实例对象`】  
        2. new Vue(options)配置中:  
        data函数、methods中的函数、watch中的函数、computed中的函数它们的this均是【`Vue实例对象`】。  

    5. VueComponent的实例对象，以后简称 `vc` (也可称之为:组件实例对象)。Vue的实例对象,以后简称 `vm` 。    
5. Vue和VueComponent的关系  
    1. **一个重要的内置关系**: `VueComponent.prototype._proto_ ===Vue.prototype`(VueComponent的原型对象的原型对象是Vue的原型对象)  
    2. 为什么要有这个关系:让组件实例对象（vc）可以访问到 Vue原型上的属性、方法。  
    ![Vue和VueComponent的关系](./IMG/Vue%E4%B8%8EVueComponent%E7%9A%84%E5%85%B3%E7%B3%BB.png)  
6. 单文件组件  
   单文件组件是以 `.vue` 为结尾的文件  
   内容有：  
    - 页面结构`<template></template>`  
    - 脚本`<script></script>`  
    - 样式`<style></style>`  

    基本结构：  
    ```  
    <template>
        <div>
        <!-- 组件结构内容 -->
        </div>
    </template>
    <script>
    <!-- 写Vue实例的相关配置 -->
        import xxx from'./xx/xx'
        export default{
            name:'组件名称',
            data(){
                return{
                    a:'',
                    b:''
                }
            },
            methods:{},
            components:{}
        }
    </script>
    <style>
        <!-- css样式 -->
    </style>
    ```
---
## 17、vue项目中的基本渲染原理  
如图：  
![vue基本结构](./IMG/vue%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84.png)  
1. `School.vue`和`Student.vue`分别为两个单页面组件。  
2. `App.vue` 是**vue项目中统筹全部组件的父组件**，在`App.vue`中引入(import)两个组件并组注册、写在`<template></template>`中。  
3. 之后由`main.js`负责创建`vue实例`(new vue)：  
![main.js创建vue实例](./IMG/mainjs%E4%B8%AD%E4%BB%A3%E7%A0%81.png)  
注册了组件`App`，并指定`el`为`root`  
4. 之后在`index.html`中：  
    ```  
    <div id="root">
        <App></App>
    </div>
    ```
    即刻起完成了渲染，所有组件全部展示在`index.html`上  
---
## 18、Vue脚手架Vue CLI  
Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统：  
使用步骤：  
```
第一步（仅第一次执行)︰全局安装@vue/cli
`npm install -g @vue/cli`  
第二步:切换到你要创建项目的目录，然后使用命令创建项目  
`vue create xxx`
第三步:启动项目  
`npm run serve`  
```
---

## 19、ref属性  
1. 被用来给元素或子组件注册引用信息(id的替代者)  
2. 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象(vc)  
3. 使用方式：  
打标识：`<h1 ref="xxx">.....</h1>`或`<School ref="xxx">x</School>`。  
获取： `this.$refs.xxx`(this是VueComponent实例vc)。  
---

## 20、配置项props  
功能:让组件接收外部传过来的数据  
1. 传递数据：  
    比如：`<Demo :name="xxx"/>`  
2. 接收数据：  
    第一种方式(只接收)：  

        props: ['name']  

    第二种方式(限制类型)：  
    
        props:{
            name: Number
        }  

    第三种方式(限制类型、限制必要性、指定默认值)：  
    ```  
    props:{
        name:{
            type:String,//类型  
            required:true,//必要性  
            default:'老王’/默认值  
        }
    }
    ```
    **备注**： `props`是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，
    若业务需求确实需要修改，那么请复制`props`的内容到`data`中一份，然后去修改data中的数据。  

---

## 21、配置项mixin(混入)  
**功能：可以把多个组件共用的配置提取成一个混入对象使用方式**  
    第一步定义混合，例如：  
    `mixin.js`内容中配置：
        ```
        {
            data(){....},
            methods:{....}
        }
        ```
​    第二步使用混入，例如：  
    1. 全局混入：  
        在`main.js`中：  
        ```
        Vue.mixin(xxx)  
        ```
        2. 局部混入：  
        ```
        mixins: ['xxx']
        ```

---

## 22、插件
**功能:用于增强Vue**  
本质；包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。  
定义插件：  
在`plugin.js`中定义
```
install(Vue, options){
    //1. 添加全局过滤器  
    Vue.filter(....)  
    //2. 添加全局指令 
    Vue.directive(....)  
    //3. 配置全局混入(合)  
    Vue.mixin(....)
    //4. 添加实例方法  
    Vue.prototype.$myMethod = function () {...}  
    vue.prototype.$myProperty = xxxx
    }
```
使用插件：
```  
Vue.use(插件)
```

---
## 23、全局事件总线(GlobalEventBus)  
1. 一种组件间通信的方式，适用于`任意组件间通信`。
2. 安装全局事件总线(在`main.js`中):
    ```  js
    new Vue({
        beforeCreate(){
            Vue.prototype.$bus = this//安装全局事件总线，$bus就是当前应用的vm
        }
    })
    ```

3. 使用事件总线：  
    1. 接收数据:A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的回调留在A组件自身。
    ```  js
    methods(){
        demo(data){......}
    }
    .... ..
    mounted(){
        this.$bus.$on('xxxx ',this.demo)
    }
    ```
    2. B组件提供数据：  
    ```  js
    this.$bus.$emit('xxxx',数据)
    ```
    
4. **最好在beforeDestroy钩子中，用$off去解绑当前组件所用到的事件。**  
    ```  js
    beforeDestroy(){
        this.$bus.$off('xxxx')
    }
    ```
---

## 24、消息订阅与发布(pubsub)  
1. 一种组件间通信的方式，适用于任意组件间通信。  
2. 使用步骤：  
    1. 安装pubsub：npm i pubsub-js  
    2. 引入：import pubsub from 'pubsub-js'  
3. 接收数据:A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身。
```  js
methods(){
    demo(data){......}
}
mounted(){
    this.pid = pubsub.subscribe('xxx',this.demo)//订阅消息
}
```
4. 提供数据：`pubsub.publish('xxx',数据)`
5. 最好在 `beforeDestroy` 钩子中，用`PubSub.unsubscribe(pid)`去取消订阅。
---
## 25、nextTick  
1. 语法：  
    ```js
    this.$nextTick(()=>{
        .....
    })  
    ```
2. 作用：在下一次DOM更新结束后执行其指定的回调。  
3. 什么时候用：当改变数据后，要基于`更新后的新DOM`进行某些操作时，要在nextTick所指定的回调函数中执行。  
---
## 26、动画与过度  
写法:  
1. 准备好样式：  
    - 元素进入的样式：  
        1. v-enter：进入的起点  
        2. v-enter-active：进入过程中  
        3. v-enter-to：进入的终点  
    - 元素离开的样式：  
        1. v-leave：离开的起点  
        2. v-leave-active：离开过程中  
        3. v-leave-to：离开的终点
2. 使用`<transition>`包裹要过度的元素，并配置`name`属性：
    ```  
    <transition name="hello">
        <h1 v-show="isShow">你好啊!</h1>
    </transition>
    ```
    在CSS样式中：  
    - `动画`的写法  
    ```  
    /*定义动画*/
    @keyframes 动画名 {
    from{
        transform: translateX(-100px);
    }
    to{
        transform: translateX(0px);}
    }
    /*开始动作*/
    .hello-enter-active{
        animation: 动画名 0.5s linear;
        }
    /*结束动作*/
    .hello-leave-active{
        animation: 动画名 0.5s linear reverse;
        }


    ```  
    - `过渡`的写法  
    ```  
    /*进入的起点、离开的终点*/
    .hello-enter,.hello-leave-to{
        transform: translateX (-100%);
    }
    .he1lo-enter-active,.hello-leave-active{
        transition: 0.5s linear;
    }
    /*进入的终点、离开的起点*/
    .hello-enter-to,.hello-leave{
        transform: translateX(0);
    }
    
    ```  
3.备注：
若有多个元素需要过度，则需要使用：`<transition-group>`，且每个元素都要指定 `key` 值。  

---
## 27、配置代理(重要！)  
**方法一、在`vue.config.js`中添加如下配置：**
```  
devServer:{
    proxy : "http://localhost:5000"
}
```
说明：  
1. 优点:配置简单，请求资源时直接发给前端(8080)即可。  
2. 缺点:不能配置多个代理，不能灵活的控制请求是否走代理。  
3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时那么该请求会转发给服务器 **(优先匹配前端资源,如果请求的资源能在前端找到，就不会将请求转发给后端了)**  

**方法二、编写vue.config.js配置具体代理规则：**
```
module.exports = {
    devServer: {
        proxy:{
            '/api1':{//匹配所有以'/api1'开头的请求路径
                target:"http://localhost:5000',//代理目标的基础路径
                changeorigin: true,
                pathRewrite: i'^/api1': ''}
            },
            '/api2':{//匹配所有以'/api2'开头的请求路径
                target:"http://localhost:5001",//代理目标的基础路径
                changeOrigin:true,
                pathRewrite:{'^/ api2' : ''}
            }
    }
}
/*
changeOrigin设置为true时，服务器收到的请求头中的host为: localhost:5000
changeOrigin设置为false时，服务器收到的请求头中的host为: localhost:8080
changeOrigin默认值为true
*/
```
说明：  
1. 优点:可以配置多个代理，且可以灵活的控制请求是否走代理。  
2. 缺点:配置略微繁琐，请求资源时必须加前缀。  

## 28、Vue插槽(重要！！！)  
1. 作用：让父组件可以向子组件指定位置插入`html结构`，也是一种组件间通信的方式，适用于`父组件===>子组件`。  
2. 分类：
    - 默认插槽  
    - 具名插槽  
    - 作用域插槽
3. 使用方式：  
    1. 默认插槽：
        ```  vue
        父组件中使用子组件：
        <Category>
            <div>html结构1</div>
        </Category>
        子组件中的内容：
        <template>
            <diy>
                -- 定义插槽-->
                <slot>插槽默认内容...</slot>
            </div>
        </template>
        ```
        
    2. 具名插槽(`给插槽指定名称`)  
        ```  vue
        当父组件中写了多个HTML结构需要插入到不同的插槽：
        <Category>
            <template slot="center">  //对应插槽名的第一种写法
                <div>html结构1</div>  
            </template>
            <template v-slot:footer>  //对应插槽名的第一种写法
                <div>html结构2</div>
            </template>
        </Category>
        子组件中:
        <template>
            <div>
                <!--定义插槽-->
                <slot name="center">插槽默认内容...</slot>
                <slot name="footer">插槽默认内容...</slot>
            </div>
        </template>
        ```
        
    3. 作用域插槽  
        1. 理解：数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定。(games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定)
        2. 具体编码:
        ```  vue
        // 父组件中：  
        <Category>  
            <template scope="scopeData">  //绑定插槽的方式一  
                <! --生成的是ul列表-->  
                <ul>  
                    <li v-for="g in scopeData.games" :key="index">{{g}}</li>  
                </ul>  
            </template>  
        </Category>  
        <Category>  
            <template slot-scope="scopeData">  //绑定插槽的方式二  
                <! --生成的是h4标题-->  
                <h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>  
            </template>  
        </Category>  
        ```
        
        ```vue
        // 子组件中：  
        <template>  
            <div>  
                <slot :games="games"></slot>
            </div>  
        </template>  
        <script>
            export default {  
            name: 'Category',
            props: [ 'title '], //数据在子组件自身  
            data(){
                return i
                games :['红色警戒","穿越火线','劲舞团",'超级玛丽"]
              }
            }
        </script>
        ```
        
        
---
## 29、路由(重要！！)  
1. 理解：一个路由(`route`)就是一组映射关系(key - value)，多个路由需要路由器(router)进行管理。  
2. 前端路由： `key`是路径，`value`是组件。**使用路由可以实现单页面应用(即不需要切换或者刷新页面就可以展示不同组件页面的内容)**  
3. 基本使用：  
    1. 安装`vue-router`，使用命令：  
        ```
        npm i vue-roter 
        ```
    2. 在 `main.js` 中引入  
        ```
        Vue.use(VueRouter)  
        ```
    3. 在路由的配置文件 `index.js` 中编写配置项  
        ```  
        //引入VueRouter  
        import  VueRouter from 'vue-router'
        //引入需要配置路由的组件组件
        import Home from '../components/Home'  
        //创建router实例，管理路由的规则  
        const router = new VueRouter({
            //可以配置多组路由规则，对应多个组件
            routes:[
                {
                    path:'/home' //这个path就是点击切换组件的路径
                    component: Home  
                },
                {
                    .....
                }
            ]
        })
        ```
    4. 实现路由切换(active-class可以配置高亮样式)  
        ```  
        //router-link和a标签很像
        <router-link active-class="active to="/home">Home</router-link>
        ```
    5. 指定路由组件展示的位置  
        ```  
        <router-view></router-view>
        ```
4. 多级路由(嵌套路由)  
    1. 在配置路由规则里添加 `children` 配置：  
        ```  js
        routes:[
                {
                    path:'/home' //这个path就是点击切换组件的路径
                    component: Home  
                    children:[
                        //配置子路由  
                        {
                            path: 'new'
                            component: News
                        },
                        {
                            path: 'message'
                            component: Message
                        }
                    ]
                },
                {
                    .....
                }
            ]
        ```
    2. 路由的跳转  
        ```  js
        <router-link to="/home/news">News</router-link>
        <router-link to="/home/message">Message</router-link>
        ```
5. 注意点  
    1. 每个组件都有自己的`$route`属性，里面存储着自己的路由信息。  
    2. 整个应用只有一个router，可以通过组件的`$router`属性获取到。  
6. 路由参数  
    1. 路由的 `query` 参数  
        ```  js
        //to的字符串写法
        <router-link :to="/home/message?id= & title= "></router-link>  
        //to的对象写法
        <router-link 
            :to="{
                path: '/home/message
                query:{
                    id:
                    title:
                }'
            }">
        </router-link>  
        ```
        接收参数：  
        ```  js
        $route.query.id  
        $route.query.title
        ```
    2. 路由的params参数  
        ```  js
        //在路由规则中填写params参数
        [
            {
                path: 'home/:id/:title' //使用占位符声明接收params参数  
                component: Home  
                name: home  
            }
        ]
        ```
        传递参数  
        ```   js
        to的字符串写法
        <router-link :to="/home/message/id/title"></router-link>
        to的对象写法
        <router-link
            :to="{
                name:'home'
                params:{
                    id:
                    title:
                }
            }"></router-link>
        ```
        接收参数  
        ```  
        $router.params.id
        $router.params.title
        ```
7. 给路由命名  
    ```  
    routes:[
                {
                    path:'/home' //这个path就是点击切换组件的路径
                    component: Home  
                    name: 'home'  //给路由命名
                },
            ]
    ```
    好处：(简化跳转)  
    ```  
    //简化后，直接通过名字跳转  
    <router-link :to="{name:'home'}">Home</router-link>
    //简化后配合传递参数  
    <router-link
        :to="{
            name:'home' //不需要配置path路径了
            query:{
                id:
                title:
            }
        }">
    </router-link>
    ```
8. 路由规则里的`props` 配置  
   作用：让路由组件更方便接收到参数  
    ```  
    [
        {
            path:'/home' //这个path就是点击切换组件的路径
            component: Home  
            name: 'home'  //给路由命名
            //第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件  
            props:{a:10}  
    
            //第二张写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件  
            props:true  
    
            //第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给组件  
            props(route){
                return{
                    id:route.query.id,
                    title:route.query.title
                }
            }
   
        }
    ]
    ```
9. 编程式路由导航  
   作用：不借助 `<router-link>` 实现路由跳转  
   使用`$router`的两个API  
    ```  
    //1.push
    this.$router.push({
        name:
        params:{
   
        }
    })
    //2.replace
    this.$router.replace({
        name:
        params:{
            
        }
    })
    ```
10. 缓存路由组件  
    作用：让不展示的路由组件保持挂载，不被销毁  
    ```  
    <keep-alive include="Home,News">
        <router-view></router-view>
    </keep-alive>
    ```
    **注意：**   
    1. 默认是缓存使用使用了路由的组件，可以使用 `include` 属性来指定哪些组件是需要缓存，提高代码效率  
    2. `max`属性控制最多可以缓存多少组件实例。一旦这个数字达到了，在新实例被创建之前，已缓存组件中最久没有被访问的实例会被销毁掉。  
11. 路由的两个生命周期钩子  
    作用：写在组件内部，路由组件所独有的两个钩子，用于捕获路由组件的激活状态  
    1. `activated` 路由组件被激活时触发  
    2. `deactivated` 路由组件失活时才触发  
12. 路由守卫  
    作用：对路由的跳转进行权限控制  
    分类：  全局守卫、独享守卫、组件内守卫  
    1. 全局守卫：  
       全局守卫又分为 `全局前置守卫` beforeEach 、`全局后置路由守卫` afterEach ，配置在`index.js` 文件下
        ```  
        //全局前置守卫：初始化时执行、每次路由切换前执行，例如初次登入，不能访问除登录页面外的页面，已登录会自动跳转自首页等等。   
        router.beforeEach((to,from,next)=>{
            if(to.meta.isAuth){
                if(...){  //权限控制
                    next() //放行
                }
            }else{
                next() //放行
            }
        })
        //全局后置守卫：初始化时执行，每次路由切换后执行，例如对跳转后的页面进行例如滚动条回调0 0 位置、更新页面title、懒加载结束等等。   
        router.afterEach((to,from,next)=>{
            if(to.meta.title){
                document.title = to.meta.title //修改页面title
            }else{
                document.title = 'xxx'
            }
        })
        ```
    2. 独享守卫  
       独享守卫只有一个：`beforeEnter`，写在 `index.js` 文件懂得路由规则配置 `routes:[]` 下，配置在某一个组件的路由规则下  
        ```  
        beforeEnter(to,from,next){
            if(to.meta.isAuth){
                    if(...){  //权限控制
                        next() //放行
                    }
                }else{
                    next() //放行
                }
        }
        ```
    3. 组件内路由守卫  
    ```  
    //进入守卫：通过路由规则，进入组件时被调用  
    beforeRouteEnter(to,from,next){
        ....
    }
    //离开守卫：通过路由规则，离开组件时被调用  
    beforeRouteLeave(to,from,next){
        ....
    }
    ```

# VUE3的学习  
## 1. 使用 `vite` 创建项目  
创建工程  
```  
npm init vite-app xxx
```
进入工程项目，安装依赖  
```  
cd xxx  
npm install
```
运行  
```  
npm run dev
```
---
## 2. setup函数  
1. 理解： Vue3.0中一个新的配置项，值为一个函数。  
2. setup是所有Composition API(组合API)“表演的舞台”。  
3. 组件中所用到的:数据、方法等等，均要配置在setup中。  
4. setup函数的两种返回值:  
    1. 若返回一个对象，则对象中的属性、方法,在模板中均可以直接使用。(重点关注! )  
    2. 若返回一个渲染函数：则可以自定义渲染内容。(了解)  
5. 注意点:  
    1. 尽量不要与`vue2.x`配置混用
        - Vue2.x配置（data、methods、computed...）中可以访问到setup中的属性、方法。  
        - 但在setup中不能访问到Vue2.x配置(data、methods、computed...) 。  
        - 如果有重名， setup优先。
    2. setup不能是一个`async`函数，因为返回值不再是return的对象，而是`promise对象`，模板看不到return对象中的属性。
---
## 3. ref函数  
- 作用:定义一个响应式的数据
- 语法: const xxx = ref(initValue)
    - 创建一个包含响应式数据的引用对象(reference对象)。。JS中操作数据：`xxx.value`
    - 模板中读取数据:不需要`.value`，直接：`<div>{{xxx}}</div>`
- 备注：
    - 接收的数据可以是：基本类型、也可以是对象类型。
    - 基本类型的数据：响应式依然是靠`object.defineProperty()`的get与set完成的。
    - 对象类型的数据:内部“求助”了Vue3.0中的一个新函数——`"reactive函数"`。
---
## 4. reactive函数
- 作用：定义一个**对象类型**的响应式数据(基本类型别用它，用ref函数)
- 语法： `const 代理对象= reactive(源数据)`  
接收一个对象(或数组)，返回一个代理器对象(proxy对象). reactive定义的响应式数据是“深层次的”。
- 内部基于ES6的Proxy实现，通过代理对象操作源对象内部数据都是响应式的

## 5. vue3 响应式原理

- 通过Proxy对象（代理）：**拦截**对象中属性的变化：属性值的读、写、改、添加、删除等
- 通过Reflect对象（反射）：对源对象的属性进行操作

  ```js
  <script>
      let person = {
        name: 'lxl',
        age: 18,
        hobby: ['唱', '跳', 'Rap', '篮球'],
        family: {
          father: 'lsg',
          mother: 'xsg'
        }
      }
      const p = new Proxy(person, {
        // 读取person的属性时调用
        get(target,propName){
          console.log(`读取了p上的${propName}属性`);
          return Reflect.get(target,propName) // 真正操作源数据
        },
        // 修改数据时调用
        set (target,propName,value){
          console.log(`修改了p上的${propName}属性,值为${value}`);
          return Reflect.set(target,propName,value)
        }, 
        // 删除数据时调用
        deleteProperty(target,propName){
          console.log(`删除p上的${propName}属性`);
          return Reflect.deleteProperty(target,propName)
        }
      }) 
    </script>
  ```

## 6. 计算属性

vue3的计算属性不再是配置项，而是一个**API**

```vue
<script lang="ts">
import { reactive,computed } from 'vue' // 需要引入'computed'
  export default {
    name:'DemoTest',
    setup(){
      const person = reactive({
        firstName:'张',
        lastName:'三'
      })
      // 简写
      const fullName = computed(()=>{
      	return person.firstName + '-' + person.lastName
       })
      
      // 考虑读和写
      let fullName = computed({
        get(){
          return person.firstName + '-' + person.lastName
        },
        set(value){
          const NameArr = value.split('-')
          person.firstName = NameArr[0]
          person.lastName = NameArr[1]
        }
      })
      return {
        person,
        fullName
      }
    }
  }

</script>
```

## 7. watch监视

vue3的watch也是一个**API**

基本语法：

```js
import {watch} from 'vue' // 需要引入
// 一个三个参数
watch('被监视的属性',(newValue,oldValue)=>{
    // 当属性发生变化时执行的回调函数
    // newValue和oldValue都是Proxy对象
},{ 
    // 配置项，例如 ‘immediate’、‘deep’ 等
})
```

**监视ref定义的一个响应式数据**

```js
let person = ref('lxl')
watch(person,(newValue,oldValue)=>{
    console.log('peson发生了变化')
},{immediate:true})
```

**监视ref定义的多个响应式数据**

```js
let a = ref('0')
let b = ref('1')
watch([a,b], //  使用数组包裹
     (newValue,oldValue)=>{ 
    // newValu和oldValue是一个数组，一起记录监视的新旧值
    console.log('a或b发生了变化')
})
```

**监视reactive定义的响应式数据**

```js
let n = reactive({
    a:1
    b:{
        c:2,
        d:4
	}
})
watch(n,(newValue,oldValue)=>{ 
    console.log('p发生了变化')
})
```

注意：

- 监视reactive的全部数据时，强制开启**深度监视**，无法配置`deep`
- 监视reactive的全部数据时，oldValue无法正常获取，得到的值和newValue一模一样（坑）

**监视reactive数据中的某个属性**

```js
let n = reactive({
    a:1
    b:2
})
watch(()=>n.a, // 需要写成函数返回值的形式
     (newValue,oldValue)=>{
    console.log('p的a发生了变化',newValue,oldValue)
    // 此时可以正常获取oldValue
})
```

**监视reactive数据中的某些属性**

```js
let n = reactive({
    a:1
    b:2
})
watch([()=>n.a,()=>n.b], // 使用数组，里面写成函数返回值的形式
     (newValue,oldValue)=>{
    console.log('p的a或b发生了变化',newValue,oldValue)
})
```

**监视reactive数据中的对象**

```js
let n = reactive({
    a:1
    b:{
        c:2,
        d:4
	}
})
watch([()=>n.a,()=>n.b], // 使用数组，里面写成函数返回值的形式
     (newValue,oldValue)=>{
    console.log('p的a或b发生了变化',newValue,oldValue)
},
{deep:true}  // 此时deep奏效)
```

注意

- 此时oldValue也是无效

**watchEffect()函数**

**作用**：也是用于监视属性，但是不会指明监视哪个属性，而是再当中用到哪个属性，当属性变化时，就会执行一次这个方法

```js
let n = reactive({
    a:1
    b:2
})
// 当n.a或n.b变化，改函数会执行
watchEffect(()=>{
    const a = n.a // 内部引用要监视的属性
    const b = n.b // 内部引用要监视的属性
    consloe.log('a或b被改变了')
})
```

## 8. 生命周期

vue3声明周期其实和vue2相差无几

![vue3生命周期](D:\Frontend\vuestudy\IMG\vue3生命周期.png)

不同点：

- 原来的`beforeDestroy` ==> `beforeUnmount`
- 原来的`destroyed` ==> `unmounted`

**vue3还将生命周期钩子新增了对应的组合式API，可以写在`setuo()`里面**

| 钩子函数         | 组合式API         |
| ---------------- | ----------------- |
| beforeCreate（） | setup()           |
| created（）      | setup()           |
| beforeMount()    | onBeforeMount()   |
| mounted()        | onMounted()       |
| beforeUpdate()   | onBeforeUpdate()  |
| updated()        | onUpdated()       |
| beforeUnmount()  | onBeforeUnmount() |
| unmounted()      | onUnmounted()     |

>  在开发中，无论是写配置项钩子函数，还是写组合式API都是可以的，组合式API的执行顺序会先于钩子函数

## 9. toRef

作用：创建一个ref对象，将指定的数据返回成一个`ref对象`

语法：`cosnt x = toRef(对象名,'对象.属性')`

**当我们需要将一个响应式对象的一个属性拿出来单独做响应式**

```js
let p = reactive({
    a:1,
    b:2
})

const x1 = toRef(p,'a')
```

**toRefs()**

当我们需要把多个属性拿出来处理时，使用`toRefs`，toRefs可以批量创建多个ref对象

```js
let p = reactive({
    a:1,
    b:2
})

const x2 = toRefs(p)
```

## 10. 组件间通信

1. **props参数**

   vue3中通过自带的`defineProps`方法

   >  在vue3中由于写的是setup组合式API，所有在组件里接收props参数使用的不再是props配置项

   ```js
   // 父组件
   <Child name="lxl" age="18"></Child>
   ```

   ```js
   // 子组件
   <script setup lang="ts">
       // definedProps不需要引入，直接使用
       let props = definedProps(['name','age'])
   </script>
   ```

2. **自定义事件（emit）**

   vue3绑定自定义事件，使用自带的`defineEmits`方法

   ```js
   // 子组件
   <script setup lang="ts">
       // defineEmits方法的参数是一个自定义事件数组
       let emit = defineEmits(['lxl','zml'])
   	const car = '法拉利'
       const sendDate = () =>{
           // 返回的emit是一个方法，第一个参数是自定义事件，之后的参数是要传输的数据
           emit('lxl',car)
       }
   </script>
   ```

    ```js
    // 父组件
    <Child @lxl = "getData"></Child>
    
    <script setup lang="ts">
    	const getData = (params) => {
            console.log(params) // 输出 ‘法拉利’
        }    
    </script>
    ```

3. **全局事件总线**

   vue3 全局事件总线可以使用插件 `mitt`

   ```shell
   npm install -s mitt
   ```

   > vue3取消了vue构造函数，没有this的写法，无法使用 this.$bus 来使用全局总线

   配置mitt：

   ```js
   // 在bus/index.ts下
   import mitt from 'mitt'
   const $bus = mitt()
   export default $bus;
   ```

   兄弟组件1：

   ```js
   <script setup lang="ts">
   	import $bus from '../../bus'
   	import { onMounted } from 'vue'
   	onMounted(()=>{
           // on 用于接收数据
           // 第一个参数为自定义事件，第二个为接收到的参数的回调
           $bus.on('lxl',(car)=>{
               console.log(car)
           })
       })
   </script>
   ```

   兄弟组件2：

   ```js
   <button @click="sendData">传输数据给组件1</button>
   <script setup lang="ts">
   	import $bus from '../../bus'
   	let car = '兰博基尼'
       const sendData = () =>{
           // emit 用于传输 
           // 第一个参数为自定义事件，后面的参数为数据
           $bus.emit('car',car)
       }
   </script>
   ```

4. **v-model：**

   使用  `v-model` 可以实现父子组件**数据同步**

   ```js
   // 父组件
   <Child v-model="count"></Child>
   
   <script setup lang="ts">
   	let count = ref(100)
   </script>
   ```

   类似语法糖，v-model相当于给子组件传递了props，名为`modelValue`，以及给子组件绑定了自定义事件为 `update:modelValue`

   ```js
   // 子组件
   <button @click = "sendData"></button>
   <script setup lang="ts"> 
       // modelValue 和 update:modelValue 是固定的
       let props = defineProps(['modelValue'])
   	let $emit = defineEmits(['update:modelValue'])
       const sendData = () =>{
           // count + 100
           $emit('update:modelValue',porps.modelValue + 100)
       }
   </script>
   ```

   **还可以有指定的写法**，这种方式可以绑定多个v-model

   ```js
   // 父组件
   <Child v-model:count="count"></Child>
   
   <script setup lang="ts">
   	let count = ref(100)
   </script>
   ```

   ```js
   // 子组件
   <button @click = "sendData"></button>
   <script setup lang="ts"> 
       // modelValue 和 update:modelValue 是固定的
       let props = defineProps(['count'])
   	let $emit = defineEmits(['update:count'])
       const sendData = () =>{
           // count + 100
           $emit('update:count',porps.count + 100)
       }
   </script>
   ```

5. **useAttrs方法**

   useAttrs可以获取组件身上的**属性**与**事件**

   ```js
   // 父组件
   <Child name="lxl" age="18"></Child>
   ```

   ```js
   // 子组件
   <script setup lang="ts"> 
       import { useAttrs } from 'vue'
   	let $attrs = useAttrs()
       // 返回的$attrs是一个proxy对象，存储了父组件传来的参数
   </script>
   ```

   >  useAttrs 和 props 都可以获取父组件传来的参数，但是props更优先接收，且props接收后，useAttrs就获取不到了

6. **ref 和 $parent**

   > ref：可以既可以获取真实的DOM节点，也可以获取组件实例vc
   >
   > $parent：在子组件内可以通过 $parent 获取父组件的实例vc

   **ref**

   ```js
   // 子组件
   <script setup lang="ts"> 
       import { definedExpose }
   	let money = ref(100)
   	// 组件内部的数据对外是默认关闭的，就算外部获取组件实例也无法读取组件内部数据
       // 需要使用 definedExpose 将组件内部数据暴露
   	definedExpose（{
           money
       }
   </script>
   ```

   ```js
   // 父组件
   <Child ref="child"></Child>
   <script setup lang="ts"> 
   	console.log(son.value.money) // 100
   </script>
   ```

   **$parent**

   ```js
   // 父组件
   <Child ref="child"></Child>
   <script setup lang="ts"> 
       import { definedExpose }
   	let money = ref(100)
       // 父组件也需要把数据暴露出去
       definedExpose({
           money
       })
   </script>
   ```

   ```js
   // 子组件
   <button @click="handler($parent)">click</button>
   <script setup lang="ts"> 
       // %parent 需要作为回调的参数才能拿到
   	const handler = ($parent) =>{
         $emit('update:modelValue', props.modelValue + 1000)
         console.log($parent.money); // 100
   }
   </script>
   ```

7. **Provide 和 Inject** 

   > Provide 和 Inject 可以实现祖孙组件之间的通信
   >
   > provide(提供数据)、inject(获取数据)

    ```js
    // 祖组件
    <Father/>
    <script setup lang="ts"> 
        import { ref,provide}
    	import Father from './Father'
    	let money = ref(100)
        // 第一个参数为提供数据的 key
        // 第二个参数为 数据
        provide('m',money)
    </script>
    ```

   ```js
   // 父组件
   <Child/>
   <script setup lang="ts"> 
   	import Child from './Child'
   </script>
   ```

   ```js
   // 孙组件
   <script setup lang="ts"> 
   	import Child from './Child'
   	import { inject}
       let m = inject('m')
       consloe.log(m) //100
   </script>
   ```

   > 通过这种方式传来的数据可以修改

# Vuex的学习

 vuex是实现组件全局状态**共享**（数据共享）以及**管理**机制

### 1. 工作流程

![vuex](D:\Frontend\vuestudy\IMG\vuex工作流程.png)

### 2. 基本使用

1. **安装依赖： `npm install vuex@next --save`**

2. **State:**

   ```js
   // 配置在store中
   const store = createStore({
       state:{
           list:[
               {id:1},{id:2},{id:3}
           ]
       }
   })
   ```

   在组件中获取state

   ```js
   import { mapState } from 'vuex' // 使用辅助函数 mapState
   export default {
       name:xxx
       // state在compted内获取
       computed: mapState({
       	xxx: state => state.xxx
   	})
   }
   ```

3. **Getter：**

   ```js
   // Getter中编写处理state的方法，（不是修改），例如筛选、排序等等
   const store = createStore({
       state:{
           list:[
               {id:1},{id:2},{id:3}
           ]
       },
       getters:{
           handleList(state){
               return state.list.filter(item => item.id > 1)
               // 筛选出id大于1的
           }
       }
   })
   ```

   Getter会暴露为  `store.getters`对象，在组件中使用：

   ```js
   $store.getters.state // 可以以此拿到state
   ```

   通过 `mapGetters`方法

   ```js
   import { mapGetters } from 'vuex'
   export default{
       computed:{
           ...mapGetters({
               'handleList',
               'xxxxxx'
                xxx:'xxx' // 也可以取别名
           })
       }
   }
   ```

4. **Mutation：**

   更改 Vuex 的 store 中的状态的唯一方法是提交 mutation

   ```js
   // 配置
   const store = createStore({
     state: {
       count: 1
     },
     mutations: {
       increment (state) {
         // 变更状态
         state.count++
       },
           // 第一个参数是state，第二个是而外的参数，可以是是一个值，也可以是对象
       incrementA (state,payload) {
         // 变更状态
         state.count+payload
       },
       incrementB (state,payload) {
         // 变更状态
         state.count+payload.n // 对象形式
       }  
     }
   })
   ```

   在组件中

   ```js
   export default{
       name:xxx
       computed: mapState({
       	count: state => state.count
   	})
       // 使用 store.commit 提交修改
       store.commit('increment')
   	cosole.log(this.$store.state.conut) // 输出2
   	store.commit('incrementA','3') //传入payload为3
   	cosole.log(this.$store.state.conut) // 输出4
   	store.commit('incrementA',{
           n:4
       }) //传入payload为对象
   	cosole.log(this.$store.state.conut) // 输出5
   }
   ```

   **注意：**一条重要的原则就是要记住 **mutation 必须是同步函数**

5. **Action：**

   Action的作用：

   - Action 提交的是 mutation，而不是直接变更状态。
   - Action 可以包含任意异步操作。

   ```js
   // 配置
   const store = createStore({
     state: {
       count: 0
     },
     mutations: {
       increment (state) {
         state.count++
       }
     },
     actions: {
       increment (context) {
       // Action 函数接收一个与 store 实例具有相同方法和属性的 context 对象
         context.commit('increment')
       }
     }
   })
   ```

   在组件中分发Action

   ```js
   this.$store.dispatch('increment')
   // 同样也可以有payload
   this.$store.dispatch('increment',{xxx})
   // 以对象的形式（类似react的action）
   this.$store.dispatch({
     type: 'increment',
     xxx: xxx
   })
   ```

6. **Module：**

   >  由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

   vuex提供模块化管理，将store分割成多个**模块（module）**

   ```js
   const moduleA = {
     state: () => ({ ... }),
     mutations: { ... },
     actions: { ... },
     getters: { ... }
   }
   
   const moduleB = {
     state: () => ({ ... }),
     mutations: { ... },
     actions: { ... }
   }
   
   const store = createStore({
     modules: {
       a: moduleA,
       b: moduleB
     }
   })
   ```

   在一个模块局部

   ```js
   const moduleA = {
     state: () => ({
       count: 0
     }),
     mutations: {
       increment (state) {
         // 这里的 `state` 对象是模块的局部状态
         state.count++
       }
     },
     getters: {
       doubleCount (state) {
         return state.count * 2
       }
     }
   }
   ```

   

​	

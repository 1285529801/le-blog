---
title: Less学习
---
Less是一种动态样式语言，Less将CSS赋予了动态的语言特性，如：变量、继承、运算、函数。Less既支持在客户端上运行，也可以借助Node在服务端运行

**常用的预处理语言：**

- Sass
- Less
- Stylus

**Less安装**：在`vscode`下下载一个插件`easy less`就可以直接写less代码了

## 1. Less的使用

1. **Less的注释**

   - 以 `//` 开头的注释，不会被编译到css文件中
   - 以 `/**/` 包裹的注释会被编译到css文件中

2. **Less变量**

   Less变量需要以 `@` 为开头定义

   ```less
   // Less
   @blue: blue; //定义颜色的名字
   @wid: 200px; //定义一个长度
   div{
     color:@blue;
     width:@wid;  
    }
   ```

   编译成CSS后

   ```css
   div{
     color:blue;
     width:200px;  
    }
   ```

   > 使用变量的好处，当多个地方都使用同一个样式变量时，在外面后期需要更换时，只需要修改变量的值，就能实现全部改变

## 2. Less变量

1. **选择器变量**

   选择器的名称以变量代替

   ```less
   // Less
   @smile: orange;
   @fz-12: 12px;
   @hello1: hello;
   @wrap: div
   
   // 标签选择器
   @{wrap}{
     color: @smile;
     font-size: @fz-12;
   }
   // 类选择器
   .@{hello1}{
     color: @smile;
     font-size: @fz-12;
   }
   // id选择器
   #@{hello1}{
     color: @smile;
     font-size: @fz-12;
   }
   ```

   编译成CSS后

   ```css
   // 标签选择器
   div {
     color: orange;
     font-size: 12px;
   }
   // 类选择器
   .hello {
     color: orange;
     font-size: 12px;
   }
   // id选择器
   #hello {
     color: orange;
     font-size: 12px;
   }
   ```

2. **属性变量**

   属性的名称以变量代替

   ```less
   // Less
   @bg-color: background-color;
   @wp: wrap;
   .@{wp}{
     @{bg-color}:@smile;
   }
   ```

   编译成CSS后

   ```css
   .wrap {
     background-color: orange;
   }
   ```

3. **url变量**

   ```less
   @images: "@/asset/img";
   .wrap{
       background: url("@{images}/xxx.png");    
   }
   ```

   编译成CSS

   ```css
   .wrap {
     background: url("@/asset/img/xxx.png");
   }
   ```

4. **声明变量**

   声明变量的后面需要加上 `()`

   ```less
   // Less
   @background:{background:red;};
   @Rules:{
      width:200px;
      height:200px;
    }
   .wrap{
     @background(); 
   }
   ```

   编译成CSS

   ```css
   .wrap {
     background: red;
     width: 200px;
     height: 200px;
   }
   ```

5. **运算变量**

   ```less
   @color: #333;
   @len: 200px;
   .wrap{
     width: @len * 2;
     height: @len + 100;
     background-color: @color + #423;
   }
   ```

   编译成CSS

   ```css
   .wrap {
     width: 400px;
     height: 300px;
     background-color: #775566;
   }
   ```

   > 注意变量运算之间要隔着空格，不然会误认为是一体的

## 3. Less嵌套

1. **基本嵌套规则**

    ```html
    <div class="center">
        <div class="inner">
            hello world
        </div>
    </div>
    ```

    在Less中这样表示

    ```less
    .center{
        width:100px;
        height:100px;
        .inner{
            color:red;
            font-size:15px;
        }
    }
    ```

    编译成CSS是这样

    ```css
    .center {
      width: 100px;
      height: 100px;
    }
    .center .inner {
      color: red;
      font-size: 15px;
    }
    ```

2. `&` **符号表示当前选择器**

   ```less
   .center{
     width:100px;
     height:100px;
     .inner{
         color:red;
         font-size:15px;
   
   +++   &:hover{
           color:aqua;
         }
     }
   +++ &{
          text-decoration: none;
        }
   }
   ```

   编译成CSS

   ```css
   .center {
     width: 100px;
     height: 100px;
   +++  text-decoration: none;
   }
   .center .inner {
     color: red;
     font-size: 15px;
   }
   +++ .center .inner:hover {
   +++    color: aqua;
   +++  }
   ```

## 4. 混合方法

1. **普通混合**

   把**一个选择器**放到**另一个选择器**中，实现一个选择器有另一个选择器的属性

   ```less
   .a, #b ,.c{
     color: red;
   }
   .mixin-class {
     .a(); //加括号
   }
   .mixin-id {
     #b();
   }
   .mixin-icon{
     .c  //直接用名字
   }
   ```

   编译成CSS

   ```css
   .a,#b,.c {
     color: red;
   }
   .mixin-class {
     color: red;
   }
   .mixin-id {
     color: red;
   }
   .mixin-icon {
     color: red;
   }
   ```

   > 在混合后面需要加 '()'
   >
   > 混合的调用既可以是 .name() 也可以是 .name

2. **不带输出的混合**

   其中的属性会编译到CSS中去，但是选择器本身不会

   ```less
   // Less
   // 基本语法：.name(){}
   .mixin(){ // 加括号
       font-size:12px;
       width:200px;
       height:200px;
   }
   .wrap{
     .mixin; //使用混合
   }
   ```

   编译成CSS

   ```css
   /* mixin 没有输出，只有wrap */
   .wrap {
     font-size: 12px;
     width: 200px;
     height: 200px;
   }
   ```

3. **带参数的混合**

   ```less
   // 1. 带参数的混合
   // 2. 可以携带多个参数
   .mixin-a(@length:200px,@font:10px){ // 3. 可以给一个默认值
     font-size:@font;
     width:200px;
     height:@length;
   }
   .wrap{
       // 4. 在传递参数时，可以指定传给哪个参数
     .mixin-a(400px,@font:15px);
   }
   ```

   编译成CSS

   ```css
   .wrap {
     font-size: 15px;
     width: 200px;
     height: 400px;
   }
   ```

4. **带条件的混合**

   格式：`.name(@x) when(@x 比较运算符 xxx){}`

   比较运算符：`>`、`>=`、`=`、`<`、`<=`
   
   ```less
   .mix-light(@a) when(@a > 50){
     background-color: red;
   }
   .mix-light(@a) when(@a < 50){
     background-color: blue;
   }
   .mix-light(@a) when(@a = 50){
     background-color: blue;
   }
   
   .box1{
     .mix-light(55)
   }
   .box2{
     .mix-light(45)
   }
   .box3{
     .mix-light(50)
   }
   ```
   
   编译成CSS为
   
   ```css
   .box1 {
     background-color: red;
   }
   .box2 {
     background-color: blue;
   }
   .box3 {
     background-color: blue;
   }
   ```
   
   1. **less提供的一些参数类型检测函数**
   
      - `iscolor()` ：是不是颜色
   
      - `isnumber()` ：是不是数字
   
      - `isstring()`：是不是字符串
   
      - `iskeyword()`：是不是关键字
   
      - `isurl()`：是不是url
   
   2. **less提供的一些参数单位检测函数**
   
      - `ispixel`：是不是px
      - `ispercentage`：是不是%
      - `isem`：是不是em
   
4. **arguments参数**

   类似于JS的argument参数，将所有传来的参数全部放在`@arguments里面

   ```less
   // Less
   .box-shadow(@top,@right,@bottom,@left){
     margin: @arguments;
   }
   .wrap{
     .box-shadow(1px, 2px, 3px, 4px);
   }
   ```

   编译成CSS

   ```css
   .wrap {
     margin: 1px 2px 3px 4px;
   }
   ```

## 5. 继承

继承就是将父级选择器的属性继承，拥有与父级相同的属性

1. **extend关键字使用**

   ```less
   .red{
     background-color: red;
   }
   .wrap{
     // 关键字的使用
     &:extend(.red);
   }
   ```

   编译成CSS

   ```css
   /*从而.wrap 和 .red有相同的属性*/ 
   .red,
   .wrap {
     background-color: red;
   }
   
   ```

## 6. 导入

**nav.less如下：**

```less
.btn{
  background-color: red;
  width: 30px;
  height: 30px;
}
```

1. **通过 `@import` 来进行导入**

    ```less
    @import "nav"; 
    ```

    > 导入的时候只需要写文件名，不需要加后缀；
    >
    > 而且引入的文件可以随意放置，less会自动找到

2. **使用`reference`取消导入文件的编译**

    ```less
    @import (reference)"nav"; 
    ```

    > 通过该关键之的引入不会被编译
    
3. **使用`once`限制导入文件的编译次数**

    ```less
    @import (once)"nav"; 
    @import (once)"nav"; // 被忽略
    @import (once)"nav"; // 被忽略
    ```

    > 导入的 nav 只会被编译一次，后面的都会被忽略


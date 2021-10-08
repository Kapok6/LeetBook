# 1. JS

## 1.1. 基础

### 1.1.1. 变量声明

1. var：声明一个变量，在全局范围内都有效。

    ```javascript
    var a = [];
    for(var i=0;i<10;i++){ // 输出10,i全局有效，每轮共用一个
        a[i]=function(){console.log(i)}
    }
    a[6]();
    ```
    
2. 变量提升，只提升变量的声明，不提升初始化

    ```javascript
    console.log(a); // undefined
    var a = 3;
    ```

### 1.1.2. 作用域

1. 全局：在函数外声明，可被当前文档中的任何其他代码所访问。

2. 局部：函数内声明，仅函数内部访问。

3. 注意：ES6之前，无块作用域，在语句块中声明的变量将成为语句块**所在函数（或全局作用域）的局部变量**。

   ```javascript
   if(true){
   	var x=5 // x的作用域为if所在的函数内部
   }
   console.log(x) //5
   ```

4. 变量提升：JS允许，先使用变量，之后用var声明。 变量提升后会返回undefined。

5. 暂时性死区：let/const同样会被变量提升，不会被赋予初始值。在变量声明之前引用这个变量，将抛出引用错误（ReferenceError），直到被声明为止，都处在一个“暂时性死区”。

6. 函数提升：只有函数声明会被提升到顶部，而函数表达式不会被提升。

   ```javascript
   // 函数声明
   function foo() {}
   // 函数表达式
   var baz = function() {};
   ```

### 1.1.3. 数据类型

#### 1.1.3.1. 基础数据类型

七种基本数据：String，Number，Boolean，Null，Undefined，Object、Symbom（ES6）

1. null：表示此处不该有值；空指针，比如初始化对象为null。
   1.  作为函数的参数，表示该函数的参数不是对象。
   2. 作为对象原型链的终点。
2. undefined：表示此处缺少值；
   1. 变量仅声明，未初始化。
   2. 调用函数时，应该提供的参数没有提供，该参数等于undefined。
   3. 对象没有赋值的属性，该属性的值为undefined。
   4. 函数没有返回值时，默认返回undefined。
3. Boolean：true/false。
4. Number：整数或浮点数。用BigInt存任意精度的大整数 ，甚至可以超过数字的安全整数限制。
5. Sysbom：一种实例，唯一且不可改变。
6. Object：对象。

【注意】

1. 任意类型都可转化为Boolean值，Boolean(xxx)。
2. JS大小写敏感。

#### 1.1.3.2. 类型判断

1. `typeof xxx `  // `type`
   1. 未初始化/未声明：undefined。
   2. 数组/json对象/null：object。
   3. 常规情况：数字：number；字符串：string；函数：function。

2. `Object.prototype.toString.call(val)` // `[object type]`
   1. toString() 是 Object 的原型方法，Object 做类型判断，可用`Object.prototype.toString(val)`。
   2. ES6中新增的map/set，可直接用`val.toString()`。

3. `A instanceof B`：若A为B的实例，则返回true。

#### 1.1.3.3. 类型转换

1. 说明：JS是一种动态类型语言，声明变量时可以不必指定数据类型，代码执行时会自动转换。

2. 自动转换：

   1. 数值、字符串进行“+”运算：数字会转为字符串。

   2. 数值、字符串进行“-”运算：数字不会变为字符串。

      ```javascript
      '05'-7 // -2
      '3a-7' // NaN
      '05'+7 // 057
      ```

3. 强转：

   1. 字符串》数值：`parseInt()`和`parseFloat()`

### 数据基础操作



### 操作符



### 1.1.4. 字面量

1. 定义：可理解为由一定字词组成的词语表达式常量。
2. 字面量种类：
   1. 数组字面量 `["Lion", , "Angel"]`
   2. 布尔字面量``true`和`false`
   3. 整数字面量：0O73(或0o73)、0xa3(或0Xf3)、0b11(或0B11)

### 1.1.5. （不）可枚举属性

1. 定义：该属性指内部的“可枚举“标志。
2. 可枚举属性：
   1. 获取方式：直接的赋值和属性初始化的属性，均为可枚举。
   2. 特点：可以通过`for...in`循环进行遍历（除非该属性名是一个[Symbol](https://link.zhihu.com/?target=https%3A//developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)），或者通过`Object.keys()`方法返回一个可枚举属性的数组。
3. 不可枚举属性：
   1. 获取方式：（1）js中基本包装类型的原型属性是不可枚举的，如Object, Array, Number,String等。（2）通过 `Object.defineProperty` 定义的属性，该标识值默认为 false。

### 私有属性

1. 私有属性是指用户在本地定义的属性，而不是继承的原型属性。

### 1.1.6. 对象拷贝

#### 1.1.6.1. 浅拷贝

1. `copy=Object.assign(tar1,org1,org2...)`

   1. tar1是对org1,org2...的浅拷贝(若元素为对象，则拷贝引用；)，
   2. copy是对tar1的引用。

   ```javascript
     let obj1 = { a: 0 };
     let obj2 = { a: 1, b: { c: 3 } };
     let copy = Object.assign(obj1, obj2);
     obj2.a= 6;
     obj2.b.c= 4;
     copy.a = 5;
     console.log(obj1,copy);  // { a: 5, b: { c: 4 } }, { a: 5, b: { c: 4 } },obj3,和 obj2相等
   ```

2. `new = myArr.prototype.slice(start,end)`，原数组不受影响。

3. `...`扩展运算符


#### 1.1.6.2. 深拷贝

1. JSON.parse(JSON.stringify())
2. 递归实现

### 1.1.7. 遍历方法

1. **for key in arr**：
   1. 使用条件：主要用于遍历对象的**可枚举属性，包括自有属性、继承自原型的属性**。(**适合遍历对象**)
   2. key：为数组的索引/键名，且为string类型。
   3. 弊端：（1）某些情况，会出现随机顺序的遍历。（2）key为string类型，增加了转换，开销大。
   4. 优点：（1）可遍历数组的键名，使得遍历对象更方面。（2）可终止迭代：使用break， throw， continue 或return终止。
2. **arr.forEach**：
   1. 使用条件：用于调用**数组**的每个元素，并将元素传递给回调函数。（**不能遍历对象**）
   2. 弊端：（1）无法终止迭代，不能使用break, continue 或return等终止。（2）不能同时遍历多个集合。（3）在遍历的时候无法修改和删除集合数据。
   3. 优点：（1）简洁、效率同for。（2）不用关心下标，减少出错。
   4. 如何让`forEach,for`遍历对象：将对象转为数组`newArr = Object.keys(obj)`。
3. **for key of arr**：
   1. 使用条件： **ES6引入**，允许遍历 Array, String, Map, Set,TypedArray(类型化数组)、arguments、NodeList对象、Generator等可迭代的数据结构等。(**适合遍历数组或类数组**)
   2. `key`：为数组/对象的值。
   3. 弊端：不适用于处理原有的原生对象（原生对象是一个子集，包含一些在运动过程中动态创建的对象）。
   4. 优点：（1）可避免所有的`for in`陷阱。
4. **for**：
   1. 弊端：（1）结构比while循环复杂，容易出编码错误。（2）不可遍历对象。
   2. 优点：（1）简洁清晰。
   3. 如何让`forEach,for`遍历对象：将对象转为数组`newArr = Object.keys(obj)`。
5. **Object.keys**：返回一个数组，元素均为对象的**自有、可枚举**属性。
6. **Object.getOwnProperty**：用于返回对象的**自有属性，包括可枚举和不可枚举**。

### 1.1.8. 数组方法

1. map：数组映射，返回一个新数组，`arr.map(function)`。
2. filter：过滤，返回满足条件的新数组，`arr.filter(function)`。

### 1.1.9. setTimeout

1. 方法说明：`setTimeout(func,delay,arg1,arg2...)`定时器到期，会将`arg1...`传给前面的方法。

2. 运行机制：先将回调函数放到等待队列中，待区域内其他主程序执行完毕后，再从队列中，按先进先出顺序执行。会导致以下问题：

   ```javascript
   for (var i=0; i<4; i++) {
       setTimeout( function timer() {
           console.log( i ); // 4 4 4 4 输出结果，并非我们需要的 1 2 3 4
        }, i*1000 );
   }
   ```

3. 避免上述情况：

   ```javascript
   // 1. 使用闭包
   for (var i=0; i<4; i++) {
       (function(j){
          setTimeout( function timer() {
           console.log( j ); // 1 2 3 4
        	}, j )
       })(i)
   }
   // 2. 将定时器抽取为独立的方法，在for中调用
   // 3. 使用let的块级作用域，将for中的var，替换为let
   ```

## 1.2. 中级特性

### 1.3.1. 函数

### 1.3.2. 闭包

闭包/上下文/this的原理和基础用法

1. 闭包定义：有权访问另外一个函数作用域中的变量的函数。一个函数和对其周围状态（词法环境）的引用捆绑在一起（或者说**函数被引用包围），这样的组合就是闭包**（**closure）。
2. 闭包作用：嵌套函数可访问声明于它们外部作用域的变量。
3. 闭包特性：
   1. 闭包可以访问当前函数以外的变量
   2. 即使外部函数已经返回，闭包仍能访问外部函数定义的变量
   3. 闭包可以更新外部变量的值

### 模块加载机制

https://m.imooc.com/article/284624#%E6%A8%A1%E5%9D%97%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6

## 1.3. 高级特性

### 1.3.3. 面向对象

一切皆对象，js基于原型的

[对象及原型](https://zhuanlan.zhihu.com/p/28496709)

[面向对象ES5](https://zhuanlan.zhihu.com/p/334757139)

1. 创建对象的方式：
   1.ES6之前：通过构造函数
   1. JS内置构造函数
   2. 通过字面量
   3. 自定义构造函数

1. 创建对象1：使用构造器
2. 创建对象2：使用`Object.create`
3. 创建对象3：使用`class`关键字

### 1.3.4. 继承&原型链

1. 原型链
   1. 定义：每个实例对象，都有一个私有属性（`__proto__`），指向它的构造函数的原型对象（`prototype`）,该原型对象也有一个自己的原型对象( `__proto__`) ，层层向上直到一个对象的原型对象为 `null`。
   2. 特点：JS对象有一个指向一个原型对象的链，当访问一个对象的属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾。

2. 继承
   1. JS的继承，是基于原型链的。
   2. 几乎JS中的对象都是位于原型链顶端的`Object`实例。
   3. 继承时，无论属性还是方法，都会进行覆盖。
   4. 当继承的函数被调用时，**this指向**当前继承对象，而不是继承函数所在的原型对象。

## 1.4. ES6

### 1.4.1. 块级作用域

1. ES5：只有全局、函数作用域，引发的问题：

   ```javascript
   // 第一种场景，内层变量可能会覆盖外层变量。
   var tmp = new Date();
   function f() {
     console.log(tmp); // undefined。由于下面声明了var，所以这里会有“变量提升”。
     if (false) {
       var tmp = 'hello world';
     }
   }
   f(); // undefined
   
   // 第二种场景，用来计数的循环变量泄露为全局变量。
   var s = 'hello';
   for (var i = 0; i < s.length; i++) {
     // ...
   }
   console.log(i); // 5
   ```

2. ES6，let的块级作用域

   1. 内、外层互补影响
   2. 可以无限嵌套
   3. 可以在块中声明函数


### 1.4.2. let & const

1. `let`作用：声明一个块作用域的**局部变量**。

2. `const（ES6）`：声明一个块作用域的**只读常量**，必须在声明时赋值，且不允许在函数运行时重新声明。

3. `let/const`特点：无变量提升、会导致暂时性死区、不可重复声明、具有块级作用域。

4. `let`块级作用域：

   1. 只在`let`命令所在的代码块内有效

      ```javascript
       var a = [];
       for (let i = 0; i < 10; i++) {//i只在本轮循环有效，每轮都是新的
         a[i] = function () {console.log(i);};
       }
       a[6](); // 6
      ```

   2. for循环：循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

      ```javascript
      for (let i = 0; i < 3; i++) {
        let i = 'abc';
        console.log(i); // abc abc abc
      }
      ```

5. 不可重复声明

   1. 同一个作用域内不能重复声明
   2. 不能在函数内部重新声明参数

6. `let/const`无变量提升：不可以先使用，后声明，会报错。（var可以，值为undefined）

7. `let/const`的暂时性死区：（概念：进入当前作用域后，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。）

   1. 在let/const`声明变量之前使用，会报错。（块内变量不会受全局变量的影响）

      ```javascript
      // 场景1：取值
      var tmp = 123;
      if (true) {
        tmp = 'abc'; // ReferenceError
        let tmp;
      }
      
      // 场景2：赋值
      var x = x // 正常
      let a = a // ReferenceError:a is not defined
      
      // 场景3：传参赋值
      function bar(x = y, y = 2) {
        return [x, y];
      }
      bar(); // 报错, 因为x=y时，y还没有声明。若改为x=2,y=x则正确。
      ```

   2. 在let/const`声明变量之前，使用`typeof`判断类型，会报错。

      ```javascript
      typeof tmp; // 不会报错，返回undefined
      var tmp = 123;
      if (true) {
        typeof tmp; // 若块内有了let声明，则先判断，会报错，返回ReferenceError
        let tmp;
      }
      ```

### 1.4.3. 数组方法

1. `arr.map(func)`：数组映射，`['1','2'].map(parseInt)`  // [1, NaN,NaN]。
2. `arr.filter(func)`：数组过滤，返回符和条件的元素数组。


### 1.4.4. proxy

### 1.4.5. 异步编程Promise/async/await

### 1.4.6. Generator

### call/apply/bind

1. 3者作用：

   1. 劫持另外一个对象的方法，继承另外一个对象的属性.
   2. 将任意对象设置为任意函数的作用域，这样对象可以不用关心方法

2. apply的使用：`func.apply(obj,arguments/array)`，第1个参数为this指向。

   ```js
   //功能1：劫持另一个对象的方法
   function sum (num1,num1){
       return num1+num1
   }
   function callSum(num1,num2){
       //传参方式一：传入arguments对象
       return sum.apply(this,arguments)
       
       //传参方式二：传入参数数组
       // return sum.apply(this,[num1,num2])
       }
   console.log(callSum(10,10)) // 20
   
   // 功能2：改变this指向
   window.color = 'red'
   let obj = {
       color:'blue'
   }
   function sayColor(){
       console.log(this.color)
   }
   sayColor() //全局调用函数，this指向window
   sayColor.call(this) //this指向window
   sayColor.call(window) //this指向window
   sayColor.call(obj) //改变了this指向，指向对象obj
   ```

3. call的使用：`func.call(obj,param1,param2...)`，第1个为this指向。

   ```js
   function callSum (num1,num2){
       return sum.call(this,num1,num2)
   }
   console.log(callSum(10,10))
   ```

4. bind的使用：会创建一个新的函数实例，其this指向会被绑定到传给bind()的对象上。`func.bind(obj)`，this指向传入的obj。

   ```js
   window.color = 'red'
   let obj = {
       color:'blue'
   }
   function sayColor(){
       console.log(this.color)
   }
   let objSayColor = sayColor.bind(obj) // this指向obj
   objSayColor()
   ```

5. 区别：

   1. apply传参：（obj, `arguments`或数组)
   2. call传参：（obj, 枚举参数)
   3. bind传参：`(obj)`

## 1.5. 安全相关

### 1.5.1. 安全头

- CSP 内容安全策略——开启后，请求头部增加 Content-Security-Policy:object-src 'self' 设置
- XSS 攻击防护——开启后，请求头部增加 X-XSS-Protection:1; mode=block 设置
- 点击劫持攻击防护——开启后，请求头部增加 X-Frame-Options:SAMEORIGIN 设置
- 内容嗅探攻击防护——开启后，请求头部增加 X-Content-Type-Options:nosniff 设置
- 浏览器缓存禁用——开启后，增加 Cache-Control:no-cache、Pragma:no-cache&Expires:0 设置

## 1.6. 问题总结

### 1.6.1. 获取前12个月份

```javascript
function getLastYearMonthArray() {
    var result = []; // 结果集
    var d = new Date();  
    var year = d.getFullYear(); 
    d.setMonth(d.getMonth() + 1, 1); //设置月份,设置当前日为1号 （避免出现31号时候，其他月份没有31号的bug）
    for (var i = 0; i < 12; i++) { 
        d.setMonth(d.getMonth() - 1); //月份值-1 
        var m = d.getMonth() + 1;  // 月份 + 1,获取真正的月份
        m = m < 10 ? "0" + m : m;  //如果小于10月 给前面 +0
        result.push(d.getFullYear() + "-" + m); //存储结果
    }
    return result.reverse(); // 返回反向排列数组
}
```

### 1.6.2. 跳出循环/方法

- `return`：跳出函数
- `break`：
- `continue`：

### 1.6.3. JS中定义方法

## 1.7. JS技巧

### 1.7.1. 简写

1. 三元操作符：if,else if ,else if, ...else的简写模式

   ```javascript
   // if, else
   let res1;
   if (condition1){
       res2 = val1True;
   } else {
       res2 = val1False;
   }
   let res1 = condition1 ? val1True : val1False;
   // if ,else if ,else if, else
   let res;
   if(condition1){
       res2 = val1True;
   } else if(condition2){
       res2 = val2True;
   } else{
       res2 = valFalse;
   }
   let res2 = condition1 ? val1True : condition2 ? val2True : valFalse;
   ```

2. 短路求值：当需要用变量A给B赋值时，判断A不为null，undefined，或空时，才进行赋值，否则赋值为C。

   ```javascript
   if (A !== null || A !== undefined || A !== '') {
        let B = A;
   } else {
       let B=  C;
   }
   // 简写，短路求值，A短路，会用C赋值
   B = A || C; 
   ```

3. 连续声明多个变量

   ```javascript
   let a;
   let b=1;
   let c=3;
   // 简写
   let a,b=1,c=3; // undefined 1 3
   ```

4. if 条件判断

   ```javascript
   if (A===true){}
   // 简写
   if(A){} // A真
   if(!A){} // A非真
   ```

5. for循环》`forEach`

   ```javascript
   for (let i=0;i<arr.length;i++){
       console.log(arr[i]);
   }
   // 简写
   for(let index in arr){ 
       console.log(arr[index]);
   }
   arr.forEach((element,index,arry)=>{
       console.log(element);
   })
   ```

6. 十进制数简写》科学计数法：10000=1e4

7. 对象属性简写：如果属性名与key名相同，则可以采用ES6的方法：
   `const obj = { x:x, y:y };` 》`const obj = { x, y };`

8. 函数简写》箭头函数：

   ```javascript
   // 普通函数
   function tFun(name){let a=3;}
   tFun = name => let a = 3; // 简写
   
   // 将函数赋值给变量
   var func = function(){ return {foo:1}; }
   var func = () => { return {foo:1}; } // 简写
   
   // 嵌套函数，无参数
   setTimeOut( function tFun(){ let a=3;let b = 5;},2000 )
   setTimeOut( ()=>{ let a=3;let b = 5;},2000 ); 
   ```

9. 函数的隐式返回值：箭头函数，多行语句，用()包裹。

   ```javascript
   function calc(x){
       return Math.PI*x;
   }
   // 简写
   calc = x => (Math.PI*x;)
   ```

10. 强制函数传参

    ```javascript
    function foo(bar) {
      if(bar === undefined) {
        throw new Error('Missing parameter!');
      }
      return bar;
    }
    // 简写
    mandatory = () => {
      throw new Error('Missing parameter!');
    }
    foo = (bar = mandatory()) => {
      return bar;
    }
    ```

11. 模板字符串：

    ```javascript
    const db = `http://${host}:${port}/${database}`;
    ```

12. 解构赋值

    ```javascript
    const { store, form, loading } = this.props;
    ```

13. 多行字符串简写：使用反引号，可省略“换行”和“+”

    ```javascript
    const lorem = `Lorem ipsum dolor sit amet, consectetur
        adipisicing elit, sed do eiusmod tempor incididunt`
    ```

14. 扩展运算符简写：数组拼接、对象解构

    ```javascript
    // 数组拼接（任意位置）
    const odd = [1, 3, 5 ];
    const nums = [2, ...odd, 4 , 6];
    // 对象解构
    const { a, b, ...z } = { a: 1, b: 2, c: 3, d: 4 };
    ```

15. 数组查找：用`find`替换`for遍历`

    ```javascript
    const pets = [
      { type: 'Dog', name: 'Max'},
      { type: 'Cat', name: 'Karl'},
      { type: 'Dog', name: 'Tommy'},
    ]
    
    // for循环
    for(let i = 0; i<pets.length; ++i) {
      if(pets[i].type === 'Dog' && pets[i].name === name) {
        return pets[i];
      }
    }
    // 简写：find进行遍历查找
    pet = pets.find(pet => pet.type ==='Dog' && pet.name === 'Tommy');
    ```

16. 统一对象校验：

    ```javascript
    // 判断两个属性是否存在
    function validate(values) {
      if(!values.first)
        return false;
      if(!values.last)
        return false;
      return true;
    }
    console.log(validate({first:'Bruce',last:'Wayne'})); // true
    
    // 简化：判断n个属性是否存在的通用方法
    const schema = {
      first: {
        required:true
      },
      last: {
        required:true
      }
    }
    const validate = (schema, values) => {
      for(field in schema) {
        if(schema[field].required) {
          if(!values[field]) {
            return false;
          }
        }
      }
      return true;
    }
    console.log(validate(schema, {first:'Bruce'})); // false
    console.log(validate(schema, {first:'Bruce',last:'Wayne'})); // true
    ```

17. 替换`Math.floor()`》双重非运算符`~~`，运算更快

    ```javascript
    Math.floor(4.9)===4 // true
    ~~4.9===4 // true
    ```

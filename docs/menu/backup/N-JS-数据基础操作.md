## 控制台处理

1. 逐行读取：`readline`

   ```javascript
   const readline = require('readline');
   const rl = readline.createInterface({
     input: process.stdin,
     output: process.stdout,
   });
   rl.on('line', (line) => { // line：逐行读取的line
     console.log(line); 
   })
   ```

##  数据结构&操作

### 字符串

1. 创建：

   1. 含变量：使用模板字符串，

      ```js
      const str = `import ${func} from ${func}'
      ```

   2. 重复n次： `lodash/repeat 或 str.prototype.repeat`

      ```js
      let str = 'js'let str1 = repeat(str, 3); 
      let str2 = str.repeat(tiems); // times>=0才有效，小数会自动下取整
      ```

2. 检查：

   1. 是否字符串：`typeof str`

   2. 是否为空： ！，!!

      1. `!`：取反操作，针对 `'', null , undefined, 0`，取反后为true。

      2. `!`：取反操作，针对 `' ', '0'`，取反后为false。

      3. **`!!`：判空表达式（推荐）**

         ```js
         let a;
         if(a!=null && typeof(a)!=undefined && a!=''){}
         if(!!a) // 上下等价
         ```

   3. 是否全字母：`let res = /^[a-zA-Z]+$/.test(str)`，（返回布尔值）。

   4. 是否全数字：

      

   5. 是否包含指定字符：`str.includes(chr)`，（返回布尔值）。

      ```js
      const result1 = !!str && str.includes('Guide');
      ```

   6. 是否以某个字符结尾：`str.startsWith/endsWith(chr)`，（返回布尔值）。

3. 查找：

   1. 遍历：推荐`for of`
   2. 按值查 首次/末次出现下标：
      1. 推荐：**`indexOf、lastIndexOf`(返回下标/-1)。**
      2. `search(正则/str)`(返回首次出现下标/-1)。
      3. `match(正则)`(返回正则匹配结果，数组)。
      4. `matchAll(全局模式的正则)`，正则需是‘全局模式’，`.../g`，否则报错（返回迭代器，可用`[...matchAll(reg)]`获取）。
   3. 按下标查 字符/Unicode码：
   1. `str.charAt(index)、str.charCodeAt(index)`，（返回字符/空，下标越界也返回空）。
      2. 下标：`str[index]`（返回字符/undefined）。

4. 切片：

   1. **`str.substring(start,[end])`，**左闭右开；start,end可为负，则逆向截取。
   2. **`str.slice(start,[end])`，**同`substring`
   3. `str.substr(start,len)`，截取len个。(可能会废弃)

5. 替换/改：

   1. 逆置：借助数组逆置 `arr.reverse()`

      ```js
      const result = 'abcdefg'.split('').reverse().join('');
      ```

   2. 单次替换（按值）：str.replace(oldChr/reg,newChr)`，（返回新串，不影响原串）。`

   3. 全部替换（按值）：`str.replaceAll(oldChr/reg,newChr)`,（返回新串，不影响原串）。

   4. 按值修改：无。（注：**通过索引修改，无效**）

   5. 去除首尾空白：`str.trim(),trimEnd(),trimStart()`，（返回新串，不影响原串）。

6. 转换：

   1. 其他类型》字符串：

      1. `val.toString([进制])`：可设置进制。
         1. null、undefined没有该方法
         2. 数值、布尔、对象、字符串都有该方法
      2. `String(val)`：任何数据都有该方法，但不能设置“进制”。
      3. num与str相加：如`23+''`，不推荐，因为不易理解。

   2. 对象》JSON字符串：` let newStr = JSON.stringify(obj)` ,（对象判空：`newStr=='{}'`）

   3. 字符串》数字：

      1. 强转：`parseInt()`和`parseFloat()`

      2. num与str相减（且不含字母）：不推荐，不易理解。

         ```js
         log('06' + 3) //063
         log(5-'07') // -2
         log('05'-9)// -4 
         log('3a'-9)//NaN ,相减 含有字母，则为NaN
         log('3a'-'9')//NaN,相减 含有字母，则为NaN
         ```

   4. 大小写转换：`str.toLowerCase()`,`str.toUpperCase()`（返回新串，不影响原串）。

7. 分隔：

   1. `str.split(分割符)`，分割符可为空''，则按字符分割；分隔符可用正则（返回新串，不影响原串）。

      ```js
      let str = 'a,b，c,d'
      let arr = str.split(/,|，/)
      console.log(arr) // [a,b,c,d]
      ```

8. 其他：
   1. 拼接：`res = str1.concat(str2,...)`，不影响原串。
   2. 计算长度：`str.length`。
   3. **计算字符频次**：无直接的方法，可间接获取 如`str.split(chr).length-1`。

### 数组

1. 创建：（二维类似）

   1. `arr=[e1,e2...]`；
   2. `arr = new Array(len).fill(初始值)`;
   3. `arr=new Array(e1,e2...)`;

2. 增（会修改原数组）

   1. 尾插：`arr.push(el)`，（返回数组的新长度，修改原数组）。
   2. 头插：`arr.unshift(el)`，（返回数组的新长度，修改原数组）。
   3. 扩张/缩减数组：通过直接修改length的值，或者在超出数组的边界处赋值，如 `arr.length=newLen; arr[newLen] = el`。（修改原数组）。

3. 删（会修改原数组）

   1. 尾删：`arr.pop()`（返回删除的元素，修改原数组）。
   2. 头删：`arr.shift()`（返回删除的元素，修改原数组）。
   3. 按下标删：`arr.splice(index,[count])`，若省略count，会删除index后所有的（返回删除的数组，修改原数组）。

4. 改（会修改原数组）：修改某值：`arr[index]=newVal`。

5. 转化：

   1. 逆序：`arr.reverse()`，（返回原数组，修改原数组）。
   2. 转字符串：`arr.toString()`，逗号分隔。（不修改原数组）。
   3. 转字符串'：`arr.join(sep)`，可设置分隔符。（不修改原数组）。
   4. 扁平化：`arr.flat([depth])`，默认1，Infinity可完全压缩；会移除空项。

6. 查

   1. 按值查索引：`arr.indexOf(el)`，（返回索引/-1）。
   2. 按值查 存在性：`arr.includes()`（返回true/false）。
   3. 按下标查/索引：`arr[index]`, 若索引无效，返回 `undefined`。
   4. 按条件查下标：`arr.findIndex(func)`（返回第一个满足条件的**索引**/-1）
   5. 按条件查值：`arr.find(func)`（返回第一个满足条件的**元素**/`undefined`）。

7. 拼接

   1. ES6 拷贝：`let newA = [... a]`（**浅拷贝**，返回新数组，不修改原数组）。

   2. 利用ES6进行拼接：`arr1.push(...arr2)，**（推荐）**

   3. apply劫持数组：`arr1.push.apply(arr1,arr2) `**（推荐）**

   4. 拼接：`let newarr = arr.concat([e1,e2..])`，（**浅拷贝**，返回新数组，不修改原数组）。

      ```js
      // 浅拷贝：第一层为深拷贝，再深的为浅拷贝
      let a=[1,2,[3,4]] ; // 1,2为深，[3,4]为浅
      let newA = a.concat(); 
      ```

   5. 原地拷贝：`arr.copyWithin(target,[start,[end]])`：在数组内部，将一段元素序列拷贝到另一段元素序列上，覆盖原有的值。（返回原数组，修改原数组）。

   6. 切片：`arr.slice([start,end])`，（**浅拷贝**，返回新数组，不修改原数组）。

8. 遍历/迭代

   【注意】：1. `for,for of,forEach,map`遍历时的`el`都是引用，无法直接修改原数组，都需要借助到下标。2. 删除元素，都会导致后续元素被跳过。

   1. `forEach`：完整遍历，一般用于**访问**（非修改，无返回）。

      1. 语法：`arr.forEach((cur,[index,arr])=>{})`
      2. 修改：若实在需要，可通过`arr[index]`修改，通过`el`修改无效。
      4. 使用`return`无法终止遍历；使用`break,continue`会报错。
      
   ```javascript
      let clothing = ['a', 'b', 'c', 'd'];
      clothing.forEach((el, index) => {
        // el += '-'; // 修改原数组 无效
        clothing[index] = el + '-'; // 修改原数组OK
      })
      console.log(clothing); //  [ 'a-', 'b-', 'c-', 'd-' ]
   ```
   
   2. `map`：遍历&修改原数组（**返回新数组，不修改原数组**）。

      1. 语法：`arr.map((cur,[index,arr])=>{return newEl })`
      4. 使用`break,continue`会报错。
      
      ```javascript
   let numbs = [1, 2, 3]
      let nArr = numbs.map(x => { return x * 2 });
      console.log(nArr,numbs); // [ 2, 4, 6 ],[1, 2, 3]
      ```
      
   3. `for of`：遍历数组中的“值”，**可中断**。

      1. 语法：`for(let el of arr)`。
      2. 修改：无法修改/删除。

   4. `for`：下标迭代，遍历数组，**可中断**。

      1. 语法：`for(let i=0;i<arr.length;i++)`。
      2. 修改：可通过`arr[i]`修改。

   5.  `reduce`：`arr.reduce((accumulator,cur)=>{},[initVal])`，对数组中的每个元素执行一个由您提供的**reducer**函数(升序执行)，将其**结果汇总为单个**返回。

   6.  `reduceRight`：类似上面，只不过逆序遍历“从右到左”。

   10. `every/some`：`arr.every(func)`，判断数组中每个元素/是否有一个，满足func。（返回true/false）

   11. `filter`：`arr.filter(func)` 返回满足条件的元素组成的数组。（返回新数组，不修改原数组） 

9. 索引/切片

   1. 索引：`arr[index]`,索引无效，返回 `undefined`。
   2. 切片：arr.slice([start,end])`：默认拷贝全部，**浅拷贝**。（返回新数组，不修改原数组）

10. 排序 `arr.sort([func])`（返回新数组，**修改原数组**）。

   1. 排序方法`func(a,b)`：函数可以带一个或两个参数，表示数组的元素，返回值必须是正负零，表示数组元素相比较的办法。

   2. 默认按字符序，数字非升序。

      ```javascript
      var numbers = [4, 2, 5, 1, 3];
      numbers.sort((a, b) => a - b);  // 数字升序，[ 1, 2, 3, 4, 5 ]
      ```

   3. 英文字母排序，默认小写比大写大，会排后面；若想忽略大小写，可自定义规则，做大小写转换。

      ```javascript
         var a = ["Java","C","C++","JavaScript","jQuery","PHP","Python"];
         a.sort(function (x, y) {
             if(x.toLowerCase()>y.toLowerCase())
                 return 1;
             else if(x.toLowerCase()<y.toLowerCase())
                 return -1;
             else return 0;
         });
         console.log(a);
      // [“C”,“C++”,“Java”,“JavaScript”,“jQuery”,“PHP”,“Python”]。
      ```

   4. 数组对象排序：单个属性

      ```javascript
         a.sort(function (x,y) {
             return y.score-x.score; // 按sort排序,score降序
         });
      ```
      
   5. 数组对象排序：多个属性

      ```javascript
      // 按年龄从低到高排序，年龄相同时按性别排序  
         a.sort(function (x,y) {
             if(x.age!=y.age)
                 return x.age-y.age; // 升序
             else {
                 if(x.gender=="男")
                     return 1;
                 else
                     return -1;
             }
         });
      ```
      
16. 判断

    1. 数组判空：`arr.length>0`
    2. 判断是否为数组：`Array.isArray(arr)`

### 对象

1. 创建：`let obj={a:1,b:2}`，字符串可省略单引号。

2. 删：`delete x`，返回布尔值

    1. `delete obj.x`：可删除对象的指定属性，若再访问该属性，返回`undefined`；

    2. 不可以删除对象的原型链上的属性。

    3. 已声明的变量（字符串、数组、对象、函数）不可以删除；

    4. 未声明的变量，或window下的变量，可以删除；

       ```javascript
       var name = 1; // 已声明
       age=2; // 未声明
       this.width = 3; // window下的变量
       delete name;
       delete age;
       delete width;
       console.log(typeof name) // number
       console.log(typeof age) // undefined
       console.log(typeof width) // undefined
       ```

3. 增/改/查：

   1. `obj.x` 点语法：当属性名为常量时使用，属性不存在则返回`undefined`。

   2. `obj[x]`中括号：当属性名为**变量**时使用，属性不存在则返回`undefined`。注意：中括号中可以使用字符串/字符型表达式（要保证值为字符串）。

   3. `Object.defineProperty`：增加/修改属性（若属性不存在，则添加；若属性存在，则进行修改；一次可修改多个）。

      1. 语法：`Object.defineProperty(obj,proName,desc)`，参数分别为：对象名、属性名、属性描述（值、可写、可枚举、可修改）。

      2. 举例：

         ```javascript
         var obj = {};
         Object.defineProperty(obj, "x", {
             value : 1,
             writable : true,
             enumerable : true,
             configurable : true
         });
         console.log(obj.x);  //1
         ```

4. 查-键值对信息：

   1. 获取 keys：`Object.keys(obj)`：自身可枚举属性，不包含原型链上的；返回数组。
   2. 获取 keys：`Object.getOwnPropertyNames(obj)`：自身所有属性（包括可枚举、不可枚举属性）。
   3. 获取 values：`Object.values(obj)`，与`Object.keys(obj)`对应，返回数组。
   4. 获取 items： `Object.entries(obj)`，示例`[[key1,value1],...]`，与`Object.keys(obj)`对应，返回数组。

5. 判断：

   1. key 存在性：
      1. `'key' in obj`：key必须为字符串，返回布尔值。
      2. `obj.x/obj['x']`：对象/原型链上是否存在，不存在返回undefined。
      3. `obj.hasOwnProperty(key)`：对象自身是否存在，不存在返回undefined
   2. 判断对象是否为空：`Object.keys(obj).length==0` 或者`Json.stringify(obj)=='{}'`

6. 遍历

    1. `for(let k in obj){}`：遍历obj中每个“可枚举”属性，包括**自身对象上的、以及原型链上的属性**。(k为属性名)

    2. `Object.keys(obj).forEach();` （仅包含**自身的属性**）

    3. `Object.getOwnPropertyNames(obj).forEach()`：自身所有属性（包括可枚举、不可枚举属性）。

       ```javascript
       function Person() {
           this.name = "Ykx";
       };
       Person.prototype.School = 'Tust'; // 原型链上的属性
       Object.defineProperty(ykx, "sex", {
           value: "male",
           enumerable: false // 不可枚举属性
       });
       let ykx = new Person();
       for (let key in ykx) {
           console.log(key) //name, School
       }
       Object.keys(ykx).forEach(function(key) {
           console.log(key) //name
       });
       Object.getOwnPropertyNames(ykx).forEach(key=>{
           console.log(key) //name, sex
       })
       ```

7. 拷贝

   1. `let newObj = Object.assign(tar,source,...)`：将所有可枚举属性的值从一个或多个源对象分配到目标对象，返回目标对象。（浅拷贝，返回新对象，会修改tar）
   2. `let newObj = JSON.parse(JSON.stringify(obj))`：深拷贝，但无法拷贝方法和undefined值。

### 函数

1. 入参非空检查：参数赋值
2. 获取深层树形结构对象的属性，需层层判空：使用`lodash get`，get(obj, 对象属性)

### Map

1. 创建：`let map = new Map()`
2. 增、改：`map.set(key,value)`
3. 删：
   1. 删除某个key：`map.delete(key)`，返回布尔。
   2. 清空map：`map1.clear()`。
4. 查：
   1. 键/值/键值对[key,value]：`map.keys(),map.values(),map.entries()`，返回迭代器。
   2. 某个key的值：`map.get(key)`，返回值/`undefined`。
5. 遍历：`map.forEach([value,key,map])`
6. 判断：
   1. 判断key是否存在：`map.has(key)`，返回布尔值。
   2. 大小：`map.size`。

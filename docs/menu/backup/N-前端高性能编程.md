## JS

### 数组

1. 扁平化：限2层嵌套

   ```js
   let arr = [1,[2,5],[6,7],9]
   let fArr = [].concat(...arr)
   ```

2. 数组拷贝/拼接：扩展运算符代替concat

   ```js
   // bad
   const array1 = [1,2,3,4];
   const array2 = [5,6,7,8];
   const array3 = array1.concat(array2);
   
   // good
   const array1 = [1,2,3,4];
   const array2 = [5,6,7,8];
   const array3 = [...array1, ...array2]
   ```

3. 是否存在数组中：some代替forEach

   ```js
   // bad
   const myArray = [{a: 1}, {a: 2}, {a: 3}];
   let exist = false;
   myArray.forEach( item => {
    if (item.a === 2) exist = true
   })
   
   // good
   const myArray = [{a: 1}, {a: 2}, {a: 3}];
   const exist = myArray.some( item => item.a == 2)
   ```

4. 数组去重：Set

   ```js
   const arr = [1,1,3,2,2,5,1,5];
   const uniArr = [...new Set(arr)];
   ```

5. 空数组填充：fill

   ```js
   let arr = Array(3).fill('') //用空字符串填充 ['','','']
   ```

6. 获取数组最后的元素：slice(-index) 倒叙获取

   ```js
   let arr = [1,2,3,4]
   arr.slice(-2) // [3,4]
   ```

7. 

### 对象

1. 使用对象方法简写

   ```js
   // bad
   const customObject = {
     val: 1,
     addVal: function () {}
   }
   // good
   const customObject = {
     val: 1,
     addVal(){}
   }
   ```

2. 创建对象的值：同名不需要做映射

   ```js
   // bad
   const value = 1;
   const myObject = {
     value: value
   }
   // good
   const value = 1;
   const myObject = {
     value
   }
   ```

3. 给对象赋值：拷贝代替枚举

   ```js
   // bad
   const object1 = { val: 1, b: 2 };
   let object2 = { d: 3, z: 4 };
   object2.val = object1.val;
   object2.b = object1.b;
   
   // good
   const object1 = { val: 1, b: 2 };
   const object2 = { ...object1, d: 3, z: 4 }
   ```

4. 从对象获取值：解构赋值

   ```js
   // bad
   const object1 = { a: 1 , b: 2 };
   const a = object1.a;
   const b = object1.b;
   
   // good
   const object1 = { a: 1 , b: 2 };
   const { a, b } = object1;
   ```

5. 对象支持动态属性（ES6）

   ```js
   let dKey = 'age'
   let user = {
       name: 'tom',
       [dKey]: 12
   }
   ```

6. 

### 其他

1. 常量使用const代替var

   常量是永远不变的变量，这样声明变量可以确保它们永远不变。
   
2. 使用let替换变量，而不是var

3. 声明对象：用快捷方式声明对象

   ```js
   // bad
   const myObject = new Object();
   // good
   const myObject = {};
   ```

4. 使用模板字符串，进行字符串拼接

   ```js
   // bad
   const myStringToAdd = "world";
   const myString = "hello " + myStringToAdd;
   
   // good
   const myStringToAdd = "world";
   const myString = `hello ${myStringToAdd}`;
   ```

9. 获取对象的多个属性

   ```js
   // bad
   function getFullName(client){
     return `${client.name} ${client.last_name}`;
   }
   
   // good
   function getFullName({name, last_name}){
     return `${name} ${last_name}`;
   }
   ```

10. 创建函数

    ```js
    // old
    function myFunc() {}
    
    // good
    const myFunc = function() {}
    
    // perfect
    const myFunct = () => {}
    ```

12. 设置方法传参默认值

    ```js
    // bad
    const myFunct = (a, b) => {
      if (!a) a = 1;
      if (!b) b = 1;
      return { a, b };
    }
    
    // good
    const myFunct = (a = 1, b = 1) => {
      return { a, b };
    }
    ```

13. 使用reduce代替forEach和for来求和

    ```js
    // bad
    const values = [1, 2, 3, 4, 5];
    let total = 0;
    values.forEach( (n) => { total += n})
    
    // good
    const values = [1, 2, 3, 4, 5];
    const total = values.reduce((total, num) => total + num);
    ```

9. 使用三目运算符简化判断

      ```js
   // bad
   const a = 5;
   let b;
   if (a === 5){
     b = 3;
   } else {
     b = 2;
   }
   
   // good
   const a = 5;
   const b = a === 5 ? 3 : 2;
      ```

10. 双位运算符替代`Math.floor`，仅限正数

    ```js
    Math.floor(3.6) // 3
    ~~3.6 //3
    
    Math.floor(-3.6) // -4
    ~~-3.6 //-3
    ```

11. 取整：数字|0，正负都适用

    ```js
     1.3|0  // 1
    -1.9|0 // -1
    ```

12. 缩短条件语句

    ```js
    // bad
    if(flag){
        aFun();
    }
    // good
    flag && aFun();
    ```

    


​    

​    

​    


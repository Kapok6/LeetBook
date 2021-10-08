## 手册

1. `TypeScript`的核心原则：对值所具有的*结构*进行类型检查。
2. 待学习点：可索引类型，

### 基础类型

1. 类型分类：string、 number、 boolean、 null、 undefined、数组、元组、枚举enum、any、 void、never、object。

2. 数组：

   1. 规则：元素类型需一致。

   2. 用法：

      1. 可变：`let arr:number[]=[]; 或 let arr1: Array<number>=[]`。
      2. 只读：`let arr: ReadonlyArray<number> =[1,2]`，赋值后无法修改，但可以通过断言重写。

      ```typescript
      let arr: ReadonlyArray<number> =[1,2]
      arr[0]=12; // error
      newArr = arr as number[]; //
      ```

3. 元组：元素类型可不同，`let tup:[string,number]`。

4. 枚举：枚举值默认从0开始编号，可手动赋值,`enum xxx {x=3,x,x..}`。

5. 任意类型：any

   1. 语法：`let notSure:any=4`，不进行类型检查，而是在编译阶段再进行检查。
   2. 应用：声明数组，仅知道部分数据的类型，`let arr:any[]`

6. 没有任何类型：void

   1. 规则：只能赋值为`null 或 undefined`
   2. 应用：函数无返回，`function t():void{}`

7. 类型断言：

   1. 作用：类似类型转换，只在编译阶段起作用，不影响运行。

   2. 语法：尖括号或者as

      ```typescript
      let len: number=(<string>val).length
      let len: number=(val as string).length
      ```

### 变量声明

1. TY为JS超集，推荐let,const替代var。

2. var弊端：

   1. 作用域：

      1. 规则：函数作用域，`var`声明可以在包含它的函数，模块，命名空间或全局作用域内部任何位置被访问 
      2. 问题：重复声明不报错，易引发错误。

   2. 异常捕获：

      ```typescript
      // 问题
      for (var i = 0; i < 10; i++) {
          setTimeout(function() { console.log(i); }, 100 * i);
      } // 输出 10，..., 10
      
      // 解决：使用立即执行的函数表达式IIFE来捕获每次迭代时i的值
      for (var i = 0; i < 10; i++) {
          (function(i) {
              setTimeout(function() { console.log(i); }, 100 * i);
          })(i);
      }
      ```

      原因：我们传给`setTimeout`的每一个函数表达式实际上都引用了相同作用域里的同一个`i`。 `setTimeout`在若干毫秒后执行一个函数，并且是在`for`循环结束后。 `for`循环结束后，`i`的值为`10`。

3. let

   1. 块作用域：块外无法访问，（在`catch(e)`语句里声明的变量也具有同样的作用域规则）。

   2. 暂时性死区（无变量提升）：在变量声明之前读/写，会抛出引用错误（ReferenceError）。即：直到被声明为止，都处在一个“暂时性死区”。（**注意，变量在声明前可以被获取，只是不能提前执行**）

      ```typescript
      function foo() {
          // okay to capture 'a'
          return a;
      }
      // 不能在'a'被声明前调用'foo'，否则 运行时应该抛出错误
      let a=3;
      foo();
      ```

   3. 重定义及屏蔽：

      1. 不能重复声明

      2. 在函数内声明，需要保证是明显不同的块，内层会屏蔽外层的值。

         ```typescript
         function f(x) {
             let x = 100; 
         } //Uncaught SyntaxError: Identifier 'x' has already been declared
         
         function f(condition, x) {
             if (condition) {
                 let x = 100; // Yes, 与参数不同块
                 return x;
             }
             return x;
         }
         ```

   4. 作用域及变量获取（当`let`声明出现在循环体里时拥有完全不同的行为。 不仅是在循环里引入了一个新的变量环境，而是针对 *每次迭代*都会创建这样一个新作用域。 ）

      ```typescript
      for (let i = 0; i < 10 ; i++) {
          setTimeout(function() {console.log(i); }, 100 * i);
      } // 0,1,2,3...
      ```

4. const

   1. 块作用域
   2. 暂时性死区
   3. 声明时需要赋值，且之后无法修改

5. 解构

   1. 数组解构

      ```typescript
      // 全部解构
      let [first, second] = [1,2]
      
      // 忽略部分值
      let [first, ...rest] = [1, 2, 3, 4];
      let [first] = [1, 2, 3, 4];
      let [, second, , fourth] = [1, 2, 3, 4];
      
      // 应用：元素交换/赋值
      [first, second] = [second, first];
      
      // 应用：函数传参
      function f([first, second]: [number, number]) {
          console.log(first,second);
      }
      ```

   2. 对象解构

      ```typescript
      // 用已声明的进行赋值
      let o = {
          a: "foo",
          b: 12,
          c: "bar"
      };
      let { a, b, c } = o;
      
      // 用未声明的进行赋值
      // 注意：需要用括号将它括起来，因为Javascript通常会将以 { 起始的语句解析为一个块
      ({a,b}={ a: "foo", b: 12, c:"bar" });
      
      // 忽略部分值
      let { a, ...passthrough } = o;
      let {a} = 0;
      
      // 重命名，并指定类型
      let { a: newName1, b: newName2 } = o;
      let {a: newName1, b: newName2 }: {a: string, b: number} = o; // 指定类型
      
      // 应用：函数传参设置默认值（结构复杂时，不建议使用）
      function func(wObj: { a: string, b?: number }) {
          let { a, b = 1001 } = wObj;
      }
      ```

### 接口

1. 作用：为类型命名，为代码或第三方代码定义契约。

2. 规则：

   1. 属性控制：必填、可选、只读 `readonly`。
   2.  `readonly与const`取舍： 做为变量使用的话用 `const`，若做为属性则使用`readonly`。
   3. 编译时会检查是否存在已定义的约束。

3. 不同类型的属性：必填、可选、只读

   ```typescript
   // 用接口定义类型
   interface LabelType = {
       label: string, // 必填属性
       color?: string, // 可选属性
       readonly x:number // 只读变量，赋值后无法再改变
       test: ReadonlyArray<number> // 只读数组
   }
   
   // 使用‘接口’控制传参类型
   function printLabel (labelObj: LabelType){
   	console.log(labelObj.label);
   }
   let obj = {label:'size 10'}
   printLabel(obj);
   ```

4. 额外属性检查

   ```typescript
   interface SquareConfig {
     color?: string;
     width?: number;
   }
   function createSquare(config: SquareConfig) {
   }
   // 入参为colour，与定义中的color不符，虽然非必选，但是会报错，coulor 不存在
   let mySquare = createSquare({ colour: "red", width: 100 });
   ```

   屏蔽额外属性的检查：类型断言、或 索引签名

   ```typescript
   // 类型断言
   let mySquare = createSquare({ colour: "red", width: 100 } as SquareConfig);
   // 索引签名
   interface SquareConfig {
     color?: string;
     width?: number;
     [propName: string]: any; // 非 color 或 width 的任意值
   }
   ```

5. 函数类型：

   1. 接口定义内容：参数列表、返回值类型。
   2. 注意：函数的参数名不需要与接口里定义的名字相匹配，要求对应位置上的参数类型是兼容的，不指定类型也可以。

   ```typescript
   // 包含参数列表、返回值类型
   interface SearchFunc {
     (source: string, subString: string): boolean;
   }
   
   let mySearch: SearchFunc;
   
   // 名称不匹配
   mySearch = function(src: string, sub: string): boolean {
     let result = src.search(sub);
     return result > -1;
   }
   
   // 不指定类型
   mySearch = function(src, sub) {
       let result = src.search(sub);
       return result > -1;
   }
   ```

6. 可索引类型：

   1. 作用：索引签名，描述了对象索引的类型，还有相应的索引返回值类型。

   2. 规则：

      1. 支持两种索引签名：字符串和数字；
      2.  可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型。

   3. 示例：

      ```typescript
      interface StrArr {
      [index:number] :string
      }
      
      let cArr = StrArr;
      cArr = ['bob','tom']
      let mName:string = cArr[0]
      ```

7. 类类型

   1. 实现接口：

      1. 接口可以明确的强制一个类去符合某种契约。
      2. 接口描述了一个类的公共部分，不会检查私有部分。

      ```typescript
      interface Shape {
          size: number;
          setSize(s:number) // 声明
      }
      class Rec implements Shape{
          size: number;
          setSize(s:number){ // 实现
              this.size = s;
          }
      }
      ```

   2. 类具有两个类型：静态部分、实例部分

      ```typescript
      interface ClockConstructor {
          new (hour: number, minute: number);
      }
      
      class Clock implements ClockConstructor {
          currentTime: Date;
          constructor(h: number, m: number) { }
      }
      ```

      


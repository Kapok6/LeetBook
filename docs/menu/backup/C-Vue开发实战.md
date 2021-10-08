

[TOC]

# 1. 开发实战

- From 《vue开发实战》
- Teacher：唐金州 一点资讯技术专家、Ant Design Vue作者

## 1.1. 基础：vue 核心知识点

### 1.1.1. 简介

**特点：**

1. 轻量：20kb min+gzip；react 40kb；angular:60kb;
2. 渐进式：可以只用 vue 种的一部分，学多少用多少；
3. 学习成本低：底层成本基于 html，模板语法；
4. 响应式更新机制；
5. 虚拟DOM；
6. 组件式开发；
7. 允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统；
8. 适用场景：用户频繁交互的场景，也是相对于传统模式的优势；资料参考：https://segmentfault.com/a/1190000018634744

**问题：**

1. vue中为何可以使用变量、循环指令等：vue中有模板解析函数，可对html代码进行解析编译，从而转变成渲染函数，渲染函数执行后就变成了虚拟DOM节点树。

   ![vue模板解析过程](./img/vue开发实战/vue模板解析过程.png)

### 1.1.2. Vue实例

1. 创建实例》设置挂载点》通过模板语法实现数据渲染

   ```html
   <div id="app">
     {{message}} {{message + message}}
     <div :id="message"></div>
   </div>
   <script>
     var vm = new Vue({
       //根实例
       el: "#app", // 设置挂载点，绑定dom元素
       data: {
         //数据对象，共享一份
         message: "hello world",
       },
     });
   </script>
   ```

### 1.1.3. 组件

1. 使用组件是为了复用。
2. 组件本质：一个拥有预定义选项的一个 Vue 实例。

#### 1.1.3.1. 组件注册

1. 组件作用：复用。

2. 组件本质：一个拥有预定义选项的一个 Vue 实例。

3. 组件注册&使用

   ```html
   <div>
     <!-- 对于未在prop里声明的传参，如item-class，会自动挂在到模板的根节点上 -->
     <todo-item v-for="item in list" item-class="test111" :title="item.title"
       :del="item.del"></todo-item>
   </div>
   <script>
     // 组件注册，组件名需要全局唯一
     Vue.component("todo-item", {
       template: `<li>
              <span v-if="!del">{{title}}</span>
              <span v-else style="text-decoration: line-through">{{title}}</span>
              <button v-show="!del">删除</button>
         </li>`,
   
       props: {
         title: String, //设置类型
         del: {
           type: Boolean,
           default: false, //设置默认值
         },
       },
       //简写：props:['title']
   
       data: function () {
         //通过方法，返回一个新的对象（与vue实例有区别）
         return {
           list: [
             { title: "课程1", del: false },
             { title: "课程2", del: true },
           ],
         };
       },
       // 简写 data(){return {}}
     });
   </script>
   ```

4. 组件注册&创建 vue 实例，data 有区别：
   - 创建 vue 实例：data 直接返回一个对象，全局共用。
   - 组件注册：data 通过方法返回一个对象，保证每次复用时，都拥有独立的一份 data 对象，互补影响。

5. 定义data的几种写法的区别

   - vue实例中，3种方法都可用；但vue组件中，只能用方法2/3。（因为后者可以实现，复用后数据互不影响，因为函数自己拥有私有作用域）

   ```javascript
   var app = new Vue({
   　　el: '#app',
       // 方法1：data为一个对象
   　　data: {
   　　　　mess: 'aerchi'
   　　}
       // 方法2：data为一个函数
   	data: function() {
   　　　　return { mess: 'aerchi' }
   	}
       // 方法3：data为一个函数（ES6写法）
   	data() {
   　　　　return { mess: 'aerchi' }
   	}
   })
   ```

#### 1.1.3.2. 单文件组件

1. 形式：vue文件。
2. `Vue.component`注册组件的弊端
   1. **全局定义 (Global definitions)** 强制要求每个 component 中的命名不得重复
   2. **字符串模板 (String templates)** 缺乏语法高亮，在 HTML 有多行的时候，需要用到丑陋的 `\`
   3. **不支持 CSS (No CSS support)** 意味着当 HTML 和 JavaScript 组件化时，CSS 明显被遗漏
   4. **没有构建步骤 (No build step)** 限制只能使用 HTML 和 ES5 JavaScript，而不能使用预处理器，如 Pug (formerly Jade) 和 Babel
3. 单文件组件的优势：可解决上述问题，且可以使用 webpack 或 Browserify 等构建工具。

#### 1.1.3.3. 获取组件实例

1. 获取子组件实例的方法：`this.$refs.xxx`。（注意，只有组件，通过refs才能拿到实例）

   ```html
   <p ref='plabel'>helllo</p>
   <child-comp ref='child'></child-comp>
   <script>
   let nDom = this.$refs.plabel; // 拿到的为dom元素
   let nExam = this.$refs.child  // 拿到的为子组件的实例
   </script>
   ```

#### 1.1.3.4. *获取跨级组件实例

1. 跨层级孩子组件/跨层级兄弟节点，获取实例：

   1. 递归查找：繁琐、低效
   2. `callback ref`：主动通知(`setXxxRef`)、主动获取(`getXxxRef`)

2. 举例：A（BC（E F）D（G H I））

   ```javascript
   provide(){
       return {
           setChildRef:(name,ref)=>{
               this[name]=ref;
           },
            getChildRef:name=>{
               return this[name];
           },
           getRef:()=>{
               return this;
           }
       }
   }
   ```

#### 1.1.3.5. *函数式组件

1. 定义：函数，（入参：渲染上下文，返回值：渲染好的html）

   - 参数context的属性：props，children，slots，parent，listeners，injections，data。（可使用参数解构）

   ```javascript
   // 创建js文件，定义如下：
   export default{
       name: 'fun-btn',
       functional: true, //必须
       render: (h,context)=>{ // 必须
           return context
       }
   }
   ```

2. 特点：无状态、无实例（无this上下文）、无生命周期。

3. 使用：

   1. 事件定义：无实例，所以只能父组件传递。

      ```vue
      <!--模板层-->
      <template>
        <FunctionalButton @click="log">
          Click me
        </FunctionalButton>
      </template>
      ```
      
      ```javascript
      // render参数：listeners：监听事件，props属性
      render(createElement, { props, listeners, children }) 
        {
          return createElement(
            'button',
            { attrs: props,
              // 绑定事件
              on: { click: listeners.click }
            },
            children
          );
         }
      };
      
      // render参数可“简化”
      render(createElement, { data, children })
      {
          return createElement( 'button', data, children );
      }
      ```

4. 功能：

   1. 在模板中，生成临时变量。
   2. 程序化地在多个组件中选择一个来代为渲染。
   3. 在将 `children`、`props`、`data` 传递给子组件之前操作它们。

### 1.1.4. 模板语法

1. 对于单文件组件，组成部分：
   1. 模板层：`<template></template>`
   2. JS：`<script></script>`
   3. 样式：`<style></style>`

2. 模板语法
   1. 插值（在标签内插入内容）：
      1. 文本`{{变量/计算属性}}`
      2. HTML：`<span v-html="xxx"></span>`
      3. 属性：`v-bind`，简写`:`
      4. 事件：`v-on`，简写`@`
      5. JS表达式：`{{JS表达式}}`或`v-bind="JS表达式"`
      6. 注意：每个绑定只能包含**单个表达式**；且语句、流控制不生效。
   2. 指令：见后续章节。
   3. 缩写：属性绑定：，事件绑定：`v-on》@`。

### 1.1.5. 事件

1. 调用方式
   - 直接绑定：`v-on:方法种类/@方法种类=方法名`
   - 内联：`v-on:方法种类='方法定义'`
2. 事件修饰符特点
   修饰符可以串联；顺序很重要；
3. 事件修饰符种类

   - **.stop**：阻止单击事件继续传播（冒泡）

     > `<a v-on:click.stop="doThis"></a>`

   - **.prevent**：提交事件不再重载页面

     > `<form v-on:submit.prevent="onSubmit"></form>` > `<a v-on:click.stop.prevent="doThat"></a>`

   - **.capture**： 添加事件监听器时使用事件捕获模式，即内部元素触发的事件先在此处理，然后才交由内部元素进行处理

     > `<div v-on:click.capture="doThis">...</div>`

   - **.self**：只当在 event.target 是当前元素自身时触发处理函数

     > `<div v-on:click.self="doThat">...</div>`
     > v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。

   - **.once**：

   - **.passive**：

### 1.1.6. 指令

#### 1.1.6.1. 指令的本质

1. 对指令的理解：标志位，语法糖，vue 底层根据标志做相应的处理。当编译阶段，会将模板层中的语法糖，编译为JS代码

#### 1.1.6.2. 常用指令

1. 文本插值，双向数据绑定：`{{变量/计算属性}}`。

2. 内容替换：v-text，用字符串替换dom元素内的值。

   ```html
   <!-- div元素会显示hello vue -->
   <div v-text='hello vue'>hello word</div>
   ```

3. 内容替换：v-html，用html语言替换dom元素的值，一般不建议使用，会有xss风险。

   ```html
   <div v-html='<span style=&quot;color:red &quot;>hello vue</span>'></div>
   ```

4. 数据双向绑定：v-model。

5. 绑定元素属性:`v-bind/，如`:title,:id`。

6. 绑定事件：v-on/@，如@click。

7. 是否加载：`v-if/v-else-if/v-else`。

8. 是否显示：`v-show`。

9. 对象遍历：`v-for`，注意key不要用index。

10. 插槽：v-slot。

11. 屏蔽双括号，不编译，直接输出字符串：v-pre。

    ```html
    <div v-pre>{{this will not be compiled}}</div>
    ```

12. 保证只执行一次：v-once。

    ```html
    <!-- 双括号中的变量，只会编译一次，无论number改变多少次 -->
    <div v-once>{{number}}</div>
    ```

13. v-cloak，使用频率比较低。

#### 1.1.6.3. 指令使用注意事项

1. v-for和v-if不能一起使用：

   1. 原因：`v-for`比`v-if`优先级高，所以嵌套使用的的话，每次`v-for`都会执行`v-if`，造成不必要的计算，影响性能。

   2. 解决方案：将`v-for`的对象，替换成计算属性。

      ```vue
      <template>
      <ul>
          <li v-for="user in activeUsers" :key="user.id">{{ user.name }}</li>
      </ul>
      </template>
      <script>
      export default{
      computed: {//替换成计算属性，再遍历
      	activeUsers: function () {
      		return this.users.filter(function (user) { return user.isActive })
      		}
      	}
      }
      </script>
      ```


#### 1.1.6.4. 自定义指令

1. 通过5个生命周期的钩子，实现自定义指令：
   1. bind：组件创建/`v-if='true'`时
   2. inserted：组件创建//`v-if='true'`时
   3. update：组件更新时
   4. componentUpdated：组件更新时
   5. unbind：组件销毁/用`v-if='false'`时

#### 1.1.6.5. 双向绑定

1. 语法：v-model。（指令的一种）
2. 双向绑定和单向数据流不冲突，本质上仍为单向数据流。
   - `<input v-model="mes">`等同于`<input value="mes" @input="handleChange">`
3. v-model 在不同元素中，使用不同的属性，并抛出不同的事件
   - input：使用 value 属性和 input 事件；
   - text 和 textarea ：使用 value 属性和 input 事件；
   - select：使用 value 属性和 change 事件；
   - checkbox 和 radio：使用 checked 属性和 change；
4. 原理：MVVM设计模式。采用**数据劫持**结合**发布者-订阅者**模式的方式来实现，通过`Object.defineProperty( )`来劫持各个属性的`setter、getter`，在数据变动时，发布消息给**订阅者**，触发相应监听回调。[链接](https://www.jianshu.com/p/3e6b89d7d7ad)


### 1.1.7. 计算属性&监听属性

1. 计算属性computed
   1. 目的：减少模板中的计算逻辑
   2. 特点：会进行数据缓存，只有依赖数据发生变化，且在模板层中有使用，才会重新计算（大数据量计算时，优势明显）
   3. 特点：默认只有`getter`，可省略。
4. 注意：必须依赖固定的数据类型——**响应式数据**
   
   5. 注意：**计算属性内部this指向vue实例，所以不能使用箭头函定义计算属性**。（箭头函数绑定了父级作用域的上下文，所以 this 将不会按照期望指向 Vue 实例，this.a 将是 undefined。）
   
2. 监听器watch

   1. 更加灵活。
   2. watch中可执行任何逻辑，如函数节流、Ajax异步获取数据、操作DOM（不建议）。
   
3. watch和computed对比：

   1. computed能做的，watch都能做，反之不行。
   2. 能用computed，尽量用computed。

4. methods和computed对比：

   1. methods是一个方法，它可以接受参数，而computed 不能.
   2. computed 是可以缓存的，methods 不会。
   3. computed 可以依赖其他 computed，甚至是其他组件的 data。

5. computed使用方式

   ```javascript
    computed: {
        fullName(){
            return this.firstName+this.lastName;
        },
         mapState({age:state=>state.xx})
    }
   ```

6. watch使用方式

   ```javascript
   data(){
       return {
           a:3,
           b:{d:4},
           
       }
   }
   watch: {
       //a:1
       a: function(val, oldVal) {
         this.b.d += 1;
       },
       // b:{d:3}，改变d，用b.d能监听到，用b不能监听到变化。
       "b.d": function(val, oldVal) {
         this.e.f.g += 1;
       },
       // e: {f: {g: 4}}, 改变g时，需要加deep才能监听到变化。
       e: {
         handler: function(val, oldVal) {
           this.h.push("😄");
         },
         deep: true // 深度监听
       },   
   }
   ```


### 1.1.8. 插槽

1. 作用：在模板层中，传递复杂内容的方式。
2. 种类：
   - 作用域插槽
   - 动态插槽
   - 具名插槽
3. 作用域插槽
4. 动态插槽
5. 具名插槽

### 1.1.9. 虚拟 DOM

#### 1.1.9.1. 浏览器渲染过程

![](./img/vue开发实战/渲染引擎工作流程.png)

1. **构建 DOM 树**：用 HTML 分析器，分析 HTML 元素，构建一颗 DOM 树(标记化和树构建)。
2. **生成样式表(StyleRules)**：用 CSS 分析器，分析 CSS 文件和元素上的 inline 样式，生成页面的样式表。
3. **构建 Render 树**：将 DOM 树和样式表，关联起来，构建一颗 **Render 树**(这一过程又称为 Attachment)。每个 DOM 节点都有 attach 方法，接受样式信息，返回一个 render 对象。这些 render 对象最终会被构建成一颗 Render 树。
4. **布局(Layout)**：有了 Render 树，浏览器开始布局，为每个 Render 树上的节点确定一个在显示屏上出现的精确坐标。
5. **绘制页面(Painting)**：调用每个节点 paint 方法，根据Render 树和节点坐标，把它们绘制出来。

**【注意】**

1. DOM 树的构建是文档加载完成开始的？NO，构造 DOM 树是一个渐进的过程，渲染引擎会尽快将内容显示在屏幕上，不会等到解析完毕才开始构建。
2. CSS 的解析顺序：是从右往左逆向解析的(从 DOM 树的下－上解析比上－下解析效率高)，嵌套标签越多，解析越慢。
3. Render 树是 DOM 树和 CSSOM 树构建完毕才开始构建的吗？NO,会有交叉，会有一边加载，一遍解析，一遍渲染的工作现象。

#### 1.1.9.2. 虚拟DOM&更新机制

1. JQuery ：
   - 本质：是一个JS库/框架，用于优化HTML文档操作、事件处理、动画设计和Ajax交互。
   - 利：简化了操作 DOM 的 API，通过 JQ 绑定事件，通过事件操作 DOM 元素。
   - 弊：随着系统越来越复杂，导致事件、操作变得复杂，不易开发和维护。（因此诞生了 vue）。
   
2. 虚拟 DOM 
   - 产生背景：JS、JQ 操作 DOM ，或数据发生改变时，如果直接渲染到真实DOM上，会引起DOM树的重绘和重排（回流），开销很大。另外，真实DOM节点上的属性和方法比较多，如果每次生成新的DOM对象，会造成性能浪费。因此，为解决浏览器性能问题，提出虚拟DOM概念。
   - 本质：用JS对象，来模拟真实DOM结点，并用于中间过程的处理。比如一次操作涉及多次DOM更新，则会将多次更新的diff，保存到一个本地JS对象中，最终将JS对象attch到DOM树上，再进行后续操作。（将来可还原为 DOM 树）。
   - 为何用 JS 对象存储 DOM 树结构？操作 JS 速度比操作 DOM 结点更快，方便中间多次操作，性能也更高。
   - 优势：(1)页面的更新会先反应到JS对象上，操作JS更快，从而提供性能；(2)简化操作，开发只需关注数据，避免事件直接操作 DOM。
   
4. vue更新机制：

    ![](./img/vue开发实战/vue虚拟dom流程.png)

    1. 用JS对象模拟DOM树
        - 用JS对象，记录节点类型(如div)、属性（如class）、子节，如：`Element('div',{class:'tName'},{Element(),Element()...})` ；（老的虚拟DOM）
        - 若有更新，用新的JS对象保存；（新的虚拟DOM）
    2. 比较新、老两个虚拟DOM树的**差异**
        - 深度优先遍历（新旧2棵树），利用**diff算法**，得到差异。
    3. 把**差异**一次性渲染到真实的DOM树上(**patch过程**)
        - 遍历dom树，根据step2暂存的当前节点的差异+类型，对当前节点进行DOM操作（**只渲染不同的部分**到视图中，无需改动其他的节点状态）。

4. diff算法 

   1. 定义：一种对比新、旧虚拟节点的算法。
   2. 特点：深度遍历，只会再同层级进行，不会跨层级。（常规不会有跨层级节点的移动）
   3. diff流程： （[参考链接](https://510team.github.io/vue/patch.html), [参考链接1](https://juejin.cn/post/6844903815439712269),）
      1. 如果vnode不存在，而oldVnode存在，则调用invodeDestoryHook进行**销毁**旧的节点。
      2. 如果oldVnode不存在，而vnode存在，则调用createElm**创建**新的节点。
      3. 如果oldVnode和vnode都存在
         - 如果oldVnode不是真实节点，且vue认为两个节点值得比较（即调用sameVnode，若key、sel均相同，则true）：则调用**patchVnode进行patch**，即直接修改现有节点。
         - 如果oldVnode是真实DOM节点，或 vnode 和 oldVnode 不值得比较：则找到 oldVnode.elm 的父节点，根据 vnode 创建一个真实的 DOM 节点，并插入到该父节点中的 oldVnode.elm 位置，并移除旧节点（removeVnodes）。
      4. 返回vnode.elm（vnode对应的真实DOM的引用）。
   4. diff常见场景
      1. 移动：如只有某个节点顺序发生改变。
      2. 删除、新建：如某个节点的子树归属发生改变。
      3. 更新、删除、新建（无key）：如父节点顺序改变，且有子树。则更新父项的值，然后新建子树，并删除原来位置的子树。
      4. 插入：无key：更新、新建；有key：插入
   5. 注意：
      - v-for，不要用index作为key，否则频繁增删，会导致更新问题。（key会用于dom比较，index不变，但可能元素变了。）
      - 对于diff过程，只需了解每种场景下的操作how，无需了解具体的diff细节what。

6. patch：
   
    1. 定义： 通过打补丁的形式充分利用原有的dom进行**增加、删除、移动**的操作，从而避免了重新创建大量dom操作，进一步提升了性能。
    
    2. **patchVnode逻辑**（打补丁 [参考链接](https://510team.github.io/vue/patchVnode.html#what)）
       
       1. 针对vnode和oldVnode,若两者完全一致（==）：不做处理，直接返回；否则：则复制一份旧的真实DOM的引用，命名为elm。
       2. 若两者都是静态节点，且key相同，且vnode是克隆节点或只渲染一次（isCloned, isOnce）：那么只需要替换elm以及componentInstance即可。
       3. 若两者都有text，但值不同：则用vnode.text更新elm的text内容。否则：
          - 若两者均存在children，调用diff后不同：**updateChildren**更新子节点。
          - 若只有vnode存在子节点：则清空elm文本内容，之后为当前节点加入子节点（addVnodes）。
          - 若只有oldVnode存在子节点：则移除所有 elm 的子节点（removeVnodes）。
       
    3. **updateChildren逻辑**：（[参考链接](https://510team.github.io/vue/updateChildren.html#what)）
    
       参数说明
    
       ```js
        *  oldCh:oldVNode chirdren
        *  newCh:newVNode chirdren
        *  StartIdx   开头索引
        *  EndIdx     结束索引
        *  vnode中的key，可以作为节点的标识
       ```
    
       1. `oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx`时：
          - 让oldCh和newCh的首尾指针StartIdx 和 EndIdx，分别向中间靠拢，并两两比较，一共有 4 种比较方式：sameVnode(oldStartVnode, newStartVnode)、sameVnode(oldStartVnode, newEndVnode)、sameVnode(oldEndVnode, newStartVnode)、sameVnode(oldEndVnode, newEndVnode)
          - 若比较不成功，则用key比较。
       2. 当`oldStartIdx > oldEndIdx`时，表示 oldCh 已经先遍历结束，即 newCh 还没有遍历完，则 newStartIdx 和 newEndIdx 之间的 vnode 是新增的，则**插入**新节点。
       3. 当`newStartIdx > newEndIdx`时，表示 newCh 已经先遍历结束，说明 oldCh 中剩余的节点在新的子节点里已经不存在了，此时应该**删除** oldStartIdx 和 oldEndIdx 之间的 vnode。

### 1.1.10. 触发组件更新

1. 数据驱动：是数据驱动的一个视图框架，数据改变时，视图才会更新。

2. 注意：没有特殊情况，尽量不要去操作DOM。

3. 数据来源：
   1. 父元素属性props
   2. 组件自身的状态data
   3. 状态管理器，如vuex,Vue.observable
   
4. 状态data vs 属性props

   1. 状态：组件自身的数据；其改变未必触发更新。
   2. 属性：来自父组件的数据；其改变未必触发更新。

5. 响应式更新

   ![](./img/vue开发实战/数据的响应式更新.png)

   1. vue实例化时，会对data{return{}}中声明的变量，添加一个**代理层**(getter、setter)，来执行相应的操作。

   2. 页面渲染时Render，对于页面(模板层)用到的变量，会放入watcher中进行监测。

   3. 当watcher中的数据发生改变时，才会触发组件更新。

6. 触发组件更新的必备条件

      1. 在data的return下进行了声明（若不在return中声明，则不行；针对一个对象，后期才添加的属性也不行）。
      2. 在模板层中进行了使用。

7. 强制组件更新：`this.$forceUpdate`。
   
### 1.1.11. 生命周期

![生命周期](./img/vue开发实战/vue生命周期.png)

1. 创建、销毁阶段，只执行一次。

2. 过程：开始创建、初始化数据、编译模板、挂在DOM、渲染、更新->渲染、销毁等。

3. 各阶段解析：

   ![生命周期](./Img/面试题/生命周期.png)

#### 1.1.11.1. nextTick

1. 定义：在下次 DOM **更新循环结束**之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

2. [关于异步及回调](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

3.  DOM 更新事件循环

   1. 规则1：Vue是**异步执行DOM更新**的，数据发生变化之后， DOM不会 立即变化，而是按一定的策略进行 DOM 的更新。
   2. 规则2：Vue 在修改数据后，视图不会立刻更新，而是等**同一事件循环**中的所有数据变化完成之后，再统一进行视图更新。
   3. 事件循环：同步代码执行 -> 查找异步队列，推入执行栈，执行Vue.nextTick[事件循环1] ->查找异步队列，推入执行栈，执行Vue.nextTick[事件循环2]...。

4. 用途：需要在视图更新之后，基于新的视图进行操作。

   1. 在 created 和 mounted 阶段，如果需要操作渲染后的试图，需要使用 nextTick 方法。

   2. 元素用v-show控制，当从隐藏变为展示，若要获取焦点，或获取宽高等，需要借助nextTick。

      ```javascript
      this.$refs.myWidth.offsetWidth;
      document.getElementById("keywords").focus();
      ```

   3. 使用 swiper 插件通过 ajax 请求图片后的滑动问题。

#### 1.1.11.2. 创建阶段

1. render阶段：生成虚拟DOM.挂载DOM。

   ![生命周期](./img/vue开发实战/vue创建阶段.png)

#### 1.1.11.3. 更新阶段

![生命周期](./img/vue开发实战/vue更新阶段.png)

1. beforeUpdate：依赖数据改变/$forceUpdate强制刷新》触发beforUpdate》移除已经添加的时间监听器等。
2. render操作：
3. updated：操作DOM添加事件监听器等。
4. 【注意】：步骤1、3，万万不可更改依赖数据（即响应式数据），否则会死循环。

#### 1.1.11.4. 销毁阶段

1. beforeDestroy：移除已经添加的事件监听器，计时器等。
2. destroyed：用的比较少。

### 1.1.12. *高级特性provide/inject

1. 作用：跨层级间的组件通信，主要在开发高阶插件/组件库时使用。并不推荐用于普通应用程序代码中。

2. 使用方法：

   1. 父：通过`provide`提供数据
   2. 子：通过`inject`注入数据

3. `provide：`是一个对象或返回一个对象的函数，其包含可注入其子孙的 property。

   ```javascript
   // 父组件
   provide(){//层级和data并列
       return {
           theme:{
               color:this //直接使用this.color,当this.color更新时，无法触发这里的更新
           }
       }
   },
   data(){
     return{color:'red'}  
   }
   ```
   
4. `inject`：

   1. 是一个字符串

   2. 或是一个对象，对象的 key 是本地的绑定名，value 是：

      1. 在可用的注入内容中搜索用的 key (字符串或 Symbol)
      2. 一个对象，该对象的
         - `from property` 是在可用的注入内容中搜索用的 key (字符串或 Symbol)。
         - `default` property 是降级情况下使用的 value。

   3. 举例

      ```javascript
      //子组件1，对应上述2.1，直接使用父项中的key
      inject:{
          theme:{ //直接使用父项中的key
              default:()=>({}) 
          }
      }
      //子组件2，对应上述2.2，自定义key
      inject:{
          theme1:{ // 自定义key，类似于起别名
              from :'theme', // 父项中的key
              default:()=>({})
          }
      }
      ```


### 1.1.13. *template和JSX

1. JSX：

   1. 是Javascript的语法扩展
   2. 语法：数据绑定使用单大括号
   3. 使用：伴随这react产生的，在vue中，通过一些插件，可以使用JSX

   ```html
   <span>message:{this.msg}</span>
   ```

2. template：

   1. 是一种模板语法（html的扩展）
   2. 语法：数据绑定使用Mustache语法（双大括号）

   ```html
   <span>message:{{msg}}</span>
   ```

3. 两者特点

   1. template：学习成本低、组件作用域CSS、大量内置指令（但灵活性低）。
   2. JSX：灵活。
   3. 相同点：都是语法糖而已，最终都会被编译成`createElement()`语法。
   4. 取舍：组件可以分为两类：1）偏视图表现 2）偏逻辑；大部分为第1种，但若遇到第2种，推荐使用JSX或渲染函数。

## 1.2. 生态：大型 vue 项目所需周边技术

### 1.2.1. 组件通信

#### 父子

   1. 直接父子关系 A>B：props，$emit；

      1. props:设置类型、默认值：`type,default`。

      2. 对于未在 prop 里声明的传参，如 item-class，会自动挂在到模板的根节点上。 
      
         ```html
         <div>
            <todo-item v-for="item in list" item-class="test111" :title="item.title"
         </div>
         ```

   2. 间接父子关系 A>C(A>B>C)：`Vue 2.4` 提供了`$attrs` 和 `$listeners`。

      ```javascript
      //A组件
       <B :messagec="messagec" :message="message" v-on:getCData="getCData" v-on:getChildData="getChildData(message)"></B>
      //B组件
      //C组件中能直接触发 getCData 的原因在于：B组件调用 C组件时，使用v-on绑定了 $listeners属性，通过v-bind绑定$attrs属性，C组件可以直接获取到A组件中传递下来的 props（除了 B组件中 props声明的）
      <C v-bind="$attrs" v-on="$listeners"></C>
      props: ['message'],
      //C组件
      <input v-model="$attrs.messagec" @input="passCData($attrs.messagec)">
      ```

   3. 深层父子关系 `provider`和`inject` ：在父组件中通过 `provider` 来提供属性，然后在子组件中通过 inject 来注入变量。不论子组件有多深，只要调用了 `inject` 那么就可以注入在 provider 中提供的数据，而不是局限于只能从当前父组件的 prop 属性来获取数据，只要在父组件的生命周期内，子组件都可以调用。

   5. `$parent` 和 `$children`: 

      直接操作父子组件的实例。`$parent` 就是父组件的实例对象，而 `$children` 就是当前实例的直接子组件实例了，不过这个属性值是数组类型的，且并不保证顺序，也不是响应式的。

   6. `$boradcast` 和 `$dispatch`: 不过只是在 `Vue1.0` 中提供了，而 `Vue2.0` 被废弃了，

#### 兄弟

   1. EventBus(小项目)：                                                                                                                                                                   

      `EventBus`通过新建一个`Vue`事件的`bus`对象，然后通过 `bus.$emit` 触发事件，`bus.$on` 监听触发的事件。


#### vuex

#### 缓存

1. localstorage

   

2. sessionstorage

### 1.2.2. 状态管理 vuex

####  vuex是什么

1. 产生背景：当遇到**多个组件共享状态**时，单向数据流的简洁性很容易被破坏。而provide/inject适合小型项目，因此需要一个状态管理机制。

   - 多个视图依赖于同一状态。
   - 来自不同视图的行为需要变更同一状态。
   
2. 是什么？状态管理器，用于集中管理一个单页应用中的各种状态。

3. 特点：

   1. 状态存储是响应式的，store中的状态改变，相应组件也会得到更新。
   2. 状态改变唯一途径就行显示地提交（commit）mutation，方便跟踪状态变化，方便调试。

4. 3部分

   - state：驱动应用的数据源；
   - view：以声明方式将state映射到视图；
   - action：响应在view上的用户输入导致的状态变化；

5. 运行机制

   ![](./img/vue开发实战/vuex运行机制.png)

6. 核心概念/属性：

   - state：取值，提供一个响应式数据。
   - getters：取值，只有当它的依赖值发生了改变才会被重新计算。（类计算属性）。
   - mutation：赋值，更改store中状态的方法（类似方法，同步）。`store.commit(mutation名)`
   - actions：赋值，触发mutaion方法，支持异步操作。
   - modules：模块化。Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割。（由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。）

7. vuex是通过什么提供响应式数据的呢？`new Vue({})`

8. store是如何挂在到this实例上的？

#### 1.2.2.3. 创建/声明模块

引入&注入（将状态注入每个子组件中）： 

```javascript
import Vuex from 'vuex'; 
Vue.use(Vuex) // 将状态注入每个子组件中
const store = new Vuex.Store({ })
```

#### 1.2.2.4. state：获取状态

1. 初始化

   ```javascript
   const store = new Vuex.Store({
   	state:{
           count: 5
       }
   })
   ```

2. 取值1：直接取值，`this.$store.state.xxx`

   ```javascript
   computed:{
       count(){
   		return this.$store.state.xxx
       }
   }
   ```

3. 取值2：`mapState`，当一个组件需要**获取多个状态**的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性。从而将store中的state状态，统一映射到局部计算属性中。：

   1. 若映射时需要重命名，可使用`{A:B,...}`json对象形式
   2. 若映射时无需重命名，可使`['oName',...]`用字符串数组
   3. 可使用`...扩展运算符`，进行对象混入

   ```javascript
   computed:mapState({
    // 映射时，可进行重命名
       newCount: state => state.count,
       countAl(state){
   		return state.count+state.localCount
       }
   })
   
   // 映射时，无需重命名：当映射的计算属性名称和state子节点同名，可给mapState传“字符串数组”
   computed:mapState(['count'])
   
   // 与局部计算属性合并
   computed:{
       localComputed(){},
       ...mapState({
           // ...
       })
   }
   ```

#### 1.2.2.5. getter：获取派生状态

1. 使用场景：需要对已有state进行扩展时，且多个组件需要用到该属性。

2. 特点：会根据它的依赖值进行**缓存**，只有改变时，才会重新计算。

3. 定义及参数`(state,getters)`

   ```javascript
   const store = new Vuex.Store({
       state: {
         todos: [
            { id: 1, text: '...', done: true },
            { id: 2, text: '...', done: false }
         ]
       },
   	getters:{
          // 定义对象，可接受state作为第1个参数
          doneTodos: state => {
          return state.todos.filter(todo=>todo.done)
          },
   	   // 定义对象，可接收getters作为第2个参数 
          doneTodosCount:(state,getters)=>{
            return getters.doneTodos.length
          },
          // 定义方法，可接收外部传参id
          getTodoById: (state) => (id) => {
            return state.todos.find(todo => todo.id === id)
          }
       }
   })
   ```

4. 取值1：直接取值，`this.$store.getters.xxx`通过属性/方法，进行取值

   ```javascript
   // 通过属性
   this.$store.getters.doneTodos // [{ id: 1, text: '...', done: true }]
   
   // 通过方法访问
   store.getters.getTodoById(2) // { id: 2, text: '...', done: false }
   ```

6. 取值2：将store中的getter映射到局部计算属性，`mapGetters`

   - 可使用`...扩展运算符`，进行对象混入
   - 若映射时无需重命名，可使`['oName',...]`用字符串数组
   - 若需要重命名，可使用`{A:B,...}`json对象形式

   ```javascript
   import { mapGetters } from 'vuex'
   export default {
     computed: {
     	// 使用对象展开运算符将 getter 混入 computed 对象中
       ...mapGetters([
         'doneTodosCount', // 无需重命名
       ])
    	...mapGetters({ 
     		doneCount: 'doneTodosCount'// 重命名为doneCount
   	})
     }
   }
   ```

#### 1.2.2.6. Mutation：更改状态（同步）

1. 作用：是更改状态的唯一方法（提交`mutation`）。

2. 定义：类似事件，用于更改状态。每个mutation包含一个“事件类型”type，和一个回调函数（用于更改状态）。

   ```javascript
   const store = new Vuex.Store({
     state: { count: 1 },
     mutations: {
       // 事件类型:increment；回调函数：函数体中的内容
       increment (state,playload) { //额外的参数，就叫“载荷”
         state.count+=playload.amount // 变更状态
       }
     }
   })
   ```

3. 赋值1：直接赋值，`this.$store.commit('xxx')`。

   ```javascript
   // 1.1 载荷为一个单一变量
   store.commit('increment', 10)
   // 1.2 载荷为一个对象
   store.commit('increment', {
     amount: 10
   })
   // 1.3 已对象方式提交
   store.commit({
     type: 'increment',
     amount: 10
   })
   ```

4. 赋值2： 将组件中的 methods 映射为 `store.commit` 调用，使用`mapMutations` 辅助函数。
   
   ```javascript 
   import { mapMutations } from 'vuex'
   
   export default {
     methods: {
       ...mapMutations([
         'increment', // `this.$store.commit('increment')`
   
        // 支持载荷 `this.$store.commit('incrementBy', amount)`
         'incrementBy' 
       ]),
       ...mapMutations({
         // `this.$store.commit('increment')`
         add: 'increment' 
       })
     }
   }
   ```


4. 注意事项

   1. store中的状态是响应式的，所以状态改变时，监视状态的vue组件也会自动更新。

   2. ∴ 最好提前在你的 store 中初始化好所有所需属性。

   3. 在对象上添加新属性时：

      ```javascript
      Vue.set(obj, 'newProp', 123)
      // 或使用扩展符
      state.obj = { ...state.obj, newProp: 123 }
      ```
   4. mutation必须为同步函数。

5. 使用常量替代 Mutation 事件类型，同时用单独文件归档常量，方便阅读和多人协作的大型项目（在Flux实现中很常见）。


#### 1.2.2.7. Action：更改状态（异步）

1. 定义：类似mutaion，区别在于

   1. `action`提交的是`mutaion`，不直接改变状态。
   2. `action`可包含任何**异步**操作。
   3. `action`接受一个与`store`实例具有相同方法、属性的`context`对象（因此可用`context.commit/state/getters`等），但非store实例本身。 参数：`{state,commit}`

2. 注册action：

   ```javascript
   const store = new vuex.Store({
       state:{count:0},
       mutation:{
           increment(state){state.count++}
       },
       actions:{
           increment(context){contex.commit('increment')}
           // 简写
           incrementAsync  ({ commit }) {
   			setTimeout(() => {
         			commit('increment')
       		}, 1000)
   		}
       }
   })
   ```

3. 赋值：

   1.  为何不直接分发mutaion？因为actoin支持异步，可在其内部执行异步操作。
   2. 方法1：直接赋值，`this.$store.dispatch('xxx')`，支持载荷方式和对象形式。
   3. 方法2：将组件的methods映射为`store.dispatch`调用：使用`mapActions`函数。
   
   ```javascript
   // 以载荷形式分发
   this.$store.dispatch('incrementAsync', {
     amount: 10
   })
   
   // 以对象形式分发
   this.$store.dispatch({
     type: 'incrementAsync',
     amount: 10
   })
   
   // 使用`mapActions`
   import { mapActions } from 'vuex'
   export default {
     methods: {
       ...mapActions([
         'increment', 
         'incrementBy' // 支持载荷
       ]),
       ...mapActions({
         add: 'increment' 
       })
     }
   }
   ```

4. 组合action（异步处理时的组合）

    - `store.dispatch` 可以处理被触发的 action 的处理函数返回的 Promise，并且 `store.dispatch` 仍旧返回 Promise：
- 一个 `store.dispatch` 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。
  
   ```javascript
   actions: {
     async actionA ({ commit }) {
       commit('gotData', await getData())
     },
     async actionB ({ dispatch, commit }) {
       await dispatch('actionA') // 等待 actionA 完成
       commit('gotOtherData', await getOtherData())
     }
   }
   ```


#### 1.2.2.8. Modules：模块化

1. 背景：使用单一状态树，所有状态会集中到一个比较大的对象上，从而store会变得臃肿。

2. `moudules`作用：将store分割为模块，每个模块有自己的state, mutaion, actions,getters,甚至是嵌套子模块。

3. 定义

   ```javascript
   // 每个模块，也可以放入独立文件中
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
   const store = new Vuex.Store({
     modules: {
       a: moduleA,
       b: moduleB
     }
   })
   store.state.a // -> moduleA 的状态
   store.state.b // -> moduleB 的状态
   ```

4. 模块内的状态管理

   1. mutation参数：(state)，指局部状态。
   2. `actions`参数：context，访问局部状态通过 `context.state` ，访问根节点状态则为 `context.rootState`。若要在全局命名空间内分发action，或提交mutation，将{ root: true }作为第3个参数传给dispatch或commit。
   3.  `getter`参数：`(state,getters,rootState,rootGetters)`后2个为全局的。

5. 命名空间

   1. 默认命名空间：**全局命名空间**。
   
   2. 设置为带命名空间的模块：`namespaced: true` ，提高封装度。
   
   3. 在带命名空间的模块内访问全局内容action：如上。
   
   4. 带命名空间的绑定函数
   
      当使用 `mapState`, `mapGetters`, `mapActions` 和 `mapMutations` 这些函数来绑定带命名空间的模块时，写起来可能比较繁琐。可以将**模块的空间名称字符串**作为第一个参数传递给上述函数，这样所有绑定都会自动将该模块作为上下文。
   
      ```javascript
      computed: {
        ...mapState({
          a: state => state.some.nested.module.a
        })
        // 简写   
        ...mapState('some/nested/module', {
          a: state => state.a,
        })
          
      },
      methods: {
        ...mapActions([
          'some/nested/module/foo', 
        ])
        // 简写
        ...mapActions('some/nested/module', [
            'foo'
        ])
      }
      ```
      
   
6. 动态注册模块：`store.registerMoudle`

   ```javascript
   // 创建/声明 模块
   import Vuex from 'vuex'
   const store = new Vuex.Store({})
   
   // 动态注册模块
   store.registerMoudle(['nested', 'myModule'], {
     // ...
   })
   ```

7. 卸载动态模块：`store.unregisterModule()`，注意无法卸载静态模块（store声明时的模块）。

8. 检查模块是否已注册到store：`store.hasModule(moduleName)`。

9. 在注册新模块时，希望保留指定的历史模块：`store.registerModule('a', module, { preserveState: true })`。

### 1.2.3. 路由

[官方链接](https://router.vuejs.org/zh/guide/advanced/meta.html)

#### 单页面应用

1. 传统开发模式：每个url对应一个html文件，这样url变化时，需要重新加载html文件，触发浏览器刷新，影响用户体验。
2. SPA单页面应用：不同url对应同一个html文件。更新视图而不会重新请求页面，可实现页面的**局部无刷新替换**。
3. SPA优缺点：
   1. 优点：Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件，核心是一个响应的数据绑定系统。MVVM、数据驱动、组件化、轻量、简洁、高效、快速、模块友好。
   2.  缺点：不支持低版本的浏览器，最低只支持到IE9；不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）；第一次加载首页耗时相对长一些；不可以使用浏览器的导航按钮需要自行实现前进、后退。

#### MVVM模式

1. mvvm即Model-View-ViewModel，mvvm的设计原理是基于mvc的。
2. Model代表数据模型负责业务逻辑和数据封装。
3. View代表UI组件负责界面和显示。
4. ViewModel监听模型数据的改变和控制视图行为，处理用户交互，简单来说就是通过双向数据绑定把View层和Model层连接起来。
5. 在MVVM架构下，View和Model没有直接联系，而是通过ViewModel进行交互，我们只关注业务逻辑，不需要手动操作DOM，不需要关注View和Model的同步工作。M和VM之间，通过ajax方式。
6. 背景：前后端不分离时期，页面请求：后端直接把数据通过模版引擎拼接进了返回的HTML中。
7. MVVM优势：前后端分离，数据的请求和提交：通过ajax的方式，但vue.js本身没有封装ajax操作库，所以我们要通过Vue-resource和Axios来进行ajax操作，而因为种种原因，现在vue.js2.0已经将axios作为官方推荐的ajax库了。

#### 1.2.3.1. 路由管理是什么

3. 定义：路径管理系统，建立起url和页面之间的映射关系。
4. 路由的作用：
   1. 监听URL的变化，在变化前后执行相应的逻辑。
   2. 不同的URL对应不同的组件。
   3. 提供多种改变URL的API（保证URL的改变不刷新浏览器）

#### 1.2.3.2. 路由使用方式

1. 安装插件：`npm i vue-router -S`

2. 提供一个“路由配置表”，不同URL对应不同组件的配置

   ```javascript
   // routes.js
   // 引入组件
   import RouterDemo from './components/RouterDemo'
   // 引入组件（懒加载方式）
   const RouterDemo = resolve => require.ensure(
       [],
       ()=>resolve( require('./components/RouterDemo')),
   )
   
   // 路由信息
   const routes = [
       {
   	 path:'/user/:id', //网页显示的路径，“:id”指动态id
        component: RouterDemo,// 页面要展示的组件
        name:'3', // 给path取别名，非必须，默认default
        props:true,// 若path中含有动态属性，需要
        meta:{ // 非必须
            titleCn:'中文名',
            titleEn:'engName'
        },
        children:[] // 子节点，非必须
       },
       
      // 若匹配到a，则重定向
       {path:'/a',redirect:'/bar'} 
       
     //若以上路由信息都未匹配到，则进入RouterDemo，命名为404
       { path: '*', component: RouterDemo, name: '404' }
   ]
   ```

3. 初始化“路由实例”

   ```javascript
   import VueRouter from 'vue-router'
   import routes from './routes'
   const router = new VueRouter({
     mode:'hash',//或history
     routes // 自定义的router模块
   })
   ```

4. 挂在到`Vue`实例上

   ```javascript
   new Vue({
       router, // 挂在到vue实例上
   }).$mount('#app')
   
   ```

5. 提供一个“路由占位”，用来挂在URL匹配到的组件

#### 1.2.3.3. 常用模式及原理

1. 路由类型：Hash模式、History模式。

2. Hash模式：

   1. 样式：localhost:8080/#/pro，会在域名后带一个‘#’
   2. 特点：丑（带#号），无法使用锚点定位；
   3. 原理： **使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。** hash（#）是URL 的锚点，代表的是网页中的一个位置，单单改变#后的部分，浏览器只会滚动到相应位置，不会重新加载网页，也就是说**hash 出现在 URL 中，但不会被包含在 http 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面**；同时每一次改变#后的部分，都会在浏览器的访问历史中增加一个记录，使用”后退”按钮，就可以回到上一个位置；所以说**Hash模式通过锚点值的改变，根据不同的值，渲染指定DOM位置的不同数据。hash 模式的原理是 onhashchange 事件(监测hash值变化)，可以在 window 对象上监听这个事件**

3. History模式：:nfis

   1. 样式：localhost:8080/pro
   2. 特点：需要后端配合，IE9不兼容（可使用强制刷新处理）
   3. 原理：由于hash模式会在url中自带#，如果不想要很丑的 hash，我们可以用路由的 history 模式，只需要在配置路由规则时，加入"mode: 'history'",**这种模式充分利用了html5 history interface 中新增的 pushState() 和 replaceState() 方法。这两个方法应用于浏览器记录栈，在当前已有的 back、forward、go 基础之上，它们提供了对历史记录修改的功能。只是当它们执行修改时，虽然改变了当前的 URL ，但浏览器不会立即向后端发送请求**。

4. 

5. 路由的底层原理

   ![](./img/vue开发实战/router底层原理.png)

### 1.2.4. Nuxt

#### 1.2.4.1. Nuxt作用

1. SPA缺点：
   1. 不利于SEO（index.html空壳，爬虫拿不到实际的内容）》服务端渲染SSR。
   2. 首屏渲染时间长》预渲染Prerendering。
2. 预渲染Prerendering：适用于静态页面。
3. 服务端渲染SSR：适合动态渲染；但构建繁琐。
4. Nuxt作用：静态站点、动态渲染、简化配置。

### 1.2.5. UI库对比

1. 对比

   ![](./img/vue开发实战/组件对比.png)

### 1.2.6. 提升开发效率和体验的常用工具

1. Vetur：语法高亮、标签补全、模板生成、Lint检查、格式化。
2. Eslint：代码规范检查、错误检查。
3. Prettier：代码格式化。
4. vue-devtools：vue官方调试工具，继承vuex、可远程调试、性能分析。

### 1.2.7. 单元测试

作用：保障研发质量、提高项目稳定性、提高开发速度。

[vue项目单元测试](https://zhuanlan.zhihu.com/p/48758013)

#### 1.2.7.1. 使用方式

1. 几种选择：
   1. jest或mocha：官方推荐的，2种运行测试的环境。
   2. @vue/test-utils：官方。
   3. sinon：非必备。
2. vue cli创建》手动》勾选unit testing》

## 1.3. 实战 Ant Design Pro

### 1.3.1. 安装 vue

1. CDN 方式：`<script src="http://...">`
2. NPM 方式

### 1.3.2. vue cli 介绍

1. 目标：将 Vue 生态中的工具基础标准化，确保了各种构建工具能够基于智能的默认配置即可平稳衔接。
2. vue cli 功能：

   - 通过 @vue/cli 实现的交互式的项目脚手架。
   - 通过 @vue/cli + @vue/cli-service-global 实现的零配置原型开发。
   - 运行时依赖 (@vue/cli-service)，该依赖可进行系列配置。
   - 丰富的官方插件集合，集成了前端生态中最好的工具。
   - 一套完全图形化的创建和管理 Vue.js 项目的用户界面。

3. vue cli 组成

   - CLI（@vue/cli）：提供 vue 命令（vue create,vue serve)，npm 包，全局安装的。
   - CLI 服务(@vue/cli-service)：开发环境依赖，npm 包，局部安装在每个 vue/cli 项目中。
   - CLI 插件（@vue/cli-plugin-）：向 vue 项目中提供可选择的 npm 包，如 babel/TypeScript 转译、ESLint 集成、单元测试和 end-to-end 测试等。

4. 常用命令

   - 安装 cli：`npm install -g @vue/cli`
   - 安装全局扩展：`npm install -g @vue/cli-service-global`
   - 检测版本：`vue --version`
   - 升级（全局）：`npm update -g @vue/cli`
   - 升级（插件）：`vue upgrade [options] [plugin-name]`

### 1.3.3. 创建项目

#### 1.3.3.1. 快速创建原型

1. 使用 vue serve
   - 安装 vue cli 及全局扩展
   - 准备入口文件：main.js、index.js、App.vue 或 app.vue
   - 在当前目录下执行 `vue serve`
2. 弊端：需要安装全局依赖，这使得它在不同机器上的一致性不能得到保证。

#### 1.3.3.2. 创建项目

1. 创建项目：`vue create prj_name`
2. 使用图形化界面：`vue ui`
3. 使用旧版 vue cli：vue cli>=3 和 vue cli2 命令相同，会发生覆盖，若用旧版的 init 功能，需要安装桥接工具`npm install -g @vue/cli-init`

### 1.3.4. 项目结构

1. 常规项目结构

   ![](./img/vue开发实战/项目结构.png)

2. vuex相关规则

   1. 应用层级的状态应该集中到单个 store 对象中。
   2. 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的。
   3. 异步逻辑都应该封装到 **action** 里面。
   4. 如果你的 store 文件太大，只需将 action、mutation 和 getter 分割到单独的文件。

### 1.3.5. 安装插件

1. 在现有的项目中安装插件：`vue add app_name`
2. 只在项目里访问插件 API 而不需要创建完整插件：在 package.json 中配置

   ```json
   {
     "vuePlugins": {
       "service": ["my-commands.js"]
     }
   }
   ```

### 引入第三方插件

1. npm install安装：之后在`webpack.base.conf.js配制`或直接挂载到vue实例上。
2. 全局引入：`index.html`中通过<script>引入，第三方js必须放在static目录下。
3. 局部引入：`import 文件名`。将js放在static/js目录下，且js文件中的对象/方法已通过`export,export default`暴露出来。
4. 参考资料：
   1. 按需引入：https://learnku.com/articles/45401
   2. 引入第3方库方法汇总：https://blog.csdn.net/yiyueqinghui/article/details/84391749

### 1.3.6. webpack和babel

[vue cli配置参考](https://cli.vuejs.org/zh/config/#%E5%85%A8%E5%B1%80-cli-%E9%85%8D%E7%BD%AE)

1. 自定义配置的文件：`vue.config.js`
2. webpack配置
3. babel配置

### 1.3.7. 高效地构建打包发布



### 1.3.8. 设计一个高扩展路由



### 1.3.9. 将菜单和路由结合



### 1.3.10. 用路由管理用户权限



### 1.3.11. 实现一个可动态改变的页面布局



### 1.3.12. 如何与服务端交互




## 1.4. Vue3.0





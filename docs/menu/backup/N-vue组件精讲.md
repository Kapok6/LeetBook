### 组件分类

1. 页面：
2. 基础组件：
   1. 特点：不包含业务，功能独立。如日期选择器。
   2. 要求：设计、兼容性、性能、复杂功能。
3. 业务组件：介于1、2之间，如vuex、axios、echarts。

### 组件组成

1. 组件的组成：props、event、slot。

2. 属性 prop：

   1. 作用：定义可配置的属性、决定了组件的核心功能。

   2. 建议：props 最好用**对象**的写法，这样可以针对每个属性设置类型、默认值或自定义校验属性的值。（`type/default/validator`）

   3. 注意：props定义的值，单向数据流，只能在父组件中修改。

   4. 父组件传入“标准html属性”，子组件会继承。若要关闭该功能，在组件中配置`inheritAttrs: false`。

      ```vue
      <!--父组件调用，传入id，class，子组件会继承，无需在props中定义-->
      <i-button id="btn1" class="btn-submit"></i-button>
      ```

3. 插槽 slot：

   1. 作用：分发组件的内容。（比如给按钮添加一些文字内容，就需要在组件按钮上定义一个插槽。）

   2. 效果：当组件渲染的时候，`<slot></slot>` 将会被替换为用户自定义的任何模板代码（包含html），若组件中未包含`<slot>`，则组件起止标签间的内容会被抛弃。

   3. 示例：

      ```vue
      <!--组件-->
      <template>
        <button :class="'i-button-size' + size" :disabled="disabled">
          <slot name="icon"></slot>
          <slot></slot>
        </button>
      </template>
      
      <!--使用-->
      <i-button>
        <i-icon slot="icon" type="checkmark"></i-icon>
        按钮 1
      </i-button>
      ```

   4. 作用域：父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

   5. 后备内容/默认值：它放在 `<slot>` 标签内，当未传入值时作为默认展示。

   6. 具名插槽：

      1. 使用场景：当需要在组件中定义多个插槽，标识名字以作区分。

      2. 使用方法：定

         ```vue
         <!--定义：添加 name 属性-->
         <slot name='header'></slot>
         
         <!--使用：在template上通过 v-slot='cname'注入，template之间的内容都会插入-->
         <template v-slot:footer>
           <p>Here's some contact info</p>
         </template>
         ```

   7. 作用域插槽：

      1. 使用场景：让插槽中的内容能够访问子组件数据。

      2. 使用方法：定义时，将要访问的属性，绑定在`slot`上。使用时，在父级作用域中，用带值的 `v-slot` 定义插槽 `prop` 的名字，然后通过`propName.attriName`访问。

         ```vue
         <!--定义：绑定user属性-->
           <slot v-bind:user="user">
             {{ user.lastName }}
           </slot>
         
         <!--使用：-->
         <current-user>
           <template v-slot:default="slotProps">
             {{ slotProps.user.firstName }}
           </template>
         </current-user>
         ```

      3. 工作原理：将插槽的内容包裹在一个拥有单个参数的函数里

         ```js
         function (slotProps) {
           // 插槽内容
         }
         ```

      4. 独占默认插槽的缩写语法：当被提供的内容*只有*默认插槽时，可将`v-slot`直接用在组件上。

         ```vue
         <!--独占默认插槽简写-->
         <current-user v-slot="slotProps">
           {{ slotProps.user.firstName }}
         </current-user>
         ```

      5. 注意：默认插槽的缩写语法**不能**和具名插槽混用，因为它会导致作用域不明确。（**只要出现多个插槽，请始终为*所有的*插槽使用完整的基于 ` 的语法**）

         ```vue
         <!-- 无效，会导致警告 -->
         <current-user v-slot="slotProps">
           {{ slotProps.user.firstName }}
           <template v-slot:other="otherSlotProps">
             slotProps is NOT available here
           </template>
         </current-user>
         
         <!-- 正确写法 -->
         <current-user>
           <template v-slot:default="slotProps">
             {{ slotProps.user.firstName }}
           </template>
           <template v-slot:other="otherSlotProps">
             ...
           </template>
         </current-user>
         
         <child>
             <template v-slot:default='myslot'>{{myslot.user}}</template>
         	<template v-slot:footer></template>
         </child>
         ```

      6. 解构插槽prop

         基于作用域插槽的工作原理，可以将`v-slot`的值，替换成可作为函数参数的JS表达式。

         ```vue
         <current-user v-slot="{ user }">
           {{ user.firstName }}
         </current-user>
         
         <!-- 重命名 -->
         <current-user v-slot="{ user: person }">
           {{ person.firstName }}
         </current-user>
         
         <!-- 设置默认值 -->
         <current-user v-slot="{ user = { firstName: 'Guest' } }">
           {{ user.firstName }}
         </current-user>
         ```

   8. 动态插槽：名字为动态

      ```vue
      <child>
          <template  v-slot:[dynamicSlotName]></template>
      </child>
      ```
   
4. event

   1. 在组件中自定义事件：通过`emit`传递给调用的父组件。

      ```vue
      <!-- 自定义事件 -->
      <template>
        <button @click="handleClick">
          <slot></slot>
        </button>
      </template>
      <script>
        export default {
          methods: {
            handleClick (event) {
              // 传递给父组件
              this.$emit('on-click', event);
            }
          }
        }
      </script>
      
      <!-- 父组件 -->
      <i-button @on-click="handleClick"></i-button>
      ```

### 组件通信

1. 组件间关系：父子、兄弟、隔代。

2. 父子间：

   1. 获取子组件引用信息： `ref`

   2. 访问父/子实例：$parent / $children`

   3. 子触发父的事件：`$emit(func)`触发， `v-on/@ func`监听。

   4. 监听自己实例上的事件：`$on()`

      ```vue
      <template>
        <div>
          <button @click="handleEmitEvent">触发自定义事件</button>
        </div>
      </template>
      <script>
        export default {
          methods: {
            handleEmitEvent () {
              // 在当前组件上触发自定义事件 test，并传值
              this.$emit('test', 'Hello Vue.js')
            }
          },
          mounted () {
            // 监听自定义事件 test
            this.$on('test', (text) => {
              window.alert(text);
            });
          }
        }
      </script>
      ```

      

3. 支持隔代：

   1. 父》孙 传递数据：`provide/inject` 
      1. `provide`将变量提供给所有子组件；`inject`注入从父组件获得的变量。
      2. 注意：非响应式，如果父改变变量值，子组件不会改变。
      3. 可以用来替换vuex，适合做一次性全局的状态数据管理
   2. 派发和广播：dispatch和broadcast
      1. v2.0+已弃用可自行实现。
      2. 常用于父子组件间通过自定义事件通信。

4. 兄弟之间：bus

5. 任意组件：vuex

6. 找到任意组件实例（自行实现）

   1. 由一个组件，向上找到最近的指定组件；

      ```js
      // assist.js
      // 由一个组件，向上找到最近的指定组件
      function findComponentUpward (context, componentName) {
        let parent = context.$parent;
        let name = parent.$options.name;
      
        while (parent && (!name || [componentName].indexOf(name) < 0)) {
          parent = parent.$parent;
          if (parent) name = parent.$options.name;
        }
        return parent;
      }
      export { findComponentUpward };
      ```

   2. 由一个组件，向上找到所有的指定组件；

   3. 由一个组件，向下找到最近的指定组件；

   4. 由一个组件，向下找到所有指定的组件；

   5. 由一个组件，找到指定组件的兄弟组件。

7. 使用`mixins`优化文件：

   将独立逻辑封装到不同js文件中，在主文件中通过`mixins`引入。

   ```js
   // mixin-test.js
   export default {
     data () {
       return {
         userInfo: null
       }
     },
     methods: {
       getUserInfo () {
         // 这里通过 ajax 获取用户信息后，赋值给 this.userInfo，以下为伪代码
         $.ajax('/user/info', (data) => { this.userInfo = data; });
       }
     },
     mounted () { this.getUserInfo(); }
   }
   ```

   ```vue
   <script>
     import mixins_user from '../mixins/mixin-test.js';
     export default {
       mixins: [mixins_user], // 引入逻辑
     }
   </script>
   ```

### vue构造器 extend/$mount

1. 目标：手动挂载、渲染vue组件：`extend、$mount`

2. 组件实例化：

   1. 实例化所包含内容：

      1. `el`：提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。（**只在用 `new` 创建实例时生效**）
      2. `template`：一个字符串模板作为 Vue 实例的标识使用。模板将会**替换**挂载的元素。挂载元素的内容都将被忽略，除非模板的内容有分发插槽。（如果 Vue 选项中包含**render**，该模板将**被忽略**。）
      3. `render`：字符串模板的代替方案，允许你发挥 JavaScript 最大的编程能力。该渲染函数接收一个 `createElement` 方法作为第一个参数用来创建 `VNode`。

   2. 实例化方法1：`Vue.extend`，基于 Vue 构造器，创建一个“子类”。

      ```js
      import Vue from 'vue';
      const AlertComponent = Vue.extend({
        template: '<div>{{ message }}</div>',
        data () {
          return {
            message: 'Hello, Aresn'
          };
        },
      });
      ```

   3. 实例化方法2：`new Vue`

      ```js
      import Vue from 'vue';
      import App from './app.vue';
      
      const AlertComponent = new Vue({
       // el: '#app', // 挂载
       render: h => h(App)
      })
      ```

   4. 注意：vue实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。（后面可以通过`$mount`手动挂载）

3. 手动挂载：`$mount` 
   1. 设置挂载点的方式1：在 $mount 里写参数来指定挂载的节点
   2. 设置挂载点的方式2：在文档之外渲染并且随后挂载
   3. 设置挂载点的方式3：创建实例时指定el

   ```js
   // 方法1：在 $mount 里写参数来指定挂载的节点：创建并挂载到 #app 
   const comp = new AlertComponent().$mount('#app');
   
   // 方法2：在文档之外渲染并且随后挂载
   const comp = new AlertComponent().$mount(); // comp实例为‘未挂载’状态
   document.body.appendChild(comp.$el);
   
   // 方法3：创建并挂载到 #app 
   new AlertComponent({ el: '#app' });
   ```

4. 获取`#app`实例：`const ins= comp.$children[0]`

5. 实战

   1. 动态渲染vue文件：display
   2. 全局提示组件：alert

### render函数

1. 作用：用来替换“模板”，可充分发挥JS编程能力。

   ```js
   // 模板写法
   <template>
     <div id="main" class="container" style="color: red">
       <p v-if="show">内容 1</p>
       <p v-else>内容 2</p>
     </div>
   </template>
   <script>
     export default {
       data () {
         return {
           show: false
         }
       }
     }
   </script>
   
   // Render写法
   export default {
     data () {
       return {
         show: false
       }
     },
     render: (h) => {
       let childNode;
       if (this.show) {
         childNode = h('p', '内容 1');
       } else {
         childNode = h('p', '内容 2');
       }
       return h('div', {
         attrs: {
           id: 'main'
         },
         class: {
           container: true
         },
         style: {
           color: 'red'
         }
       }, [childNode]);
     }
   }
   ```

2. 参数说明：`render(h)`，h即`createElement(tag/com,{属性},[childs])`，有3个参数

   1. 要渲染的元素或组件，可以是一个 html 标签、组件选项或一个函数（不常用），该参数为必填项。示例：

      ```js
      // 1. html 标签
      h('div');
      // 2. 组件选项
      import DatePicker from '../component/date-picker.vue';
      h(DatePicker);
      ```

   2. 对应属性的数据对象，比如组件的 props、元素的 class、绑定的事件、slot、自定义指令等，该参数是可选的，上文所说的 Render 配置项多，指的就是这个参数。该参数的完整配置和示例，可以到 Vue.js 的文档查看，没必要全部记住，用到时查阅就好：[createElement 参数](https://link.juejin.cn/?target=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fguide%2Frender-function.html%23createElement-%E5%8F%82%E6%95%B0)。

   3. 子节点，可选，String 或 Array，它同样是一个 h。示例：

      ```js
      [
        '内容', // child1
        h('p', '内容'),// child2
        h(Component, { // child3
          props: {
            someProp: 'foo'
          }
        })
      ]
      ```

### 递归、动态、异步组件

1. 递归组件
   
   1. 必要条件：定义name，使用时设置递归结束条件。
   
2. 动态组件
   1. 定义：根据一些条件，动态地切换某个组件，或动态地选择渲染某个组件
   
   2. 实现方式：Render函数，`<component :is=''> ` 的`is`特性，is可以绑定标签名、组件名、组件对象。
   
   3. 动态组件切换时，会重新加载，为提升性能，可使用keep-alive进行缓存。
   
   4. 举例
   
      ```vue
      <template>
        <component :is="tagName" v-bind="tagProps">
          <slot></slot>
        </component>
      <template>
      <script>
           computed: {
               // 动态渲染不同的标签
               tagName () {
                  return this.to === '' ? 'button' : 'a';
                },
              // 通过对象tagProps，绑定一系列属性
               tagProps () { 
              	let props = {};
                   props = {
                      target: this.target,
                      href: this.to
                    }
              	return props;
               }
            }
      </script>
      ```
   
3. 异步组件

   1. 使用场景：路由懒加载

   2. 示例：

      ```js
      component: () => import('./views/form.vue')
      ```

### vue不常用API




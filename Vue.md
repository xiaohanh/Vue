### 一.Vue.js是什么：
  
     Vue是一套用于构建用户界面的渐进式框架。
### 二.Vue实例：所有的Vue组件都是Vue实例，并且接受相同的选项对象。
    
```
   1.创建一个Vue实例
   var vm=new Vue({
       //选项
   })
```

```
   2.数据与方法：
   当一个Vue实例被创建时，它向Vue的响应式系统中加入了其data对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生响应，即匹配更新为新的值。
   
```
==注意：只有当实例被创建时data中存在的属性才是响应式的。==

==例外：Object.freeze()会阻止修改现有的属性，也意味着响应系统无法再追踪变化。==

==除了数据属性，Vue实例还暴露了一些有用的实例属性与方法。它们都有前缀 $，以便与用户定义的属性区分开来。==


```
 3.实例生命周期钩子：
   
```

2.v-bind 特性被称为指令。

3.条件与循环：
  v-if   v-for

4.处理用户输入：
  
    v-on 指令添加一个事件监听器。
    v-model:它能实现表单输入和应用状态之间的双向绑定。

5.组件化应用构建：

### 三.模板语法

```
  1.插值：
        1.1文本：“Mustache”语法{{ }} 数据改变，插值处的内容会更新。
        v-once指令： 数据改变，插值处的内容不更新。但是会影响到该节点上的其他数据绑定。
    
        <span>Message:{{msg}}</span>
       <span v-once>{{msg}}</span>
    
       1.2.原始HTML: v-html 指令
    
        <p>Using v-html directive: <span v-html="rawHtml"></span></p>
      
```
 ==不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。反之，对于用户界面 (UI)，组件更适合作为可重用和可组合的基本单位。==
 
==站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值==。


```
     1.3.特性：
     Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 v-bind 指令：
     <div v-bind:id="dynamicId"></div>
     
     在布尔特性的情况下，它们的存在即暗示为 true，v-bind 工作起来略有不同，在这个例子中：
     <button v-bind:disabled="isButtonDisabled">Button</button>
     如果 isButtonDisabled 的值是 null、undefined 或 false，则 disabled 特性甚至不会被包含在渲染出来的 <button> 元素中。
```

```
     1.4.JavaScript表达式：
     每个绑定都只能包含单个表达式。
```
==模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。==

```
  2.指令：
  指令 (Directives) 是带有 v- 前缀的特殊特性。指令特性的值预期是单个 JavaScript 表达式 (v-for 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。
```
  
```
   2.1参数：
      一些指令能够接收一个“参数”，在指令名称之后以冒号表示。
      <a v-bind:href="url">...</a>
      <a v-on:click="doSomething">...</a>
      
   2.2修饰符：
      修饰符 (Modifiers) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。
      .prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：

      <form v-on:submit.prevent="onSubmit">...</form>
```

```
  3.缩写：
    v-bind缩写:
    <!-- 完整语法 -->
       <a v-bind:href="url">...</a>

    <!-- 缩写 -->
       <a :href="url">...</a>
     
    v-on缩写：
    <!-- 完整语法 -->
      <a v-on:click="doSomething">...</a>

     <!-- 缩写 -->
     <a @click="doSomething">...</a>
```
### 四.计算属性和侦听器：
   

```
  1.计算属性缓存vs方法：
     计算属性是基于它们的依赖进行缓存的。只在相关依赖发生改变时它们才会重新求值。
```
### 五.列表渲染：

 1.对象更改检测注意事项：
    对于已经创建的实例，Vue不能动态添加根级别的响应式属性。但是可以使用Vue.set(object,key,value)方法向嵌套对象添加响应式属性。

### 六.组件：
    
    1.组件注册：
         全局注册、局部注册
         Vue.component("my-component-name",{/*...*/})
         
    

 **==局部注册的组件在其子组件中不可用==**  。
 
 ==在模块系统中局部注册：==
  
     导入想要使用的组件：
        例如：砸一个假设的ComponentB.js或者ComponentB.vue文件中：
       
```
        import ComponentA from "./ComponentA"
        import ComponentB from './ComponentC'
        export default {
            components: {
                ComponentA,
                ComponentC
            },
            //......
            
        }

```
==基础化组建的全局注册==

```
例如：在应用入口文件（src/main.js）中全局导入基础组件：
   import Vue from 'vue'
   import upperFirst from 'lodash/upperFirst'
   import camelCase frmo 'lodash/camelCase'
   
   const requireComponent = require.context(
  // 其组件目录的相对路径
  './components',
  // 是否查询其子目录
  false,
  // 匹配基础组件文件名的正则表达式
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // 获取组件配置
  const componentConfig = requireComponent(fileName)

  // 获取组件的 PascalCase 命名
  const componentName = upperFirst(
    camelCase(
      // 剥去文件名开头的 `./` 和结尾的扩展名
      fileName.replace(/^\.\/(.*)\.\w+$/, '$1')
    )
  )

  // 全局注册组件
  Vue.component(
    componentName,
    // 如果这个组件选项是通过 `export default` 导出的，
    // 那么就会优先使用 `.default`，
    // 否则回退到使用模块的根。
    componentConfig.default || componentConfig
  )
})
```
2.插槽：
   
```
  <slot></slot>作为承载分发内容的出口。
  如果<navigation-link>没有一个包含<slot>元素，则任何传入它的元素都会被抛弃
  具名插槽:
   <slot name="header"></slot>
   <slot name="footer"></slot>
    
 
## vue.js全局API

全局API并不在构造器里，而是先声明全局变量或者直接在Vue上定义一些新功能，Vue内置了一些全局API，比如我们今天要学习的指令Vue.directive。说的简单些就是，在构造器外部用Vue提供给我们的API函数来定义新的功能。

#### 1、Vue.directive 自定义指令

全局自定义指令
```javascript
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```
注册局部指令，组件中也接受一个 directives 的选项：
```javascript
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

**钩子函数**
自定义指令有五个生命周期（也叫钩子函数），分别是 bind,inserted,update,componentUpdated,unbind
- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。

- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

- `update`：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。

- `componentUpdated`：指令所在组件的 VNode 及其子 VNode 全部更新后调用。

- `unbind`：只调用一次，指令与元素解绑时调用。

指令钩子函数会被传入以下参数：

*   `el`：指令所绑定的元素，可以用来直接操作 DOM 。
*   `binding`：一个对象，包含以下属性：
    *   `name`：指令名，不包括 `v-` 前缀。
    *   `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
    *   `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
    *   `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
    *   `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
    *   `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
*   `vnode`：Vue 编译生成的虚拟节点。移步 [VNode API](../api/#VNode-接口) 来了解更多详情。
*   `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。

#### 2、Vue.extend构造器的延伸

Vue.extend 返回的是一个“扩展实例构造器”,也就是预设了部分选项的Vue实例构造器。经常服务于Vue.component用来生成组件，可以简单理解为当在模板中遇到该组件名称作为标签的自定义元素时，会自动调用“扩展实例构造器”来生产组件实例，并挂载到自定义元素上。

使用基础 Vue 构造器，创建一个 “子类”。参数是一个包含组件选项的对象。

`data` 选项是特例，需要注意 - 在 `Vue.extend()` 中它必须是函数
```javascript
// 创建构造器
var Profile = Vue.extend({
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
})
// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')
```
效果如下：
```html
<p>Walter White aka Heisenberg</p>
```

#### 3、Vue.set全局操作
Vue.set 的作用就是在构造器外部操作构造器内部的数据、属性或者方法。
Vue.set( target, key, value )
设置对象的属性。如果对象是响应式的，确保属性被创建后也是响应式的，同时触发视图更新。这个方法主要用于避开 Vue 不能检测属性被添加的限制。

#### 4、Vue的生命周期（钩子函数）
Vue一共有10个生命周期函数，我们可以利用这些函数在vue的每个阶段都进行操作数据或者改变内容。
!()[./lifecycle.png]

#### 5、Template 制作模版

直接写在选项里的模板
- 直接在构造器里的template选项后边编写。这种写法比较直观，但是如果模板html代码太多，不建议这么写。

写在`<template>`标签里的模板
- 这种写法更像是在写HTML代码，就算不会写Vue的人，也可以制作页面。

写在`<script>`标签里的模板
这种写模板的方法，可以让模板文件从外部引入。常用
```javascript
<script type="x-template" id="demo3">
    <h2 style="color:red">我是script标签模板</h2>
</script>

<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            message:'hello Vue!'
        },
        template:'#demo3'
    })
</script>
```

#### 6、component组件

**1-什么是组件？**

组件是 vue.js 最强大的功能之一。组件可以扩展 HTML 元素，封装可重用的代码
组件是自定义元素（对象）

**2-定义组件的方式**

方式1：先创建组件构造器，然后由组件构造器创建组件
方式2：直接创建组件

局部注册其实就是写在构造器里，但是你需要注意的是，构造器里的components 是加s的，而全局注册是不加s的。

**3-组件分类**

分类：全局组件、局部组件


**4-引用模板**

将组件内容放到模板`<template>`中并引用

**5-动态组件**

```
 <template :is="">组件
    多个组件使用同一个挂载点，然后动态的在它们之间切换
 <keep-alive>组件
```

#### 7、组件间数据传递
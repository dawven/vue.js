# 选项
### 1、propsData Option  全局扩展的数据传递
propsData 不是和属性有关，他用在全局扩展时进行传递数据。先回顾一下全局扩展的知识，作一个<header></header>的扩展标签出来。实际我们并比推荐用全局扩展的方式作自定义标签，我们学了组件，完全可以使用组件来做，这里只是为了演示propsData的用法。 
扩展标签已经做好了，这时我们要在挂载时传递一个数字过去，我们就用到了propsData。

我们用propsData三步解决传值：

1、在全局扩展里加入props进行接收。propsData:{a:1}

2、传递时用propsData进行传递。props:[‘a’]

3、用插值的形式写入模板。{{ a }}

代码如下：
```javascript
var  header_a = Vue.extend({
    template:`<p>{{message}}-{{a}}</p>`,
    data:function(){
        return {
            message:'Hello,I am Header'
        }
    },
    props:['a']
}); 
new header_a({propsData:{a:1}}).$mount('header');
```
总结：propsData在实际开发中我们使用的并不多，我们在后边会学到Vuex的应用，他的作用就是在单页应用中保持状态和数据的。

### 2、computed Option  计算选项
    computed 的作用主要是对原数据进行改造输出。改造输出：包括格式的编辑，大小写转换，顺序重排，添加符号……。
    computed 属性是非常有用，在输出数据前可以轻松的改变数据。

### 3、Methods Option  方法选项
    Methods
*   **类型**：`{ [key: string]: Function }`

*   **详细**：

    methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 `this` 自动绑定为 Vue 实例。

    注意，**不应该使用箭头函数来定义 method 函数** (例如 `plus: () => this.a++`)。理由是箭头函数绑定了父级作用域的上下文，所以 `this` 将不会按照期望指向 Vue 实例，`this.a` 将是 undefined。

*   **示例**：

    | 

    <pre>var vm = new Vue({
      data: { a: 1 },
      methods: {
        plus: function () {
          this.a++
        }
      }
    })
    vm.plus()
    vm.a // 2
    </pre>

     |
### 4、Watch 选项 监控数据
写在构造器里面直接这样,这里举的例子监控temperature的数据变化，suggestion取相对于数据
```javascript
watch:{
    temperature:function(newVal,oldVal){
        if(newVal>=26){
            this.suggestion=suggestion[0];
        }else if(newVal<26 && newVal >=0)
        {
            this.suggestion=suggestion[1];
        }else{
            this.suggestion=suggestion[2];
        }
    }
}
```
为了降低程序耦合度，是程序变得灵活。可以使用实例属性的形式来写watch监控，也就是把watch写到构造器的外部。
```javascript
app.$watch('xxx',function(){})
```
下面和上面一致
```JavaScript
app.$watch('temperature',function(newVal,oldVal){
    if(newVal>=26){
        this.suggestion=suggestion[0];
    }else if(newVal<26 && newVal >=0)
    {
        this.suggestion=suggestion[1];
    }else{
        this.suggestion=suggestion[2];
    }

})
```
### 5、Mixins 混入选项操作
Mixins一般有两种用途：

1、在你已经写好了构造器后，需要增加方法或者临时的活动时使用的方法，这时用混入会减少源代码的污染。

2、很多地方都会用到的公用方法，用混入的方法可以减少代码量，实现代码重用。

```javascript
//额外临时加入时，用于显示日志
var addLog={
    updated:function(){
        console.log("数据放生变化,变化成"+this.num+".");
    }
}
var app=new Vue({
    el:'#app',
    data:{
        num:1
    },
    methods:{
        add:function(){
            this.num++;
        }
    },
    mixins:[addLog]//混入
})
```

**mixins的调用顺序**
从执行的先后顺序来说，都是混入的先执行，然后构造器里的再执行，需要注意的是，这并不是方法的覆盖，而是被执行了两边。
ps：当混入方法和构造器的方法重名时，混入的方法无法展现，也就是不起作用。

我们也可以定义全局的混入，这样在需要这段代码的地方直接引入js，就可以拥有这个功能了。我们来看一下全局混入的方法：
```javascript
Vue.mixin({
    updated:function(){
        console.log('我是全局被混入的');
    }
})
```

### 6、Extends Option  扩展选项
    Extends

*   **类型**：`Object | Function`

*   **详细**：

    允许声明扩展另一个组件 (可以是一个简单的选项对象或构造函数)，而无需使用 `Vue.extend`。这主要是为了便于扩展单文件组件。

    这和 `mixins` 类似，区别在于，组件自身的选项会比要扩展的源组件具有更高的优先级。

*   **示例**：

    | 

    <pre>var CompA = { ... }

    // 在没有调用 `Vue.extend` 时候继承 CompA
    var CompB = {
      extends: CompA,
      ...
    }
    </pre>

     |
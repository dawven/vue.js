## vue.js常用指令

### 1、v-if v-else v-show
v-if:是vue 的一个内部指令，指令用在我们的html中。

v-if用来判断是否加载html的DOM，比如我们模拟一个用户登录状态，在用户登录后现实用户名称。

v-show:调整css中display属性，DOM已经加载，只是CSS控制没有显示出来。

**v-if 和v-show的区别**
- v-if: 判断是否加载，可以减轻服务器的压力，在需要时加载。
- v-show 调整css dispaly属性，可以使客户端操作更加流畅。

### 2、v-for 解决模板循环问题
v-for指令是循环渲染一组data中的数组，v-for 指令需要以 item in items 形式的特殊语法，items 是源数据数组并且item是数组元素迭代的别名。

模板写法
```html
<li v-for="item in items">
        {{item}}
</li>
```
js写法
```javascript
var app=new Vue({
     el:'#app',
     data:{
         items:[20,23,18,65,32,19,54,56,41]
     }
})
```
v-for常常伴随着数组排序输出，自带的sort函数不能解决问题,故
```javascript
  function sortNumber(a,b){
            return a-b
  }
```
用法
```javascript
 computed:{
    sortItems:function(){
    return this.items.sort(sortNumber);
    }
 }
```
### 3、v-text&v-html
我们已经会在html中输出data中的值了，我们已经用的是{{xxx}},这种情况是有弊端的，就是当我们网速很慢或者javascript出错时，会暴露我们的{{xxx}}。Vue给我们提供的v-text,就是解决这个问题的。我们来看代码：
```javascript
 <span>{{ message }}</span>=<span v-text="message"></span><br/>
```
如果在javascript中写有html标签，用v-text是输出不出来的，这时候我们就需要用v-html标签了。
```javascript
<span v-html="msgHtml"></span>
```
双大括号会将数据解释为纯文本，而非HTML。为了输出真正的HTML，你就需要使用v-html 指令。

需要注意的是：在生产环境中动态渲染HTML是非常危险的，因为容易导致XSS攻击。所以只能在可信的内容上使用v-html，永远不要在用户提交和可操作的网页上使用。
### 4、v-on：绑定事件监听器
页面核心代码
```html
<div id="app">
    本场比赛得分： {{count}}<br/>
    <button v-on:click="jiafen">加分</button>
    <button v-on:click="jianfen">减分</button>

</div>
```
js代码核心
```javascript
var app=new Vue({
            el:'#app',
            data:{
                count:1
            },
            methods:{
                jiafen:function(){
                    this.count++;
                },
                jianfen:function(){
                    this.count--;
                }
            }
        })
```
我们的v-on 还有一种简单的写法，就是用@代替。
```html
<button @click="jianfen">减分</button>
```
### 5、v-model指令
双向数据绑定,与数据源绑定。  
html
```html
<div id="app">
    <p>原始文本信息：{{message}}</p>
    <h3>文本框</h3>
    <p>v-model:<input type="text" v-model="message"></p>
</div>
javascript
```
```javascript
var app=new Vue({
  el:'#app',
  data:{
       message:'hello Vue!'
  }
 })
```
### 6、v-bind
v-bind是处理HTML中的标签属性的，例如'<div></div>'就是一个标签，'<img>'也是一个标签，我们绑定'<img>上'的'src'进行动态赋值。

v-bind 缩写
```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

### 7、其他内部指令
**v-pre指令**
在模板中跳过vue的编译，直接输出原始值。就是在标签中加入v-pre就不会输出vue中的data值了。
```
<div v-pre>{{message}}</div>
```
这时并不会输出我们的message值，而是直接在网页中显示{{message}}

**v-cloak指令**
在vue渲染完指定的整个DOM后才进行显示。它必须和CSS样式一起使用，
```
[v-cloak] {
  display: none;
}
```

```html
<div v-cloak>
  {{ message }}
</div>
```

**v-once指令**
在第一次DOM时进行渲染，渲染完成后视为静态内容，跳出以后的渲染过程。
```html
<div v-once>第一次绑定的值：{{message}}</div>
<div><input type="text" v-model="message"></div>
```

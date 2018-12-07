# [vue内置指令与自定义指令](https://www.cnblogs.com/ilovexiaoming/p/6840383.html)

一、内置指令

1、v-bind：响应并更新DOM特性；例如：v-bind:href  v-bind:class  v-bind:title  v-bind:bb

2、v-on：用于监听DOM事件； 例如：v-on:click  v-on:keyup

3、v-model：数据双向绑定；用于表单输入等；例如：<input v-model="message">

4、v-show：条件渲染指令，为DOM设置css的style属性

5、v-if：条件渲染指令，动态在DOM内添加或删除DOM元素

6、v-else：条件渲染指令，必须跟v-if成对使用

7、v-for：循环指令；例如：<li v-for="(item,index) in todos"></li>

8、v-else-if：判断多层条件，必须跟v-if成对使用；

9、v-text：更新元素的textContent；例如：<span v-text="msg"></span> 等同于 <span>{{msg}}</span>；

10、v-html：更新元素的innerHTML；

11、v-pre：不需要表达式，跳过这个元素以及子元素的编译过程，以此来加快整个项目的编译速度；例如：<span v-pre>{{ this will not be compiled }}</span>；

12、v-cloak：不需要表达式，这个指令保持在元素上直到关联实例结束编译；

13、v-once：不需要表达式，只渲染元素或组件一次，随后的渲染，组件/元素以及下面的子元素都当成静态页面不在渲染。

二、自定义指令

1、自定义指令：使用Vue.directive(id,definition)注册全局自定义指令，使用组件的directives选项注册局部自定义指令。

2、钩子函数：

指令定义函数提供了几个钩子函数（可选）：

bind：只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。

inserted：被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）。

update：第一次是紧跟在 bind 之后调用，获得的参数是绑定的初始值，之后被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新（详细的钩子函数参数见下）。

componentUpdated：被绑定元素所在模板完成一次更新周期时调用。

unbind：只调用一次， 指令与元素解绑时调用。

3、钩子函数的参数：(el, binding, vnode, oldVnode)

　　el：指令所绑定的元素，可以用来直接操作 DOM 。

　　binding：一个对象，包含以下属性

　　　　name：指令名，不包含v-的前缀；

　　　　value：指令的绑定值；例如：v-my-directive="1+1"，value的值是2；

　　　　oldValue：指令绑定的前一个值，仅在update和componentUpdated钩子函数中可用，无论值是否改变都可用；

　　　　expression：绑定值的字符串形式；例如：v-my-directive="1+1"，expression的值是'1+1'；

　　　　arg：传给指令的参数；例如：v-my-directive:foo，arg的值为 'foo'；

　　　　modifiers：一个包含修饰符的对象；例如：v-my-directive.a.b，modifiers的值为{'a':true,'b':true}

　　vnode：Vue编译的生成虚拟节点；

　　oldVnode：上一次的虚拟节点，仅在update和componentUpdated钩子函数中可用。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<div id="app" v-demo:foo.a.b="message"></div>

Vue.directive('demo', {
  bind: function (el, binding, vnode) {
      console.log('bind');
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
});
new Vue({
  el: '#app',
  data: {
    message: 'hello!'
  }
});

```

// name: "demo"
// value: "hello!"
// expression: "message"
// argument: "foo"
// modifiers: {"a":true,"b":true}
// vnode keys: tag, data, children, text, elm, ns, context, functionalContext, key, componentOptions, componentInstance, parent, raw, isStatic, isRootInsert, isComment, isCloned, isOnce

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

4、函数简写：大多数情况下，我们可能想在 bind 和 update 钩子上做重复动作，并且不想关心其它的钩子函数。可以这样写:

```
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

5、对象字面量：如果指令需要多个值，可以传入一个 JavaScript 对象字面量。记住，指令函数能够接受所有合法类型的 Javascript 表达式。

```
<div v-demo="{ color: 'white', text: 'hello!' }"></div>

Vue.directive('demo', function (el, binding) {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text)  // => "hello!"
})
```

 

例子解析：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<div id="app">
    <my-comp v-if="msg" :msg="msg"></my-comp>
    <button @click="update">更新</button>
    <button @click="uninstall">卸载</button>
    <button @click="install">安装</button>
</div>
<script type="text/javascript">
    Vue.directive('hello', {
        bind: function (el){
            console.log('bind');
        },
        inserted: function (el){
            console.log('inserted');
        },
        update: function (el){
            console.log('update');
        },
        componentUpdated: function (el){
            console.log('componentUpdated');
        },
        unbind: function (el){
            console.log('unbind');
        }
    });

    var myComp = {
        template: '<h1 v-hello>{{msg}}</h1>',
        props: {
            msg: String
        }
    }

    new Vue({
        el: '#app',
        data: {
            msg: 'Hello'
        },
        components: {
            myComp: myComp
        },
        methods: {
            update: function (){
                this.msg = 'Hi';
            },
            uninstall: function (){
                this.msg = '';
            },
            install: function (){
                this.msg = 'Hello';
            }
        }
    })
</script>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

a、页面加载时：bind inserted

b、更新组件：update componentUpdated

c、卸载组件：unbind

d、重新安装组件：bind inserted

注意区别：bind与inserted：bind时父节点为null，inserted时父节点存在；update与componentUpdated：update是数据更新前，componentUpdated是数据更新后。

原文链接:https://www.cnblogs.com/ilovexiaoming/p/6840383.html
### 要注意的细节

- this.$nextTick是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM

```
// 如监听某个数据变化，在数据变化后并且DOM也更新完，才执行回调里面的内容
watch() {
  // 监听播放状态
  playing(val) {
        const audio = this.$refs.audio
        this.$nextTick(() => {
          val ? audio.play() : audio.pause()
        })
      },
}

```

- props 传递数组时设置default

```
props: {
      switches: {
        type: Array,
        default: () => []
      }
    },

```

- 如果组件里用使用计时器

```
// 在组件销毁时(即切换组件或关闭页面)，
// 调用destroyed方法清除计时器
destroyed(){
  clearTimeout(this.timer)
}

```

- app.vue里给router-view外加keep-alive标签

```
// 好处 DOM会缓存在内容中
// 切换导航时不会重复获取数据
<keep-alive>
  <router-view></router-view>
</keep-alive>

```

- 在 Vue 2.0 中，为自定义组件绑定原生事件必须使用 .native 修饰符：

```
<my-component @click.native="handleClick">Click Me</my-component>

```

- filters 数据处理器，作用:在不改变的data 的情况下  输出前段需要的格式数据。

```
{{ message | filterFun }}

new Vue({  
  // ...  
  filters: {  
    //  进行格式转换并返回
    filterFun: function (value) {  
      return value;
    }  

  }  
})  
//filter中可有传多个参数，默认第一个为message的值，自定义参数从第二个开始
filters: {  
    filterFun: function (value, sta1, sta2) {  
      return value;
    }  
  }  

```

- .editorconfig 文件放在项目文件根目录里(是代码格式文件)
- 事件绑定 :
  在 Vue 2.0 中，为自定义组件绑定原生事件必须使用 .native 修饰符：
  `Click Me`
- vue本地资源放到项目的 `/static`目录下 ,才能引用(如图片、js、css)
- 引入组件 注意前面+`./` 否则打包后可能读取不到

```
import navTitle from './title.vue'

components:{
  navTitle
}
调用
<nav-title></nav-title>

```

- 在vuejs2.0中，任何试图在组件内修改通过props传入的父组件数据都被认为是anti-pattern的，报以下错误：
  `Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders`

- ​

  ​

  计算属性 是属性 可以直接放入html里

  ​

  ![img](//upload-images.jianshu.io/upload_images/9696783-a4bc1046786046e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/376/format/webp)

  ​

- 组件可以在data里定义常量 并用表达式赋值(不可以用函数赋值)
  如:获取窗口宽度:

```
win_W: window.innerWidth || document.body.clientWidth

根组件绑定窗口改变事件
window.onresize = function(){
    _this_.win_W = window.innerWidth || document.body.clientWidth;
}
窗口改变时，其他子组件也能得到

```

- 子组件利用事件传值，只能传给上一级组件，如果需要传给根组件，需要继续定义、触发事件

```
3级组件 ;  @click="trgSidebar"      
2级组件:   @trgSidebar="trgSidebar"
methods: {
    trgSidebar: () {
        this.$emit('trgSidebar');
    }
}

```

- Vue 绑定class style 之类 需要+ {}

```
<li :class="{ active: index ===0 }"></li>
```



原文链接: https://www.jianshu.com/p/64a4fa04b74d
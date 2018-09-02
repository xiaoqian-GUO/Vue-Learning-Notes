> 过渡
##### 自定义过渡的类名
1. 可以通过一下特性来自定义过渡类名，它们的优先级高于普通的类名：
```
enter-class
enter-active-class
enter-to-class
leave-class
leave-active-class
leave-to-class

可以通过指定上述特性的值，来制定特殊的类，并且结合第三方动画库使用起来十分有用

<div id="example-3">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">hello</p>
  </transition>
```
2. 同时使用过渡transition和动画animation
```
可以通过duration属性来定制一个显性的过渡持续时间

<transition :duration="3000"></transition>

也可以在duration属性中，指定进入和移出的持续时间

<transition :duration="{enter:500,leave:800}"></transition>
```

##### JavaScript钩子
1. 可以在属性中声明javascript钩子
```
// 钩子函数
before-enter
enter
after-enter
enter-cancelled

before-leave
leave
after-leave
leave-cancelled

//例子
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>

// 调用
methods: {
  // --------
  // 进入中
  // --------

  beforeEnter: function (el) {
    // ...
  },
  // 此回调函数是可选项的设置
  // 与 CSS 结合时使用
  enter: function (el, done) {
    // ...
    done()
  },
  afterEnter: function (el) {
    // ...
  },
  enterCancelled: function (el) {
    // ...
  },

  // --------
  // 离开时
  // --------

  beforeLeave: function (el) {
    // ...
  },
  // 此回调函数是可选项的设置
  // 与 CSS 结合时使用
  leave: function (el, done) {
    // ...
    done()
  },
  afterLeave: function (el) {
    // ...
  },
  // leaveCancelled 只用于 v-show 中
  leaveCancelled: function (el) {
    // ...
  }
}

注意：
1. 当只用javascript过渡的时候，在enter和leave中必须使用done进行回调，否则，它们将被同步调用，过渡会立即完成
2. 对于仅使用javascript过渡的元素添加v-bind:css="false" Vue会跳过CSS的检测，这也可以避免过渡过程中CSS的影响
3. 
```
##### 初始渲染的过渡
1. 可以通过appear特性来设置节点在初始渲染的过渡
```
<transition 
appear
appear-class="xxx"
appear-to-class="xxx"
appear-active-class="xxx"
>
......
</transition>

//自定义javascript钩子
<transition
appear
@before-appear="xxx"
@appear="xxx"
@after-appear="xxx"
@appear-cancelled="xxx"
>
</transition>
```
##### 多个元素的过渡
1. 对于不同标签的元素切换的时候，原生标签可以使用v-if/v-else来显式切换
2. 对于相同标签名的元素切换，需要通过key特性设置唯一的值来标记让Vue区分它们
```
<transition>
<button v-bind:key="docState">
{{buttonMessage}}
</button>
</transition>

computed:{
    //计算属性
    buttonMessage:function(){
        switch(this.docState){
            case 'saved': return 'Edit'
            case 'edited':return 'Save'
            case 'editing': return 'cancel'
        }
    }
}
```
##### 过渡模式
```
mode="in-out" : 新元素先进行过渡，完成之后当前元素过渡离开
mode="out-in" : 当前元素先进行过渡，完成之后新元素过渡进入
in-out模式不会经常用到，但对于一些稍微不同的过渡效果还是有用的

<transition name="fade" mode="out-in">
<!-- ... the buttons ... -->
</transition>
```
##### 多个组件的过渡
1. 不需要使用key特性，只需要使用动态组件
```
<transition name="component-fade" mode="out-in">
<component v-bind:is="view"></component>
</transition>

new Vue({
    el:"#transition-components-demo",
    data:{
        view:'v-a',
    },
    components:{
        'v-a':{
            template:"<div>component a</div>",
        },
        'v-b':{
            template:"<div>component b</div>",
        }
    }
})
```
##### 列表过渡 transition-group
1. 需要key属性提供唯一的属性值
2. 过渡模式不可用，因为不再相互切换特有的元素
```
 <transition-group name="flip-list" tag="ul">
    <li v-for="item in items" v-bind:key="item">
      {{ item }}
    </li>
 </transition-group>
 
 <transition-group name="list-complete" tag="p">
    <span
      v-for="item in items"
      v-bind:key="item"
      class="list-complete-item"
    >
      {{ item }}
    </span>
  </transition-group> 

```
##### 列表的交错过渡
1. 通过data，javascript以及数组的常用方法等实现列表的过渡
```
 <transition-group
    name="staggered-fade"
    tag="ul"
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <li
      v-for="(item, index) in computedList"
      v-bind:key="item.msg"
      v-bind:data-index="index"
    >{{ item.msg }}</li>
  </transition-group>
```
#### 状态过渡
1. 结合vue的响应式和组件系统，使用第三方库来实现切换元素的过渡状态
```
比如：
数字和运算
颜色的显示
SVG节点的位置
元素的大小和其他属性
```
##### 状态动画与侦听器
#### 可复用性 & 组合
##### 混入(mixins): 一种分发Vue组件中可复用功能的非常灵活的方式
1. 混入对象可以包含任意组件选项，当组件使用混入对象时，所有混入对象的选项将被混入该组件本身的选项
```
//定义一个混入对象
var myMixin={
    created:function(){
        this.hello();
    },
    methods:{
        hello: function(){
            console.log('hello from maxin!');
        },
    },
};

var component=Vue.extend({
    maxins:[myMixin],
});

var component=new Component();    // "hello from mixin!"
```

2. 选项合并：当组件和混入对象含有同名选项时，这些选项将以恰当的方式混合：      例如数据对象在内部会进行浅合并(一层属性深度)，在和组件的数据发生冲突的时候，以组件数据优先；<font color="red">和对象属性后者覆盖前者类似；</font>
3. <font color="blue">注意：混入对象的钩子将在组件自身钩子之前调用</font>
```
//混入对象:定义选项以及钩子函数等
var myMixin={
    data: function(){
        return {
            messsage:'goodbye',
            foo:'abc',
        };
    },
};

new Vue({
    mixins:[myMixin],
    data:{
        message:'goodbye',
        bar:'def',
    },
    created:function(){
        console.log(this.$data);
    }
});
```
4. 混入对象和组件的钩子函数的执行顺序
```
var mixin = {
  created: function () {
    console.log('混入对象的钩子被调用')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('组件钩子被调用')
  }
})

// => "混入对象的钩子被调用"
// => "组件钩子被调用"
```
5. <font color="red">注意：Vue.extend() 和 new Vue()的方式采取相同的合并策略；</font>
##### 全局混入：需要慎重使用
1. 可以全局注册混入对象，但是一旦使用全局混入对象，将会影响到之后创建的Vue实例，使用恰当时，可以为自定义对象注入处理逻辑：
```
// 为自定义的选项 'myOption' 注入一个处理器。
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!"
```
##### 自定义选项合并策略
1. 可以通过向<font color="red">Vue.config.optionMergeStrategies</font>添加一个函数：
```
Vue.config.optionMergeStrategies.myOption=function(toVal,fromVal){
    // return mergedVal
    
}

//例如：如果想采用methods的合并策略
var strategies = Vue.config.optionMergeStrategies;
strategies.myOption=strategies.methods;
```
#### 自定义指令
1. Vue允许注册自定义指令，如果需要对普通DOM元素进行底层操作，这时候就会用到自定义指令；
```
//注册组件
Vue.component('blog-post',{
    props:['post'],
    template:`<div class="blog-post">
        <h3>{{post.title}}</h3>
        <button v-on:click="$emit('enlarge',post.large)">放大</button>
        <div v-html="post.content"></div>
        <button v-on:click="$emit('smaller',post.small)">缩小</button>
        <slot name="test"></slot>
    </div>`,
})
```
全局自定义指令和注册局部指令  
```
Vue.directive("focus",{
    // 当被绑定的元素插入到DOM中时
    inserted: function(el){
        //聚焦元素
        el.focus();
    },
})

//或者 注册局部指令，可以在组件中指定一个directives的选项
directives: {
    focus: {
        //指令的定义
        inserted: function(el){
            el.focus();
        }
    }
}

// 使用的时候：可以在模版中的任何元素上使用新的v-focus属性

<input v-focus>
```
##### 钩子函数
1. 一个指令定义对象可以提供如下几个钩子函数：
```
bind：只调用一次，指令第一次绑定到元素时调用；
unbind：调用一次，指令与元素解绑时调用；
inserted：被绑定元素插入父节点时调用；
update：未确定是否全部更新完成
componentUpdated：指令所在组件的VNode及其子VNode全部更新后调用；
```
##### 钩子函数参数
```
el: 指令所绑定的元素，可以用来直接操作DOM

binding: 一个对象，包含以下属性：
  name：指令名，不包括v-前缀
  value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2；
  oldValue：指令绑定的前一个值，仅在update和componentUpdated钩子中可用；
  expression：字符串形式的指令表达式，例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"
  arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"
  modifiers：指令的修饰符对象，例如：v-my-directive.foo.bar 中，修饰符对象为 { foo: true, bar: true }
  
vnode: Vue编译生成的虚拟节点

oldVnode: 上一个虚拟节点，仅在update和componentUpdated钩子中使用
```
除了el之外，其他参数都是只读的
```
<div id="hook-arguments-example" v-demo:foo.a.b="message"></div>

Vue.directive('demo', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
})

new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'hello!'
  }
})
name: "demo"
value: "hello!"
expression: "message"
argument: "foo"
modifiers: {"a":true,"b":true}
vnode keys: tag, data, children, text, elm, ns, context, fnContext, fnOptions, fnScopeId, key, componentOptions, componentInstance, parent, raw, isStatic, isRootInsert, isComment, isCloned, isOnce, asyncFactory, asyncMeta, isAsyncPlaceholder

```
##### 函数简写
1. 在自定义指令的时候，可以忽略掉一些不关心的钩子函数
```
Vue.directive("color-switch",function(el,binding){
    el.style.backgroundColor= binding.value;
})
```
##### 对象字面量
1. 如果指令需要多个值，也可以传入一个JavaScript对象字面量，
2. 指令函数能够接受所有合法的JavaScript表达式
```
// 自定义的指令
<div v-demo="{ color: 'white', text: 'hello!' }"></div>

// 使用
Vue.directive('demo', function (el, binding) {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text)  // => "hello!"
})
```

##### 生产环境
在webpack 4+中，可以使用mode选项： 
```
module.exports = {
    mode: 'production'  // 生产环境模式
}
```
Vue源码会根据 `process.env.NODE_ENV`来决定是否启用生产环境模式，默认情况为开发环境模式  

预编译模版最简单的方式就是使用单文件组件，相关的构建设置会自动把预编译处理好；所以构建好的代码已经包含了编译出来的渲染函数而不是原始的模版字符串

组件渲染时出现运行时错误，通过 Vue.config.errorHandler 配置函数

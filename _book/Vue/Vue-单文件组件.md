> 单文件组件（SFC）

```
<template>
    <p>{{greeting}} world!</p>
</template>

<script>
    module.exports = {
        data: function(){
            return {
                greeting: 'hello',
            };
        }
    }
</script>

<!-- scoped 实现vue组件的样式私有化（模块化）-->
<style scoped>
    p {
        font-size: 2em;
        text-align: center;
    }
</style>
```
在使用单文件组件的时候，需要在js文件里import导入文件，才可以实例和继续使用

> vue的异步编程

```

var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // 更改数据
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})



在组件内使用 vm.$nextTick() 实例方法特别方便，因为它不需要全局 Vue ，并且回调函数中的 this 将自动绑定到当前的 Vue 实例上：

Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: '没有更新'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = '更新完成'
      console.log(this.$el.textContent) // => '没有更新'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => '更新完成'
      })
    }
  }
})
```
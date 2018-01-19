## 6-6、vue-router的重定向（redirect）
开发中有时候我们虽然设置的路径不一致，但是我们希望跳转到同一个页面，或者说是打开同一个组件。这时候我们就用到了路由的重新定向redirect参数。<br>
比如两个链接都是跳转到一个页面上。<br>
这里我们要求，在App.vue中给一个router-link标签，让这个链接跳转到Home页面。<br>
首先，我们需要再index.js中的route里声明。<br>
例，在index.js中：<br>
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi1 from '@/components/hi1'
import hi2 from '@/components/hi2'
import params from '@/components/params'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component:HelloWorld
    },
    {
    	path:'/params/:newsId(\\d+)/:newsName',
    	component:params
    },
    {
    	path:'/goHome',
    	redirect:'/'  				//这里使用了redirect来重定向，这个重定向后面的值为要定向的路径
    } 
  ]
})
```
总结：要定向的path是什么，那么重定向就是什么，并且这也不算组件，所以也可以不用加载。<br>

**重定向如果需要值传入：**
有时候重定向也需要值的传入，比如我们在App.vue中给一个router-link标签，然后将其重定向到Params页面，并且传递新的值。<br>
那么首先需要在index.js中定义传递值的格式。<br>
例，在index.js中：
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi1 from '@/components/hi1'
import hi2 from '@/components/hi2'
import params from '@/components/params'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component:HelloWorld
    },
    {
    	path:'/params/:newsId(\\d+)/:newsName',
    	component:params
    },
    {
    	path:'/goHome',
    	redirect:'/'
    },
    {
    	path:'/goParams/:newsId(\\d+)/:newsName',		//首先path中定义数值的格式，	
    	redirect:'/params/:newsId(\\d+)/:newsName' 		//然后在redirect中定向好页面
    }
  ]
})
```
总结，在有值的情况下，redirect的值就是对应页面的path的值，直接粘贴复制就可以了。<br>
然后在App.vue中穿件router-link标签。<br>
例，在App.vue中：<br>
```vue
<template>
  <div id="app">
    <p><img src="./assets/logo.png"></p>
    <router-link to="/">Home</router-link> |
    <router-link to="/params/233/tianer">Params</router-link> |
    <router-link to="/goHome">goHome</router-link>  |
    <router-link to="/goParams/666/angel">goParams</router-link>   //注意这里传递了新的值
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```
总结：router-link里面to属性对应的路径要跟path的一致，在有值的情况下直接写入具体值。<br>
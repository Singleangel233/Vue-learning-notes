# Vue学习笔记5-Vue-router
## 6-1、vue-router的安装和含义
vue-router，简称路由，是Vue全家桶中的一个组件，主要是用于制作单页应用。<br>
单页应用的意义在于不使用```<a href=""></a>```标签，我们就使用一个页面来管理应用。<br>

**安装：** <br>
如果在使用vue-cli初始化项目过，那么就会在命令行里提示是否安装vue-router。<br>
否则在命令行里面输入```npm install vue-router --save-dev``` 进行安装<br>


**关于src文件夹中的router文件中的Index.js解读：** <br>
```javascript
import Vue from 'vue'  //引入了vue组件包
import Router from 'vue-router'   //引入了vue-router组件
import HelloWorld from '@/components/HelloWorld'   //引入的是components文件夹中的HelloWorld.vue文件，这个文件也可以叫做模板

Vue.use(Router)  //全局使用router组件

export default new Router({   //写了一个接口定义router
  routes: [   //配置routes，格式为数组，里面为对象
    {
      path: '/',     //路径
      name: 'HelloWorld',     
      component: HelloWorld   //组件，对应上面的components/HelloWorld
    }
  ]
})
```

**创建自己的一个hi.vue页面：** <br>
首先应该在components文件下创建一个hi.vue。<br>
vue的文件格式首先得满足格式：
```
<template></template>
<script></script>
<style></style>
```
例：<br>
hi.vue：<br>
```vue
<template>
	<div>
		<h2>{{message}}</h2>
	</div>
</template>
<script>
	export default{
		name:'hi',
		data(){    //注意这里data的格式
			return{
				message:"this is my hi page"
			}
		}
	}
</script>
<style>
	
</style>
```
创建完毕后，需要再index.js中加载，并且在routes中配置好对象属性（配置路由）。<br>
例，在index.js中： <br>
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi from '@/components/hi'    //加载刚好创建的hi.vue

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {  
    	path:'/hi',
    	name:'hi',
    	component:hi
    } 							//配置路由
  ]
})
```

**关于里面的图片：**<br>
图片是在App.vue文件中出现的。如果我们对里面进行加入内容，那么HelloWorld.vue和hi.vue都会受到影响。 <br>
例，在App.vue中：<br>
```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
    111   //自己添加的111
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
然后在两个页面中都会出现111。<br>

**关于链接导航（重要）：**<br>
使用一组新的标签，可以在vue-router中创建导航。<br>
```vue 
<router-link> <router-link>
```
这组标签可以起到```<a href=""></a>```的链接效果，能够实现链接的功能。<br>
例，在App.vue中： <br>
```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
    <div>
      <router-link to="/">首页</router-link>  |
      <router-link to="/hi">hi页面</router-link>   //配合to属性到指定的页面
    </div>
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
这样就能实现页面跳转的效果，作为单页面应用，几乎没有延迟的跳转。<br>

## 6-2、子路由

**子路由的含义：** <br>
一个页面里首先有它自己的模板，然后它下面的页面都隶属于这个模板，只是部分改变样式。<br>
例如在hi.vue页面下建立两个页面hi页面1.vue和hi页面2.vue。<br>

**实现子路由：**<br>
首先在App.vue创建出hi页面的子路由页面的链接<br>
例，在App.vue中：<br>
```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
    <div>
      <router-link to="/">首页</router-link>  |
      <router-link to="/hi">hi页面</router-link>  |
      <router-link to="/hi/hi1">hi页面1</router-link>  |
      <router-link to="/hi/hi2">hi页面2</router-link>    //新增router-link标签创建链接
    </div>
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
然后在hi.vue的文件目录中创建hi1.vue，hi2.vue，并且使用```<router-view/>```标签，这个标签的功能就是展示组件的内容。<br>
例,在hi1.vue中：<br>
```vue
<template>
	<div>
		<h2>{{message}}</h2>
		<router-view/>	//加上router-view标签，显示其父路由的内容
	</div>
</template>
<script>
	export default{
		name:'hi1',
		data(){
			return{
				message:"It's hi1 page"
			}
		}
	}
</script>
<style>
	
</style>
```
hi2.vue也与此同理。<br>
设置完毕后，
先在index.js中加载刚才写好的组件。<br>
然后再index.js中使用children关键字声明，children的属性格式为数组，里面为对象，对象里面的属性一个为路径，一个为声明的组件。<br>
例，在index.js中：<br>
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi from '@/components/hi'
import hi1 from '@/components/hi1'
import hi2 from '@/components/hi2'  //加载好写好的组件

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
    	path:'/hi',
    	name:'hi',
    	component:hi,			//注意component还是写上了
    	children:[
    	{path:'/',component:hi}, 
    	{path:'hi1',component:hi1},
    	{path:'hi2',component:hi2}]    //注意格式为数组，里面是对象的形式，并且path（路径）注意在子路由上不需要加/符号
    }
  ]
})
```

**总结：**<br>
使用子路由可以实现一个主模块下附带多个子模块，然后可以方便地设计界面，比如某个专题下的各类子页面。
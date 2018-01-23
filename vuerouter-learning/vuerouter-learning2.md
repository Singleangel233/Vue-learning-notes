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

## 6-7、alias别名的应用（跟重定向有点相似）
使用alias属性，能够起到跟重定向(redirect)相似的效果，然而alias会对应到url地址栏上面改变对应的值。<br>
例，这次我们在App.vue中，新增两个router-link，来实现alias的效果。<br>
在App.vue中：<br>
```vue
<template>
  <div id="app">
    <p><img src="./assets/logo.png"></p>
    <router-link to="/hi1">hi1</router-link> |
    <router-link to="/tianer">tianer</router-link> //这边添加了两个链接，并且绑定了to属性
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
然后再index.js中配置好绑定的组件。<br>
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
    	path:'/hi1',
    	component:hi1,
    	alias:'/tianer'  //这里使用了alias
    }
  ]
})
```
这里使用了alias别名，属性值为'/tianer'，对应到App.vue中的router-link中绑定的to属性。<br>
配置完成后，运行页面，点击tianer链接，实际上的效果跟重定向相似，但是地址栏上面会发生变化，会加上/tianer。<br>
<br>
**alias的注意点：**<br>
要注意，alias的属性不适用于主页，也就是无法通过alias跳到主页上去。<br>

## 6-8、路由过渡动画
我们可以使用过渡动画来展现页面的特效，实现这个效果的标签为```<trasition></transition>```标签。<br>
首先在App.vue中，由于展示页面内容是router-view标签中，所以使用transition标签将其包裹。<br>
例，在App.vue中：<br>
```vue
<template>
  <div id="app">
    <p><img src="./assets/logo.png"></p>
    <router-link to="/">Home</router-link> |
    <router-link to="/params/233/tianer">Params</router-link> |
    <router-link to="/goHome">goHome</router-link>  |
    <router-link to="/goParams/666/angel">goParams</router-link> |
    <router-link to="/hi1">hi1</router-link> |
    <router-link to="/tianer">tianer</router-link>

    <transition name="animate" mode="out-in">				//注意这里使用了transition标签将其包裹起来
    <router-view></router-view>
    </transition>

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
注意：transition标签有两个属性，一个属性为name，一个属性为mode。<br>
name属性：用于取一个name值（任意），然后再style标签中方便定义transition的样式。<br>
其中，name属性会在style样式设置的时候，有着特定的格式写法，例：<br>
.name-enter：表示进入时的样式。
.name-enter-active：表示进入发生时的样式（一般用transition来描述）。
.name-leave：表示离开时的样式。
.name-leave-active：表示离开发生时的样式（一般用transition来描述）。


mode属性：有两个属性值可以选择，一个是in-out(默认)，一个是out-in，out-in可以流畅地配合动画的展示。<br>


接下来在App.vue中定义style样式：<br>
例，在App.vue中的style中：<br>
```vue
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.animate-enter{
  opacity: 0;       //这是设置的进入时的透明度
}
.animate-enter-active{
  transition: opacity .5s;    //这是设置的进入时候发生的过渡属性以及时间
}
.animate-leave{ 	//这是设置离开的时候的透明度
  opacity: 1;
}
.animate-leave-active{
  transition: opacity .5s;   //这是设置离开时候发生的过渡属性以及时间
}
</style>
```
总结：为了让页面的展现变得美丽，使用了过渡的动画来展现，同时还可以配合更多的CSS动画和JS动画来实现效果。<br>

## 6.9、mode的作用和404页面的处理
之前我们接触了mode，是用在了transition标签中，来改变过渡动画，这一次的mode是在index.js中设置的。<br>
作用：用于改变地址栏url的格式，默认的情况会有```\#\```的格式，但是设置了mode值后，就可以改变了。<br>
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
  mode:'history',    //这里加了mode属性，添加了字符串history
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
    }
  ]
})
```
注意：mode属性在里面的写的位置，以及后面是字符串history。<br>

**404页面的处理：**<br>
有时候会在浏览器上输入错误出现404页面，通过在index.js里面声明组件可以处理错误页面的问题，来给用户提示。<br>
例，我们在index.js中声明一个error组件：<br>
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi1 from '@/components/hi1'
import hi2 from '@/components/hi2'
import params from '@/components/params'
import error from '@/components/error'


Vue.use(Router)

export default new Router({
  mode:'history',
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
    	path:'/goParams/:newsId(\\d+)/:newsName',
    	redirect:'/params/:newsId(\\d+)/:newsName'
    },
    {
    	path:'/hi1',
    	component:hi1,
    	alias:'/tianer'
    },
    {
    	path:'*',					//注意这里就是声明的error组件，要注意path的格式
    	component:error
    }
  ]
})
```
注意：path的格式，因为是处理404页面，那么```'*'```就代表404页面的情况。<br>
然后在顶部加载error组件，并且我们在component目录下创建一个error.vue来表示错误的页面的显示。<br>
例，在error.vue中：<br>
```vue
<template>
	<div>
		<h2>404 ERROR</h2>
		<h3>{{message}}</h3>
	</div>
</template>

<script>
	export default {
		name:'error',
		data(){
			return{
				message:'you are in a wrong page'
			}
		}
	}
</script>

<style>
	
</style>
```
然后在App.vue创建一个链接，不过to属性所指向的链接是错误的，用于实现效果。<br>
例，在App.vue中：<br>
```vue
<template>
  <div id="app">
    <p><img src="./assets/logo.png"></p>
    <router-link to="/">Home</router-link> |
    <router-link to="/params/233/tianer">Params</router-link> |
    <router-link to="/goHome">goHome</router-link>  |
    <router-link to="/goParams/666/angel">goParams</router-link> |
    <router-link to="/hi1">hi1</router-link> |
    <router-link to="/tianer">tianer</router-link>
    <router-link to="/2345678">错误实例</router-link>   //创建了一个新的链接，点击后就会展示出error页面的内容
    <transition name="animate" mode="out-in">
    <router-view></router-view>
    </transition>
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
.animate-enter{
  opacity: 0;
}
.animate-enter-active{
  transition: opacity .5s;
}
.animate-leave{
  opacity: 1;
}
.animate-leave-active{
  transition: opacity .5s;
}
</style>
```

## 6-10、路由中的钩子函数
路由中的钩子函数（生命周期函数），一共分了两种。<br>
一种是进入的时候的钩子函数，一种是离开时候的钩子函数。<br>
写入钩子函数也有两种写法。

**第一种：在index.js中配置钩子函数**<br>
例，在index.js中：<br>
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi1 from '@/components/hi1'
import hi2 from '@/components/hi2'
import params from '@/components/params'
import error from '@/components/error'


Vue.use(Router)

export default new Router({
  mode:'history',
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component:HelloWorld
    },
    {
    	path:'/params/:newsId(\\d+)/:newsName',
    	component:params,
    	beforeEnter:(to,from,next)=>{
    		console.log(to);
    		console.log(from);
    		next();							//注意这里写了钩子函数
    	}
    },
    {
    	path:'/goHome',
    	redirect:'/'
    },
    {
    	path:'/goParams/:newsId(\\d+)/:newsName',
    	redirect:'/params/:newsId(\\d+)/:newsName'
    },
    {
    	path:'/hi1',
    	component:hi1,
    	alias:'/tianer'
    },
    {
    	path:'*',
    	component:error
    }
  ]
})

```
使用这个钩子函数beforeEnter，可以实现在进入params页面前就执行。<br>
里面的三个参数的含义：<br>
<br>
to：是一个对象，里面包含了很多属性，其中有很关键的前往的路径，还有具体发送的参数。<br>
from：是一个对象，包含了来自于哪里，里面也有很多相关的属性。<br>
next：在函数内必须写next()，只有执行了这个，才有页面内容的显示，同时也可以使用next(false)来达到不显示的效果,并且next里面可以跟对象的形式。<br>
例，next里面为对象，里面可以配置path：<br>
```next({path:'/'})```
这样就点击后就可以跳到主页<br>
<br>
**第二种，在模板里面配置钩子函数：**<br>
例，在params.vue中：
```vue
<template>
	<div>
		<h2>{{ message }}</h2>
		<p>newsID:{{$route.params.newsId}}</p>
		<p>newsName:{{$route.params.newsName}}</p>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				message:"this is params page"
			}
		},
		beforeRouteEnter:(to,from,next)=>{
			console.log('进入了params模板');  //注意这里写法跟在index.js中的写法不一样
			next();
		},
		beforeRouteLeave:(to,from,next)=>{
			console.log('离开了params模板');  //这里写了一个离开的钩子函数
			next();
		}
	}
</script>

<style>
	
</style>
```
实现的效果跟在index.js中实现的效果相似，但是在模板里写可以写离开的钩子函数。<br>
注意：都要加上next()，不然显示不出页面内容。<br>


## 6-11、vue-router编程式导航
我们可以让页面中的组件绑定方法实现导航的效果，比如使用按钮，给它绑定一个内置方法来实现跳转页面的效果。<br>
例，在App.vue中，我们使用三个按钮分别实现，后退，前进，返回首页的效果。<br>
在App.vue中：<br>
```vue
<template>
  <div id="app">
    <p><img src="./assets/logo.png"></p>
    <p><button @click="goBack">返回</button>
    <button @click="goGo">前进</button>
    <button @click="goHome">返回首页</button></p>    //这里使用了v-on来绑定方法
    <router-link to="/">Home</router-link> |
    <router-link to="/params/233/tianer">Params</router-link> |
    <router-link to="/goHome">goHome</router-link>  |
    <router-link to="/goParams/666/angel">goParams</router-link> |
    <router-link to="/hi1">hi1</router-link> |
    <router-link to="/tianer">tianer</router-link>
    <router-link to="/2345678">错误实例</router-link>
    <transition name="animate" mode="out-in">
    <router-view></router-view>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'app',
  methods:{
      goBack(){
        this.$router.go(-1);
      },
      goGo(){
        this.$router.go(1);
      },
      goHome(){
        this.$router.push('/');    //这里绑定了方法，这里有两种方法
      }
  }
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
.animate-enter{
  opacity: 0;
}
.animate-enter-active{
  transition: opacity .5s;
}
.animate-leave{
  opacity: 1;
}
.animate-leave-active{
  transition: opacity .5s;
}
</style>

```
注意：在绑定的方法中，```this.$router.go()```方法是实现了跳转页面的功能。<br>
在```this.$router.push()```方法中，push()的值为路径，跟path相似，可以实现跳转指定页面的效果。<br>
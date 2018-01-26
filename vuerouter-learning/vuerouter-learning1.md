# Vue学习笔记6-Vue-router
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

## 6.3、参数传递
参数传递一般是业务中的基础需求，然后我们需要一些参数传递才能达到目的。<br>

### 第一种：使用name来传递参数
如果需要传递参数，那么首先可以在index.js配置name属性的属性值。<br>
例，在index.js中配置好HelloWorld组件的name值：<br>
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi from '@/components/hi'
import hi1 from '@/components/hi1'
import hi2 from '@/components/hi2'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',   //注意是在这里配置name值
      component: HelloWorld
    },
    {
    	path:'/hi',
    	component:hi,
    	children:[
    	{path:'/',component:hi}, 
      {path:'hi1',component:hi1},
      {path:'hi2',component:hi2}]   
    }
  ]
})
```
然后在App.vue文件中，使用标签配置插值来引用。<br>
例，在App.vue中：<br>
```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <div>
      <router-link to="/">首页</router-link>  |
      <router-link to="/hi">hi页面</router-link>  |
      <router-link to="/hi/hi1">hi页面1</router-link>  |
      <router-link to="/hi/hi2">hi页面2</router-link>
    </div>
    <p>{{$route.name}}</p>   //配合引用的关键，使用$route.name可以获取到index.js中的name属性
    <router-view/>
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
关键点：配合{{$route.name}}这个规定的形式来得到name值，从而获取到name值，写法可以放到外面。<br>

**如果有子路由的情况** <br>
如果一个路由是有子路由的话，那么它的name值不能单独写出，需要在声明的路由和子路由中添加name值来表示。
例，在index.js中：
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi from '@/components/hi'
import hi1 from '@/components/hi1'
import hi2 from '@/components/hi2'

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
    	component:hi,
    	children:[
    	{path:'/',name:'hi',component:hi},
    	{path:'hi1',name:'hi下的hi1',component:hi1},
    	{path:'hi2',name:'hi下的hi2',component:hi2}]   //这里是声明的子路由，然后在子路由中表明name值
    }
  ]
})
```

### 第二种：修改router-link标签来传递值
这种方法是修改了```<router-link></router-link>```标签中的属性。<br>
例，我们要点击hi1链接中，传递一个userName和id的值。<br>
在App.vue中：<br>
```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <div>
      <router-link to="/">首页</router-link>  |
      <router-link to="/hi">hi页面</router-link>  |
      <router-link :to="{name:'hi下的hi1',params:{userName:'tianer',id:'233'}}">hi页面1</router-link>  | //注意这里面的格式
      <router-link to="/hi/hi2">hi页面2</router-link>
    </div>
    <p>{{ $route.name}}</p>
    <router-view/>
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
在router-link标签中，使用了v-bind绑定了标签中的属性to，然后值为对象，对象里面的属性有name和params，其中params的格式也为对象，可以传多个值。<br>
设定好params里面的值后，然后注明userName和id，即可表明是传递的值。<br>

然后在hi.vue中的template中使用插值的形式插入进去要传递的值。<br>
例，在hi.vue中：<br>
```vue
<template>
	<div>
		<h2>{{message}}-{{$route.params.userName}}-{{$route.params.id}}</h2>  //注意插值的格式
		<router-view/>	
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
要注意插值的格式，是$route.params.属性名。<br>
这样在运行中，点击hi1链接后，会出现传递的值的具体内容，并且出现在页面中。<br>

## 6-4、单页面多路由区操作
使用这种方式，可以在同一页面显示多个页面，并且可以根据传的值来控制多路由。<br>
例，我们要实现在一个页面显示多个页面的内容，首先在App.vue中使用router-view标签。<br>
在App.vue中：<br>
```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view></router-view>
    <router-view name="left"></router-view>
    <router-view name="right"></router-view>  //这边使用了router-view标签，并且赋予了name值
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
引入router-view标签完毕后，然后在index.js中引用组件，声明left和right所绑定的组件的名字。<br>
例，在index.js中：<br>
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi1 from '@/components/hi1'
import hi2 from '@/components/hi2'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      components:{
      	default:HelloWorld,
      	left:hi1,
      	right:hi2    //要注意声明的格式，写法为components，格式为对象
      }
    }
  ]
})

```
在routes里面引用完毕后，则再声明一下left和right对应的组件名字，要注意声明一个default。接着还要注意在顶部import一下编写好的hi1和hi2。<br>


编写好的hi1.vue: <br>
```vue
<template>
	<div>
		<h2>{{message}}</h2>	
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
<style scoped>   //注意这边用了scoped来只在当前页展示效果
	div {
		float: left;
		width: 50%;
		height: 300px;
		background-color: skyblue;
	}
</style>
```

编写好的hi2.vue <br>
```vue
<template>
	<div>
		<h2>{{message}}</h2>
	</div>
</template>
<script>
	export default{
		name:'hi2',
		data(){
			return{
				message:"It's hi2 page"
			}
		}
	}
</script>
<style scoped>
	div {
		float: left;
		width: 50%;
		height: 300px;
		background-color: hotpink;
	}
</style>
```
最后在页面中就可以得到想要的效果，hi1和hi2页面都在页面下方，在左右两边分别展示出来。

**改变path参数后对路由的控制：**<br>
我们可以在index.js中对routes增加一个对象，来表示实现改变数值后对路由的控制。<br>
例，在index.js中：<br>
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import hi1 from '@/components/hi1'
import hi2 from '@/components/hi2'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      components:{
      	default:HelloWorld,
      	left:hi1,
      	right:hi2
      }
    },
    {
      path: '/tianer',   //注意这边改变了path的值
      name: 'HelloWorld',
      components:{
      	default:HelloWorld,
      	left:hi2,
      	right:hi1     //这边简单对调了一下hi1和hi2
      }
    }
  ]
})
```
这里改变的path的值，里面的/tianer的作用就是，可以在浏览器地址栏输入tianer，然后实现改变left和right对应的hi1和hi2的页面的改变。<br>
输入完成后，页面发生的变化为：hi1和hi2相互交换了位置。<br>

## 6-5、vue-router使用url进行参数的传递
之前讲过使用vue-router进行传值，这次讲一种使用url传值的方法。<br>
首先，创建一个params.vue页面，并且在index.js中声明这个组件。<br>
例，在params.vue中：<br>
```vue
<template>
	<div>
		<h2>{{ message }}</h2>
		<p>newsID:{{$route.params.newsId}}</p>
		<p>newsName:{{$route.params.newsName}}</p>  //这里留着备用，用于之后接受参数并且显示
	</div>
</template>

<script>
	export default{
		data(){
			return{
				message:"this is params page"
			}
		}
	}
</script>

<style>
	
</style>
```
例，在index.js中声明组件：<br>
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
    	path:'/params/:newsId/:newsName',  //要注意这里开始使用一种新的传值方法，注意冒号！
    	component:params
    }
  ]
})
```
要注意在route第二个对象中，使用了一种新的传值的方法，path中不仅表达了params的路径，并且用了/:newsID/:newsName这种方式传递值。<br>
这次我们实例，用newsID表示新闻号码，newsName表示新闻名字，要注意冒号的使用，这是格式。<br>
然后在App.vue中使用router-link标签来传递值。<br>
例，在App.vue中：
```vue
<template>
  <div id="app">
    <p><img src="./assets/logo.png"></p>
    <router-link to="/">Home</router-link>
    <router-link to="/params/as233/tianer">Params</router-link>    //注意这里传值的格式
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
这里我们用了两个链接标签，一个是回到主页的Home，一个是进入到params页面的params，点击链接后还传入了值，使用的to来设置传入的值，对应设置好的参数。<br>
然后点击params链接，就显示params内容，并且接收了值的传递。<br>

**使用正则表达式来规定传值的格式：**<br>
我们可以在index.js规定传值的格式，使用正则表达式来规定传值的格式。<br>
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
    	path:'/params/:newsId(\\d+)/:newsName',   //要规定传值的格式，需要用括号，并且在里面规定
    	component:params
    }
  ]
})

```
要规定的格式就是，在要传入的值后面加入括号，里面使用正则表达式，例如```(\\d)```是表示只能传递数字。<br>
如果传入的值有字母，那么点击链接就直接载入不了页面内容。<br>

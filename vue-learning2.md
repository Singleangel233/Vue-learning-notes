# Vue学习笔记2
## 1、Vue-directive自定义指令
Vue-directive:用于配合Vue中给的API定义自己想要的功能<br>
例：用Vue-directive自定一个改变颜色为红色的功能v-tianer(v-是命名的方式) <br>	
在body中：
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>practice</title>
</head>
<body>
	<div id="app">
		<div v-tianer="color">{{num}}</div>
		<p><button @click="jiafa">加法</button></p>
	</div>
</body>
</html>
```
我们在div中，绑定了我们自己设置的v-tianer <br>	
在script中：
```
<script>
	Vue.directive('tianer',function(el,binding,vnode){
		el.style='color:'+binding.value;  //js中的style方法调用
	});
	var app = new Vue({
	el:'#app',
	data:{
		num:10,
		color:'red'
	},
	methods:{
		jiafa:function(){
			this.num++;
		}
	}
})
</script>
```
在script中，我们使用了Vue.directive方法来定义我们绑定的v-tianer，在function中的三个函数的意义分别为：  
- el:指令所绑定的元素，用于直接操作DOM（实际上该el在本例里指的是id为app的div）。  
- binding:一个对象，包含很多指令信息，里面有很多属性  
			binding.name指的是我们v-tianer中的tianer；  
			binding.value是data中的red；  
			binding.expression指的是绑定的属性；（因为v-tianer绑定data中的color，而通过binding.value可以获取到data中的color的值）。  
- v-node:Vue编译形成的一个虚拟节点。最后页面中的改变颜色的颜色为color中的red红色。  


自定义指令的五个生命周期（也可以叫钩子函数）<br>
分别是：bind，inserted，update，componentUpdate，unbind<br>
1. bind：只调用一次，指令第一次绑定到元素时使用，使用这个钩子函数可以定义一个绑定时只执行一次的初始化动作。<br>
2. inserted：被绑定元素插入父节点时调用（父节点存在时即可调用，不必存在于document中）。<br>
3. update：被绑定元素所在的模板更新（比如改变值的时候）时调用，无论绑定值是否变化，通过比较更新前后的绑定值，可以忽略不必要的模板变化<br>
4. componentUpdate：被绑定元素在模板完成一次更新周期（比如更新完成时）时调用。<br>
5. unbind：只调用一次，在于元素解绑时候调用。<br>
例：尝试在script中输出五个生命周期<br>
```
Vue.directive('tianer',{
		bind:function(el,binding){//被绑定
		     console.log('1 - bind');
		     el.style="color:"+binding.value; //自定义指令的具体功能在开始绑定的时候写
		},
		inserted:function(){//绑定到节点
		      console.log('2 - inserted');
		},
		update:function(){//组件更新
		      console.log('3 - update');
		},
		componentUpdated:function(){//组件更新完成
		      console.log('4 - componentUpdated');
		},
		unbind:function(){//解绑
		      console.log('1 - bind');
		}
		});
```
关于解绑：<br>
解绑（unbind）的例子：<br>
在作用域的外边写一个按钮，用于解除绑定<br>
在body中：
```
<body>
	<div id="app">
		<div v-tianer="color">改变颜色</div>
		<p>{{message}}</p>
	</div>
	<p><button onclick="unbind()">解绑</button></p>
</body>
```
在script中，使用原生的方式，创建unbind函数(放在vue实例化对象之外单独创建)：<br>
```
function unbinde(){
		app.$destroy();
	}
```
当点击解绑按钮的时候，则会解除原本加法按钮绑定的jiafa方法。并且，在此时控制台输出bind钩子函数的情况<br>


## 2、Vue-extend扩展实例构造器：
Vue-extend简单来说可以构造自己标签，并且为其进行自己的定义，经常用于配合Vue-component用于生成组件实例，并且挂载到自定义元素上。<br>
例：用Vue-extend来构建一个自己的tianer标签，tianer标签里可以展示自己特定的文字和拥有超链接功能：<br>
首先在script中构造：
```
<script type="text/javascript">
	var tianerExtend = Vue.extend({  //使用Vue.extend来扩展实例
		template:"<p><a :href='tianerUrl'>{{textName}}</a></p>", //加入template属性，并且赋予其属性值，同时利用v-bind来绑定数据
		data:function(){
			return{
				textName:'天儿',
				tianerUrl:"https://github.com/Singleangel233"
			}
		}
	});
	new tianerExtend().$mount('#tianer');  /*扩展完毕后，需要用Vue里面的$mount方法来直接绑定到指定元素中，
											其中$mount()中的属性，可以是标签，可以是id，也可以是class，名字*/
</script>
```
于是在body中，直接如下创建，可以得到应有的效果
```
<body>
	<h1>Vue-extend扩展实例构造器</h1>
	<hr>
	<div id="tianer"></div>
</body>
```
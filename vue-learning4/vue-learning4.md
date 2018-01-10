# Vue学习笔记4（实例和内置组件）
## 4.1、Vue和Jquery一起使用的方法和外部调用构造器内部方法
首先，实例是在构造器外部操作构造器内部的属性选项或者方法，就叫做实例。实例的作用就是给原生的<br>
或者其它的javascript框架一个接口或者说机会，便于让Vue和其它javascript框架一起使用<br>
例，在Vue构造器内部使用Jquery：<br>
在script中
```javascript
<script src="./vue.js"></script>
<script src="./jquery-3.2.1.js"></script>
<script>
	var app = new Vue({
		el:"#app",
		data:{
			message:233
		},
		mounted:function(){
			$("#app").html("singleangel");  //要使在Vue中使用Jquery，必须要在mounted钩子函数中写，因为必须挂载后才能找到dom才能使用jquery
		}
	});
</script>
```
例，在构造外部使用methods里面的方法：<br>
在script中：
```javascript
<script>
	var app = new Vue({
		el:"#app",
		data:{
			message:233
		},
		methods:{
			add:function(){
				console.log("内部方法已经调用！");
			}
		},
		mounted:function(){
			$("#app").html("singleangel");  //要使在Vue中使用Jquery，必须要在mounted钩子函数中写，因为必须挂载后才能找到dom才能使用jquery
		}
	});
	app.add(); //这是属于在外部调用构造器中methods里方法
</script>
```
注意```app.add();```这是在构造器外部调用内部methods的add方法。<br>
### 4.2、实例和内置组件
创建一个扩展，并且声明它的一个实例，然后使用内置的方法。<br>
1.$mount()：<br>
用于扩展成功后使用，称为挂载，挂载完毕后才能使用。<br>
例，扩展一个实例并且挂载：<br>
在script中：
```javascript
<script>
	var tianer = Vue.extend({
		template:`<p>{{message}}</p>`,
		data:function(){
			return{   //扩展实例的格式里的return格式写法
				message:233
			}
		},
		mounted:function(){
			console.log("挂载方法之后就出现"); //这个生命周期用于检测挂载完毕
		}
	})
	var vm = new tianer().$mount("#app"); //创建完成后，注意挂载到id为app的标签中，这边声明了一个变量vm，用于后续的学习中
</script>
```
在body中：
```html
<body>
	<h1>example methods demo</h1>
	<hr>
	<div id="app"><p></p></div>
</body>
```
挂载完毕后，就会在页面中出现对应的内容<br>
2.$destroy(): <br>
用于销毁，可以直接销毁整个挂载。<br>
例，创建一个button按钮用于实现销毁的方法，在script中：
```javascript
<script>
	var tianer = Vue.extend({
		template:`<p>{{message}}</p>`,
		data:function(){
			return{
				message:233
			}
		},
		mounted:function(){
			console.log("挂载方法之后就出现");
		},
		destroyed:function(){
			console.log("destroy方法执行了就出现"); //创建一个钩子函数，当实现destroyed生命周期就会返回对应信息
		}
	})
	var vm = new tianer().$mount("#app");
	function destroy(){
		vm.$destroy();  //在外部直接定义销毁的方法，调用$destroy()
	};
</script>
```
例，在body中定义button：
```html
<body>
	<h1>example methods demo</h1>
	<hr>
	<div id="app"><p></p></div>
	<p><button onclick="destroy()">destory</button></p>
</body>
```
点击按钮后，控制台会出现对应的信息<br>
3.$forceUpdate() <br>
这个方法用于更新数据<br>
例，创建一个更新按钮，点击之后就会更新数据：<br>
在script中：
```javascript

<script>
	var tianer = Vue.extend({
		template:`<p>{{message}}</p>`,
		data:function(){
			return{
				message:233
			}
		},
		mounted:function(){
			console.log("挂载方法之后就出现");
		},
		destroyed:function(){
			console.log("destroy方法执行了就出现");
		},
		updated:function(){
			console.log("执行了reload后就会出现"); //创建一个钩子函数，执行更新后返回对应的信息
		}
	})
	var vm = new tianer().$mount("#app");
	function destroy(){
		vm.$destroy();
	};
	function reload(){
		vm.$forceUpdate();  //在外部创建了方法并且绑定到按钮上
	} 
</script>
```
在html中：
```html
<body>
	<h1>example methods demo</h1>
	<hr>
	<div id="app"><p></p></div>
	<p><button onclick="destroy()">destory</button></p>  
	<p><button onclick="reload()">reload</button></p>  
</body>
```
点击reload按钮，控制台会返回对应的信息。<br>
4.$nextTick() <br>
当Vue的data值修改成功后就会调用这个方法，相当于一个钩子函数，跟构造器的update周期特别相似。<br>
例，创建一个按钮，更改data中的内容，更改完毕后调用这个方法返回信息则证实调用成功：<br>
在script中：
```javascript
<script>
	var tianer = Vue.extend({
		template:`<p>{{message}}</p>`,
		data:function(){
			return{
				message:233
			}
		},
		mounted:function(){
			console.log("挂载方法之后就出现");
		},
		destroyed:function(){
			console.log("destroy方法执行了就出现");
		},
		updated:function(){
			console.log("执行了reload后就会出现");
		}
	})
	var vm = new tianer().$mount("#app");
	function destroy(){
		vm.$destroy();
	};
	function reload(){
		vm.$forceUpdate();
	}
	function tick(){
		vm.message="已经修改完毕";  //注意这里更改信息的格式
		vm.$nextTick(function(){    //数据修改后，就会调用这个函数返回对应的信息
			console.log("修改完成后就调用了");
		});
	}
</script>
```
在body中：
```html
<body>
	<h1>example methods demo</h1>
	<hr>
	<div id="app"><p></p></div>
	<p><button onclick="destroy()">destory</button></p>
	<p><button onclick="reload()">reload</button></p>
	<p><button onclick="tick()">nextTick</button></p>
</body>
```
点击按钮后对修改message上的信息，并且在控制台打印出对应的内容。<br>

### 4-3、实例事件
实例事件主要是在外部创建构造器内部的事件，这样做的好处就是在构造器外部调用构造器内部的数据。<br>
1.$on() <br>
这个方法用于创建外部事件，配合emit函数使用，可以直接在构造器外部创建事件并且应用起来。<br>
这个函数有两个参数，第一个参数是方法的名字，第二个参数是方法的内容。<br>
例，创建一个reduce函数，让内部数据减少：<br>
在script中:
```javascript
<script>
	var app = new Vue({
		el:"#app",
		data:{
			message:1
		},
		methods:{
			add:function(){
				this.message++;
			}
		}
	});
	app.$on("reduce",function(){
		console.log("reduce已经调用");   //$on()方法中有两个参数，一个是方法的名字(字符串)，另一个是函数内容
		this.message--;
	});
	function reduce(){
		app.$emit('reduce');  //单独创建一个reduce方法，并且通过$emit()绑定创建好的reduce中
	};
</script>
```
在html中:
```html
<body>
	<h1>实例事件</h1>
	<hr>
	<div id="app">
		<p>{{message}}</p>
		<p><button @click="add">add</button></p>
	</div>
	<p><button onclick="reduce()">reduce</button></p> <!-- 这个按钮写在了作用域之外，绑定的方法是在script中声明好的函数 -->
</body>
```
2.$once() <br>
这个方法类似于on，但是只执行一次，也要配合$emit()函数来配合使用<br>
例，声明一个reduceOnce方法，只执行一次：<br>
在script中：
```javasctipt
<script>
	var app = new Vue({
		el:"#app",
		data:{
			message:1
		},
		methods:{
			add:function(){
				this.message++;
			}
		}
	});
	app.$on("reduce",function(){
		console.log("reduce已经调用");
		this.message--;
	});
	app.$once("reduceOnce",function(){
		console.log("reduceOnce已经调用,只调用一次");  //参数类型跟on相似，也是方法名字和函数具体内容
		this.message--;
	})
	function reduce(){
		app.$emit('reduce');
	};
	function reduceOnce(){
		app.$emit("reduceOnce");  //然后要单独声明reduceOnce()函数，并且配合$emit()函数来使用
	};
</script>
```
在html中：
```html
<body>
	<h1>实例事件</h1>
	<hr>
	<div id="app">
		<p>{{message}}</p>
		<p><button @click="add">add</button></p>
	</div>
	<p><button onclick="reduce()">reduce</button></p>
	<p><button onclick="reduceOnce()">reduceOnce</button></p>
</body>
```
点击reduceOnce按钮后会显示对应的内容<br>
3.$off() <br>
这个方法是用于关闭某个函数的功能， <br>
例，使用$off()来关闭reduce()的功能：<br>
在script中：
```javasctipt
<script>
	var app = new Vue({
		el:"#app",
		data:{
			message:1
		},
		methods:{
			add:function(){
				this.message++;
			}
		}
	});
	app.$on("reduce",function(){
		console.log("reduce已经调用");
		this.message--;
	});
	app.$once("reduceOnce",function(){
		console.log("reduceOnce已经调用,只调用一次");
		this.message--;
	})
	function reduce(){
		app.$emit('reduce');
	};
	function reduceOnce(){
		app.$emit("reduceOnce");
	}
	function off(){
		app.$off("reduce");
		console.log("已经关闭了reduce事件，已经不起任何作用了"); //注意格式
	}
</script>
```
在html中：<br>
```html
<body>
	<h1>实例事件</h1>
	<hr>
	<div id="app">
		<p>{{message}}</p>
		<p><button @click="add">add</button></p>
	</div>
	<p><button onclick="reduce()">reduce</button></p>
	<p><button onclick="reduceOnce()">reduceOnce</button></p>
	<p><button onclick="off()">off</button></p> //在外部创建了一个button绑定了off()事件
</body>
```
点击按钮后就会取消reduce按钮绑定的事件。<br>

### 4-4、内置组件slot
slot组件是用于标签的扩展，用于扩展模板组件的内容，使用slot可以给自定义组件传递内容，组件接受内容并输出<br>
例如我们首先定义一个自定义的标签tianer：<br>
```html
<body>
	<h1>slot content extend demo</h1>
	<hr>
	<div id="app">
		<tianer>
		</tianer>
	</div>
</body>

```
然后定义一个模板（template），使用在html中定义的方式：<br>
```html
<body>
	<h1>slot content extend demo</h1>
	<hr>
	<div id="app">
		<tianer>
		</tianer>
	</div>
</body>
<template id="abc">
	<div>
		<p>名字：</p>
		<p>博客地址：</p>
		<p>职位：</p>
	</div>
</template>
```
然后在script的Vue构造器中创建组件，绑定该模板内容，并且创建一个存有数据的angelData对象<br>
在script中：
```javascript
<script>
	var tianer = {
		template:"#abc"   //声明tianer变量，绑定到模板abc
	}
	var app = new Vue({
		el:"#app",
		data:{
			message:233,
			angelData:{
				myName:"天儿",
				myUrl:"https://github.com/Singleangel233",
				myWork:"web-front"
			}
		},
		components:{
			"tianer":tianer  //将tianer变量绑定到"tianer"字符串中，这样在组件里绑定即可用标签的形式使用
		}
	});
</script>
```
为了让angelData中的数据能够显示到模板中，则需要使用slot组件。<br>
使用的方式为：在tianer标签里创建好对应想显示的标签，然后绑定slot展现的内容，例：<br>
```html
<body>
	<h1>slot content extend demo</h1>
	<hr>
	<div id="app">
		<tianer>
			<span slot="myName">{{angelData.myName}}</span>
			<span slot="myUrl">{{angelData.myUrl}}</span>
			<span slot="myWork">{{angelData.myWork}}</span>
		</tianer>
	</div>
</body>
```
然后在模板中使用slot标签，然后让slot标签属性中name的属性值指向对应在tianer标签里绑定的slot内容。<br>
例：<br>
```html
<template id="abc">
	<div>
		<p>名字：<slot name="myName"></slot></p>
		<p>博客地址：<slot name="myUrl"></slot></p>
		<p>职位：<slot name="myWork"></slot></p>
	</div>
</template>
```
这时打开页面，模板就会展现对应的内容，也可以直接更改tianer标签中的span标签的内容。<br>
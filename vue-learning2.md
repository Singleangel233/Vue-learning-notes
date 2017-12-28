# Vue学习笔记2
## 1、Vue-directive自定义指令
Vue-directive:用于配合Vue中给的API定义自己想要的功能<br>
例：用Vue-directive自定一个改变颜色为红色的功能v-tianer(v-是命名的方式) <br>	
在html中：
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>practice</title>
</head>
<body>
	<div id="app">
		<div v-tianer="color">改变颜色</div>
		<p>{{message}}</p>
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
		message:'hellow world',
		color:'red'
	}
})
</script>
```
在script中，我们使用了Vue.directive方法来定义我们绑定的v-tianer，在function中的三个函数的意义分别为：
-el:指令所绑定的元素，用于直接操作DOM。
-binding:一个对象，包含很多指令信息（因为v-tianer绑定data中的color，而通过binding.value可以获取到data中的color的值）。
-v-node:Vue编译形成的一个虚拟节点。
最后页面中的改变颜色的颜色为color中的red红色。
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
在script中：
```
<script>
	Vue.directive('tianer',function(el,binding){
				el.style='color:'+binding.value;
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
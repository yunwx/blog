---
title: Vue学习笔记
date: 2025-02-04 15:47:22
tags: [Vue, JavaScript, Ajax, Axios]
categories: 前端
---

### 1.简介：

- Vue是渐进式JavaScript框架，基于标准HTML、CSS和JavaScript构建，并提供了一套声明式、组件化的编程模型。

### 2.准备：

- 引入Vue模块（官方提供）

``` vue
<script type="module">
    import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';
</script>
```

- 创建Vue程序的应用实例，控制视图元素

``` vue
<script type="module">
    import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';
    createApp({
    
    });
</script>
```

- 准备元素（div），被Vue控制（调用mount方法）
<div id="app">

</div>

``` vue
<script type="module">
    import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';
    createApp({
    
    }).mount("#app");
</script>
```

### 3.数据驱动视图：

- 准备数据
<div id="app">

</div>

``` vue
<script type="module">
    import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';
    createApp({
        data(){
            return{
                message: "Hello Vue"            
            }        
        }
    }).mount("#app");
</script>
```

- 通过插值表达式渲染页面

``` vue
<div id="app">
    <h1>{{message}}</h1>
</div>
<script type="module">
    import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';
    createApp({
        data(){
            return{
                message: "Hello Vue"            
            }        
        }
    }).mount("#app");
</script>
```

<span style="color:#FF6B6B">注：插值表达式不能出现在标签内部</span>

### 4.常用指令：

- v-for：列表渲染，遍历容器的元素或者对象的属性
	- 语法：<span style="color:#FF6B6B">v-for="(item,index) in items" :key="item.id"</span>

	``` vue
	<tr v-for="(item,index) in items" :key="item.id">{{item}}</tr>
	```

	- 参数说明：
		- items为遍历的数组
		- item为遍历出来的元素
		- index为索引/下标，从0开始；可以省略，省略index语法：v-for="item in items"
	- key：
		- 作用：给元素添加的唯一标识，便于vue进行列表项的正确排序复用，提升渲染性能
		- 推荐使用id作为key（唯一），不推荐使用index作为key（会变化，不对应）
		  
<span style="color:#FF6B6B">注：遍历的数组必须在data中定义；要想让哪个标签循环展示多次，就在哪个标签上使用 v-for 指令</span>
- v-bind：动态为HTML标签绑定属性值，如设置 href，css样式等
	- 语法：<span style="color:#FF6B6B">v-bind:属性名="属性值"</span>，可简化为：<span style="color:#FF6B6B">:属性名="属性值"</span>
	``` vue
	<img v-bind:src="item.image" width="30px">
	//简化
	<img :src="item.image" width="30px">
	```
- v-if / v-else-if / v-else：条件性的渲染某元素，判定为true时渲染，否则不渲染
	- 语法：<span style="color:#FF6B6B">v-if="表达式"</span>，表达式值为true，显示；false，隐藏
	``` vue
	<span v-if="gender==1">男生</span>
	<span v-else-if="gender==2">女生</span>
	<span v-else>未知</span>
	```

	- 原理：基于条件判断来控制创建或移除元素节点（条件渲染）
	- 场景：要么显示，要么不显示，不频繁切换的场景
	  
<span style="color:#FF6B6B">注：v-else-if 必须出现在 v-if 之后，可以出现多个；v-else 必须出现在 v-if / v-else-if 之后</span>
- v-show：根据条件展示某元素，区别在于切换的是display属性的值
	- 语法：<span style="color:#FF6B6B">v-show="表达式"</span>，表达式值为true，显示；false，隐藏
	``` vue
	<span v-show="gender==1">男生</span>
	```

	- 原理：基于CSS样式display来控制显示与隐藏
	- 场景：频繁切换显示隐藏的场景
- v-model：在表单元素上创建双向数据绑定，可以方便的获取或设置表单项数据
	- 语法：<span style="color:#FF6B6B">v-model="变量名"</span>
	``` vue
	<input type="text" id="name" v-model="searchForm.name">
	```

<span style="color:#FF6B6B">注：v-model中绑定的变量必须在data中定义</span>
- v-on：为HTML标签绑定事件（添加事件监听）
	- 语法：<span style="color:#FF6B6B">v-on:事件名="方法名"</span>，可简化为：<span style="color:#FF6B6B">@事件名="方法名"</span>
	``` vue
	<div id="app">
		<button type="button" v-on:click="handle">点我</button>
		<button type="button" @click="handle">再点我</button>
	</div>
	```

	- 在Vue中定义方法：
	``` vue
	const app = createApp({
		data(){
			//...    
		},
		methods:{
			handle(){
				console.log('试试就试试');                    
			}    
		}
	}).mount("#app")
	```

<span style="color:#FF6B6B">注：methods属性中的this指向Vue实例，可以通过this获取到data中定义的数据</span>

### 5.Ajax：

- 介绍：异步的JavaScript和XML
- XML：可扩展标记语言，本质是一种数据格式，可以用来存储复杂的数据结构
- 作用：
	- 数据交换：通过Ajax可以给服务器发送请求，并获取服务器响应的数据
	- 异步交互：可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术，如：搜索联想、用户名是否可用的校验等

### 6.Axios：

- 介绍：Axios对原生的Ajax进行了封装，简化书写，快速开发
- 步骤：
	- 引入Axios的js文件（参考官网）
	``` vue
	<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
	```

	- 使用Axios发送请求，并获取响应结果
		- method：请求方式，GET/POST
		- url：请求路径
		- data：请求数据（POST）
		- params：发送请求时携带的url参数，如：...?key=val
	``` vue
	axios({
		method: 'GET',
		url: 'https://web-server.itheima.net/emps/list'
	}).then((result) =>{ //成功回调函数
		console.log(result.data);
	}).catch((err) =>{ //失败回调函数
		alert(err);
	});
	```

- Axios-请求方式别名（推荐使用）：
	- 为了方便起见，Axios已经为所有支持的请求方法提供了别名
	- 格式：axios.请求方式(url [, data [, config]])
``` vue
//GET请求
axios.get('https://mock.apifox.cn/mq/3083103-0-default/emps/list').then((result) =>{
    console.log(result.data);
}).catch((err) =>{
    console.log(err);
});
//POST请求
axios.post('https://mock.apifox.cn/mq/3083103-0-default/emps/update','id=1').then((result) =>{
    console.log(result.data);
}).catch((err) =>{
    console.log(err);
});
```

### 7.async & await

- 作用：可以通过async、await可以让异步变为同步操作。async就是来声明一个异步方法，await是用来等待异步任务执行（提高代码可读性和可维护性）
- 语法：

``` vue
methods:{
    async search(){
        let result = await axios.get('https://web-server.itheima.net/emps/list?name=xxx&gender=xxx&job=xxx');
        this.employees = result.data.data;    
    }
}
```

<span style="color:#FF6B6B">注：await关键字只在async函数内有效，await关键字取代then函数，等待获取到请求成功的结果值</span>

### 8.Vue生命周期

- 生命周期：指一个对象从创建到销毁的整个过程
- 生命周期的八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法（钩子）
	- beforeCreate：创建前
	- created：创建后
	- beforeMount：载入前
	- mounted：挂载完成
	- beforeUpdate：数据更新前
	- updated：数据更新后
	- beforeUnmount：组件销毁前
	- unmounted：组件销毁后

{% asset_img 1.jpg %}

- 钩子函数使用：

``` vue
<script type="module">
    import { createApp } from 'https://.../vue.esm-browser.js'
    const app = createApp({
        data(){
            return {
                message:"Hello Vue"
            }        
        },
        //生命周期-钩子函数 mounted
        mounted(){
            console.log('Vue挂载完毕，发送请求获取数据...');        
        }    
    }).mount("#app");
</script>
```

<span style="color:#FF6B6B">注：钩子函数与 data 或 methods 是齐平的</span>
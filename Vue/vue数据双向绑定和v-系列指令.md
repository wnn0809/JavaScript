Vue.js是当下最火的一款前端框架了，学习的时候要多动手实践以帮助理解。我是通过例子来学习的，这样记的快一些。

目录：

 1. Vue.js介绍
 2. 如何引入Vue？
 3. 何为声明式渲染？如何实现？
 4. 文本插值{{message}}
 5. v-html
 7. v-bind：绑定元素属性
 8. v-model：双向绑定，仅用于表单元素
 9. v-on：事件绑定机制
 
## 1.什么是Vue.js？
 1. Vue.js官方网站的解释：

	Vue.js（/vjuː/） 是一套构建用户界面的渐进式框架。与其他重量级框架不同的是，Vue采用自底向上增量开发的设计。
	Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。
当与单文件组件和 Vue 生态系统支持的库结合使用时，Vue 也完全能够为复杂的单页应用程序提供驱动。
2. 分析Vue.js与DOM

	学习Vue.js时要抛开jQuery手动操作DOM的思维，因为Vue.js是数据驱动的，不需要手动操作DOM。它通过一些特殊的HTML语法，将DOM和数据绑定起来。一旦你创建了绑定，DOM将和数据保持同步，每当变更了数据，DOM也会相应地更新。
	
3. 核心

	Vue.js是一种MVVM框架（Model-View-ViewModel），其中html是view层，js是model层，ViewModel是Vue.js的核心。

	![在这里插入图片描述](https://img-blog.csdnimg.cn/20190123172514956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
	上图是实现数据双向绑定的示意图。
	
	创建了ViewModel实例后，双向绑定的过程分为：
	
	1.上图中的DOM Listeners和Data Bindings是实现双向绑定的关键。
	
	2.从View侧看，ViewModel中的DOM Listeners工具会帮我们监测页面上DOM元素的变化，如果有变化，则更改Model中的数据。
	
	3.从Model侧看，当我们更新Model中的数据时，Data Bindings工具会帮我们更新页面中的DOM元素。
	
## 2.引入Vue
- 方法一：独立下载

	在 Vue.js 的官网上直接下载 vue.min.js ，并用 `<script>` 标签引入。可放在html文件的`<head>`部分，也可放在`<body>`
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20190123184316631.png)
- 方法二：使用 CDN 方法

	不需要下载，直接通过以下方法在html中引入：

	```javascript
	<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
	```
	
## 3.声明式渲染
Vue.js 的核心是一个允许采用简洁的模板语法来声明式的将数据渲染进 DOM。

如下例子：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Vue.js的声明式渲染</title>
	<script src="js/vue.min.js"></script>
</head>
<body>
	<div id="myVue">
		<P>{{ Data }}</P>
	</div>

	<script type="text/javascript">
		var vue = new Vue({
			el: "#myVue",
			data: {
				Data: '我的第一个Vue应用'
			}
		})
	</script>
</body>
</html>
```

输出结果为：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190123192800951.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

分析：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190123193341727.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

通过Vue对象连接view和model，{{message}}就可以获取到Vue对象中data定义的message的内容。如此便完成了渲染。现在数据和 DOM 已经被建立了关联，所有东西都是响应式的。

## 4.文本插值{{message}}
Vue.js有多种数据绑定的语法，最基础的形式是文本插值，使用mustache语法，在运行时`{{ message }}`会被数据对象的message属性替换。所以下面代码页面上回输出'我的第一个Vue应用'。

```javascript
//这是定义视图View
<div id="myVue">
	<P>{{ message }}</P>
</div>

<script type="text/javascript">
	//用来连接MVVM中的view和model
	var vue = new Vue({//指向view，将Vue实例挂载
		el: "#myVue",
		data: {
			message: '我的第一个Vue应用'
		}//指向model
	})
```

上面便是文本插值实现数据绑定的用法。

关于mustache：

mustache.js是一个简单强大的Javascript模板引擎，使用它可以简化在js代码中的html编写，压缩后只有9KB，非常值得在项目中使用。

Mustache 的模板语法就下面几种：

```javascript
{{data}}
{{#data}} {{/data}}
{{^data}} {{/data}}
{{.}}
{{<partials}}
{{{data}}}
{{!comments}}
```

## 5.v-html指令
使用{{message}}的mustache语法只能将数据解释为纯文本，v-text指令也是纯文本。而v-html会被解析成html元素，所以为了输出HTML，可以使用v-html指令。

示例如下：

```javascript
<body>
	<div id="myVue">
		<P>{{ message }}</P>
		<P v-text="message"></P>
		<P v-html="message"></P>
	</div>

	<script src="js/vue.min.js"></script>
	<script type="text/javascript">
		var vue = new Vue({
			el: "#myVue",
			data: {
				message: '<h1>我的第一个Vue应用</h1>'
			}
		})
	</script>
<、body>
```

输出结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190123201503191.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

【注意】

使用v-html渲染数据会有危险，因为它很容易导致 XSS（跨站脚本） 攻击，使用的时候要小心谨慎，能够使用文本插值{{message}}或者v-text指令实现的不要使用v-html。

## 6.v-bind：绑定元素属性
使用v-bind指令可以绑定元素的属性。代码如下：

```javascript
<div id="myVue">
	<!-- v-bind指令将这个元素节点的title特性和Vue实例的msg属性保持一致 -->
	<span v-bind:title="msg">百度一下</span>
</div>

<script src="js/vue.min.js"></script>
<script type="text/javascript">
	var vue = new Vue({
		el:"#myVue",
		data:{
			msg:"www.baidu.com"
		}
	})
</script>
```

v-bind的缩写形式（直接简写为冒号：）：

例如：


```html
<span v-bind:title="msg">百度一下</span>
```

简写为：

```html
<span :title="msg">百度一下</span>
```

输出结果如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124201142618.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
## 7.v-model：双向绑定
MVVM模式支持双向绑定，可以使用v-model指令在表单元素上创建数据的双向绑定。用法如下：

```javascript
<body>
	<div id="myVue">
		<!-- v-model指令实现双向数据绑定 -->
		<h1>message is {{msg}}</h1>
		<input type="text" v-model="msg">
	</div>

	<script src="js/vue.min.js"></script>
	<script type="text/javascript">
		var vue = new Vue({
			el:"#myVue",
			data:{
				msg:""
			}
		})
	</script>
```

输出结果如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124202141270.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

当Vue实例中msg数据发生变化时，页面中也会自动刷新为相同的数据。如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124202347498.png)

## 8.v-on：事件绑定机制

v-on指令用于绑定事件。

```javascript
<!--这个div区域就是MVVM中的 View-->
 <div id="div1">
   <!-- 给button节点绑定按钮的点击事件 -->
   {{count}}
   <button v-on:click="changeNum">改变count的值</button>
 </div>


<script src="js/vue.min.js"></script>
<script type="text/javascript">
	//new出来的对象就是MVVM中的 View Module
  var myVue = new Vue({
    el: '#div1', //当前vue对象将接管上面的div区域
    data: { //data就是MVVM中的 module
      name: 'xiaohua'
    },

    //注意，下方这个 `methods` 是Vue中定义方法的关键字，不能改
    //这个 methods 属性中定义了当前Vue实例所有可用的方法
    methods: {
      changeNum: function() { //上面的button按钮的点击事件
        this.count+= '1';
      }
    }
  });
</script>
</script>
```

v-on的简写形式（直接使用@符号）：

例如：

```html
<button v-on:click="changeNum">改变count的值</button>
```

可以简写为：


```html
<button @click="changeNum">改变count的值</button>
```




## 每天进步一点点、充实一点点、加油！！！
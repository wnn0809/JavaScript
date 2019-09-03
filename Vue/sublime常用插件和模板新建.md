## 1.常用插件
列举sublime编辑器常用插件如下：

1. Emmet

	功能：编码快捷键，前端必备。

2. JSFormat

	功能：JavaScript的代码格式化插件。

3. LESS
	
	功能：LESS高亮插件。

4. Less2CSS

	功能：编译Less。

	简介：监测到文件改动时，编译保存为.css文件。

5. Git

	功能：Git管理。

6. Markdown Preview

	功能：md格式文件预览。
	
7. Markdown Editing
	
	功能：md格式文件编辑。

## 2. 模板生成
1.工具 —— 插件开发 —— 新建代码片段。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190903223719955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

然后会自动创建一个文件，填入你的代码模板 <![CDATA[

//这里填入你的代码模板

]]>

tabTrigger 标签中是自己定义的触发命令。

如下：

```javascript
<content><![CDATA[
<template>
	<div>
		
	</div>
</template>

<script>
	export default {
		data(){
			return{

			}
		}
	}
</script>

<style scoped>
	
</style>
]]></content>
	<tabTrigger>vue</tabTrigger>
```

然后Ctrl + S 保存文件，名称随意，但是文件后缀必须是 .sublime-snippet。如下就实现了输入vue快捷输出vue组件模板内容了。：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190903224555440.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

新建一个文件，输入命令 vue，按下Tab 键，刚刚设置的模板就自动生成了。
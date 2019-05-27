对变量提升、函数提升和js解析机制的理解。什么是变量提升呢，如果不能很好的理解变量提升，程序就容易出现一些问题。理解变量提升，也可帮助我们很好的弄懂js的解析机制。

## 1.什么是变量提升？

 1.  变量提升定义

	变量提升就是将变量声明提升到它所在作用域的最开始的部分。

2. 变量提升的理解
	
	理解一：该变量不管是在作用域的哪个地方声明的，都会提升到作作用域的最顶上去。

	理解二：变量可以先使用再声明。

	![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205202114239.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

3. 变量提升注意点

	1.同一个变量只会声明一次，其他的会被忽略掉。

	2.JavaScript 只有声明的变量会提升，变量初始化不会被提升。

	![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205203054677.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

	等价于：

	![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205203241617.png)

## 2.什么是函数提升？

1. 函数提升定义

	函数声明提前，函数声明式函数，函数的声明和定义会一起被提升到作用域的最上面部分。

2. 函数提升的理解

	理解一：调用的函数不管是在作用域的哪个地方声明的，都会提升到作作用域的最顶上去。

	理解二：函数可以先调用，再声明。也可以先声明函数，在调用函数。

	![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205203507450.png)

	等价于：

	![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205203654279.png)
	
3. 函数提升注意点

	- 使用函数表达式创建的函数，即匿名函数，例var myfun= function(){}，不会被声明提前，所以也不能在声明前调用。
	即使用匿名函数的方式不存在函数提升，因为函数名称使用变量表示的，只存在变量提升。

		![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205210047405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

		等价于：

		![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205210255736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

	- 函数声明的优先级高于变量声明的优先级，所以当函数和变量使用的是同一个变量名称时，函数声明先被提升，并且函数声明和函数定义的部分一起被提升。

		![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205205048203.png)

		等价于

		![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205205329888.png)

## 3.什么是js的解析机制？

在js中，代码的执行时分两步走的：解析 和 一步一步执行。

理解：

遇到 script 标签的话 js 就进行预解析，将变量 var 和函数 function 声明提升，但不会执行 function函数，然后就进入上下文执行，上下文执行还是执行预解析同样操作，直到没有 var 和 function，就开始执行上下文。

例如：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205211147399.png)

预解析：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181205211018839.png)


## 每天进步一点点，开心一点点，快乐一点点，come on！
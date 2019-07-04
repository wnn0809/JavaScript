更多内容[点这里](https://blog.csdn.net/w1418899532/article/details/84717091)

let 和 const是ES2015(ES6) 新增加的两个重要的 JavaScript 关键字。

## 1.let命令特点：

*1.ES6新增let命令。*

用来声明变量，用法类似var，但是它所声明的变量只在let命令所在的代码块内有效。使用方法如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181213090431188.png)

*2.不存在变量提升。*

let不像var那样，会发生"变量提升"现象。

通过如下程序分析：


![let不存在变量提升](https://img-blog.csdnimg.cn/2018120216040374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

上面ES5和ES6两种写法只有c是var声明的，d是let声明的。输出结果为：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202160547420.png)

问题：

`a[5]();`输出为9，`b[5]();`输出为5。为什么呢？为什么`a[5]();`输出不是理想中的5呢？

答案：

因为上述ES5写法循环结束的时候i的值已经为10，c=9，调用`a[5]();`时i是循环结束时的值，c=9，因此输出是9。

而d变量是由let声明，只在循环体内部起作用，在外部不起作用，影响不了外部的d，所以在调用第5个d时，值为5。

可以通过debug断点调试程序，帮助理解，循环结束d变为未定义，如下图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202163305193.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)



*3.暂时性死区。*

只要块级作用域内存在let命令，它所声明的变量就“绑定（binding）”这个区域，不再受外部的影响。

例如：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202164900252.png)

块级作用域内存在let命令，它所声明的变量就不受外界影响，如下情况，a输出仍然为100。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202165914842.png)


*4.不允许重复声明。*

let命令不允许在相同的作用域内，重复声明同一个变量。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202171018996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)


【注意】在 ES6 之前，JavaScript 只有两种作用域： **全局变量 与 函数内的局部变量**。

## 2.const命令

const和let一样具有只在块级作用域有效、不存在变量提升、暂时性死区、不可重复声明这四个特点。使用方法也和let命令相同。

- const定义常量和let定义的变量相同点：

    1.二者都是块级作用域。
    
    2.都不能和它所在作用域内的其他变量或函数拥有相同的名称。
    
    
- 两者不同点：

    1.const声明的常量必须初始化，而let声明的变量不用。
    
    2.const 定义常量的值不能通过再赋值修改，也不能再次声明。而 let 定义的变量值可以修改。
    
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202172736952.png)


- 特性：

    const 声明一个只读的常量，一旦声明，常量的值就不能改变。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202173052724.png)


- const声明对象

    const可以声明一个对象和数组。

    实例如下：

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202173428874.png)

- 对象的冻结

    1.冻结方法

    可以使用Object使const对象冻结。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202174614655.png)

    2.使用对象冻结


    在内部封装好对象，输出时就会有内容输出。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202175100469.png)
    

- const并非真正的常量

const 的本质:

 const 定义的变量并非常量，并非不可变，它定义了一个常量，引用一个值。使用 const 定义的对象或者数组，其实是可变的。
 
 
1.对象操作

 如下修改对象属性值和增加属性并不会报错：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202180005106.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
    
输出结果为：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202180044642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
    
***但是不能对常量对象重新赋值。！！！*** 如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202180427961.png)
    
2.数组操作

 如下修改数组元素和增加元素并不会报错：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202181440405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
    
输出结果为：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202181518369.png)

***但是不能对常量数组重新赋值。！！！*** 如下情况会报错。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181202181919109.png)


## 每天进步一点点、快乐充实一点点、开心一点点！加油！The Girl！
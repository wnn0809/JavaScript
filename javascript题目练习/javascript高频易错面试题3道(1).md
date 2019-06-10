更多内容[点这里](https://blog.csdn.net/w1418899532/article/details/91046507)
**前言**

以前刷过一遍html、css、JavaScript题目，错误率也很大，特别是JavaScript，如果不理解其考察的知识点精髓所在，遇到的错题再次遇到还会错，所以在这里将曾经做错的题目记录下来，并附上解析和答案，希望自己可以掌握这些题目考察的知识。

## 题目1
有如下代码，请问执行后输出的值是？

```javascript
var name = "World"
(function(){
	var name;
	if (typeof name==='undefined') {
		name = 'Jack';
		console.log('GoodBye'+name);
	}else{
		console.log('Hello'+name);
	}
})();
```
**答案解析：**

立即执行函数即自执行函数有自己的块级作用域，自执行函数里的var name，只是声明了，并没有被赋值，所以name等于undefined。

这道题目很容易被变量提升迷惑而做错。调试程序如下；

![调试](https://img-blog.csdnimg.cn/20190606170546311.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
综上分析可知输出答案是：GoodByeJack

如果自执行函数中没有`var name;`这句，答案输出HelloWorld。如下：

![题目1衍生](https://img-blog.csdnimg.cn/20190606171544202.png)

## 题目2
下面代码输出的结果是？

```javascript
var one;
var two=null;
console.log(one==two,one===two);
```

**答案解析：**

   `var one;` 仅声明未赋值，所以one变量的值是undefined。two的值是null。等于`==`符号是比较值，不比较类型，而且等号==会将两边的值作隐式转换，undefined和null转化为Boolean值后值都是false。严格等于`===`既比较值又比较类型，二者有一个不同，等号就不成立。严格等于符号也不会将两边的值作隐式转换。所以：
   
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190606173009604.png)

因此答案是：true false

## 题目3
下面代码，输出x的值是？

A： bar

B： 报错

C： foo

D： undefined


```javascript
function A() {
     this.do=function() {
         return 'foo'
     };
 }
 A.prototype=function() {
     return 'bar'
 };

var x = new A().do();
console.log(x);
```

可能有人向我一样，因为对原型的不理解，感觉在原型上的值都是正确的，就选了A，首先可以说这是错误的。


通过调试可知，首先执行 `new A()`，x是A的实例。然后执行`A.do()`。x先会在A函数中寻找do方法，此时，正好在A函数中找到了do方法，do方法返回`foo`。所以找到后会直接直接返回，因此不会再去A的原型上去找do方法，并且A的原型上也没有do方法，上面代码只是将A的原型重写成了一个函数。

综上：答案是C。

![题目3](https://img-blog.csdnimg.cn/20190609194700766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

另外可以通过下面方式在原型上定义方法：

方式1：

```javascript
A.prototype = new function() {
   this.do=function() {
        return 'bar'
    };
};
```

方式2：

```javascript
A.prototype.do = function() {
        return 'bar'
 };
```

此题采用方式1和方式2在A的原型上添加do方法后，把A中do重新改个名字，即A中不存在do方法时，便会去A的原型上查找do方法，此时x会输出bar，如下：

![题目3](https://img-blog.csdnimg.cn/2019060919563619.png)

关于原型和原型链请参考：[原型创建对象与原型链](https://blog.csdn.net/w1418899532/article/details/88723030)

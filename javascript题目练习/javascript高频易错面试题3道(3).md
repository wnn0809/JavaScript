更多内容[点这里](https://blog.csdn.net/w1418899532/article/details/91397361)

## 题目1

如下代码输出的结果是什么？

```javascript
console.log(1+"2"+"2");
console.log(1+ +"2"+"2");
console.log("A"-"B"+"2");
console.log("A"-"B"+2);
console.log(1+ -"1"+"2");
console.log(+1 +"1"+"2");
```

A 122 122 NaN NaN 112 112

B 122 32 NaN NaN2 02 22

C 122 32 NaN2 NaN  02 112  

D 122 32 NaN2 NaN2 02 112

这道题是百度的笔试题目，主要考察一元运算符加和减。这里先附上答案，然后再学习相关知识点。如下：

![题目一答案](https://img-blog.csdnimg.cn/20190611093137750.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

**知识点精讲**

1. 一元加法
    
    一元加法本质上对数字没有影响，即如果一元加法运算符后面本身跟着的就是一个数字型数据，那么得到的结果就是他本身，无论这个值是正数或负数，也不论这个值是整数、小数、科学计数表示。例如：

    

    ```javascript
    + 34        // 34
    + 3.4        // 3.4
    + 34e3        // 34000
    + 34e-3        // 0.034
    + -34        // -34
    ```
    
    
    但是对于字符串，它会把字符串转换成数字。例如：

    

    ```javascript
    console.log(typeof "3");     //string
    console.log(typeof +"3");     //number
    ```

    当一元加法运算符对字符串进行操作时，它计算字符串的方式与 parseInt() 相似，主要的不同是只有对以 "0x" 开头的字符串（表示十六进制数字），一元运算符才能把它转换成十进制的值。因此，用一元加法转换 "010"，得到的总是 10，而 "0xB" 将被转换成 11。

    对于运算结果不能转换为数字的，将会返回NaN。例如：

    

    ```javascript
    + 010        // 八进制转换成十进制  10
    + 0xB        // 十六进制转换成十进制 11
    + "abc"            // NaN
    ```
    
2. 一元减法

    一元减法就是对数值求负，与一元加法运算符相似，一元减法运算符也会把字符串转换成近似的数字，此外还会对该值求负。例如：

    
    
    ```javascript
    var num="20";
    console.log(-num);            //-20
    console.log(typeof -num);     //number
    console.log(-"3");            //-3
    console.log(typeof -"3");     //number
    ```
    在上面的代码中，一元减法运算符将把字符串 "-20" 转换成 -20。**另外一元减法运算符对十六进制和十进制的处理方式与一元加法运算符相似，只是它还会对该值求负。**

**3点主意事项**

1. 多个数字与数字字符串混合运算时，跟操作数(+-)的位置有关。操作数在数字字符串后面时直接进行拼接，在前面时，需要注意，视情况而定。如下：

    ```javascript
    console.log(2+1+    "3");  //33
    console.log("3"+2+1);      //321
    ```

2. 数字字符串之前存在数字中的正负号（+、-）时，会被转换成数字。
    
    ```javascript
    console.log(typeof "3");     //string
    console.log(typeof +"3");     //number
    console.log(typeof -"3");     //number
    ```

4. 对于运算结果不能转成数字的，将返回NaN.

    ```javascript
    console.log('a'*'sd')   //NaN
    console.log('A'-'B')    // NaN
    console.log('A'+'B')    // AB
    console.log(+'abc')    // NaN
    ```

## 题目2
下面代码程序的输出是什么？

```javascript
var myObject = {
    foo:"bar",
    func:function() {
        var self = this;
        console.log(this.foo);
        console.log(self.foo);
        (function(){
            console.log(this.foo);
            console.log(self.foo);
        }());
    }
};

myObject.func();
```
A bar bar bar bar

B bar bar bar undefined

C bar bar undefined bar

D undefined bar undefined bar 

这是百度公司前端工程师职位的笔试题目，主要考察立即执行匿名函数与this。

func是由myObject调用，所以this指向myObject。self是this的副本，这里this和self都指向对象myObject。因此第一个和第二个都输出`bar`。

而立即执行匿名函数的执行环境具有全局性，因此this对象通常指向window，是由window调用，使用apply和call的情况除外。所以第三个`this.foo`输出`undefined`。

第四个`self.foo`，因此这个IIFE立即执行函数的作用域处于myObject.func()的作用域中，IIFE所处的本作用域中找不到self变量，所以通过作用域链向上查找，从包含它的父函数中找到指向对象myObject的self，因此输出`bar`。

所以答案是C。运行如下：

![题目2答案](https://img-blog.csdnimg.cn/20190611104517791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

关于this的指向问题请参考[易错题(2)中的题目3](https://blog.csdn.net/w1418899532/article/details/91365180)

## 题目3
下列代码存在几个变量没有被回收？

```javascript
var i = 1;
var i = 2;
var add = function() {
    var i = 0;
    return function(){
        i++;
        console.log(i);
    }
}();
add();
```

A 0个

B 1个

C 2个

D 3个

这道题主要考察作用域与闭包，明白代码回收规则。

此题答案选D

1.js有两种作用域，即全局和局部。只要作用域的生命周期没有结束，那么里面的变量就没有被回收。变量的生命周期从声明开始，局部变量在函数执行完毕后被销毁，全局变量在页面关闭时被销毁。

因此全局变量i（第二个`var i = 2;`会覆盖`var i= 1;`，算是1个变量）和add 不会被回收。

2.闭包，闭包就是能够读取其他函数内部数据（变量/函数）的函数。如果作用域中的变量被闭包访问或者引用，那么这个变量就不会被回收。

因此闭包中i不会被回收。综上总共有3个变量不会被回收。

总结一句话：**全局变量和闭包中变量不会被回收，局部变量会被回收，就是函数一旦运行结束，函数内部的东西就会被销毁。**

详情参考[作用域和闭包](https://blog.csdn.net/w1418899532/article/details/84785314)


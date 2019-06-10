更多内容[点这里](https://blog.csdn.net/w1418899532/article/details/91365180)

## 题目1
现有如下HTML结构：

```html
<ul>
    <li>click me</li>
    <li>click me</li>
    <li>click me</li>
    <li>click me</li>
</ul>
```

运行如下代码：

```javascript
var elements = document.getElementsByTagName('li');
var length = elements.length;
for (var i = 0;i<length;i++ ) {
        elements[i].onclick=function(){
            alert(i);
        }
    }   
```

依次点击4个li标签，哪一个选项时正确的运行结果？

A：依次弹出1,2,3,4

B：依次弹出0,1,2,3

C：依次弹出3,3,3,3

D：依次弹出4,4,4,4

这题主要考察js的运行机制，因为js是单线程的，一个时间点只能做一件事，会先处理同步任务，遇到异步任务会先放到异步列表中，继续执行同步任务，等同步任务执行完毕，再去异步列表按顺序执行异步任务。（点击事件onclick、定时器setTimeout和setInterval、ajax都属于异步任务）

上面代码中for循环就是同步代码，onclick点击事件是异步任务，所以等for循环执行完了（此时i已经是4），才会去执行点击事件。这里又因为i是全局变量，最后一次i++使得i=4，onclick函数是在循环外面执行。for循环结束后，点击事件触发了4个onclick函数，依次输出4个4.因此答案是D。如下：

![题目1](https://img-blog.csdnimg.cn/2019061014533175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

## 题目2

```javascript
Number(null);
```
上面的代码将返回：

A Null

B 0

C undefined

D 1

这道题考察的是javascript的数据类型转换。经测试如下代码前面7个返回0：

```javascript
console.log(Number());//0
console.log(Number(0));//0
console.log(Number('0'));//0
console.log(Number(null));//0
console.log(Number(false));//0
console.log(Number([]));//0
console.log(Number([0]));//0
console.log(Number(''));//0
console.log(Number('false'));//NaN
console.log(Number(undefined));//NaN
```
经验证，该题选择B。

Number()转换规则：

1.如果是Boolern值，true和false分别被转换为1和0；

2.如果是null，转为0；

3.如果是undefined，转为NaN；

4.如果是数值，不需要转换，传什么就返回什么；

5.如果是字符串，则需要遵循下面规则转换：

&nbsp;&nbsp;5.1 字符串只把含数字（包含前面带正号+和负号-）的情况，将其转换为十进制数值，前面的0会被忽略。例如‘012’会变成12。

&nbsp;&nbsp;5.2 如果字符串中包含有效的浮点格式，会将其转换为相应的浮点数值，前面的0会被忽略。

&nbsp;&nbsp;5.3 如果字符串包含十六进制格式，则将其转换为相同大小的十进制整数。例如：number("01f")=16*1+15=31。

&nbsp;&nbsp;5.4 如果是空字符串，将其转换为0，例如Number('')=0。

&nbsp;&nbsp;5.5 如果字符串包含除以上格式之外的字符，则将其转换为NaN。例如：`console.log(Number('hello'));//NaN`

6.如果是对象，则调用对象的`valueOf()`方法，然后依照前面的规则转换返回的值，如果转换的结果是NaN，则调用对象的`toString()`方法，然后再一次按照前面的规则转换返回的字符串值。

valueOf() ：返回 Array 对象的原始值。

toString()：把数字转换为字符串

## 题目3

以下javascript代码执行后，浏览器alert出来的结果分别是？

```javascript
var color = 'green'
var test4399 = {
    color:'blue',
    getColor:function(){
        var color = 'red';
        alert(this.color);
    }
}

var getColor = test4399.getColor;
getColor();
test4399.getColor();
```

这道题是4399游戏的笔试题，主要考察this指向问题。

知识点1：this指向：在js中，this总是指向调用它的对象。

知识点2：js函数调用时加括号与不加括号区别：不加括号相当于把函数代码赋值给等号左边，加括号是把函数的返回值赋给等号左边。如：`var getColor = test4399.getColor;` test4399.getColor不加括号相当于把test4399对象中的getColor函数的函数代码赋值给等号左边的getColor 。

`getColor();` 就相当于普通函数的调用。此时的`this.color`中的this指向window，所以此时`this.color`的值应该是全局变量color的值，即`green`。

而`test4399.getColor();`此时的this指向调用函数的对象`test4399`，因此`this.color`值应该是对象的属性值`blue`。

所以这道题的答案是：green blue


**【题目延伸】**

延伸1：

如果去掉this，直接alert(color)，浏览器alert出来的`结果两次都是red`。此时是`getColor`函数内部color变量覆盖了全局和对象test4399中的color变量。

延伸2：

如果`getColor`函数内部定义color变量时，不适用`var`声明。即代码直接是`color = 'red';`，此时函数getColor中的color就变成了全局变量，会覆盖掉全局定义的`var color = 'green'`，此时浏览器alert出来的结果分别是：`red  blue` 。
更多内容[点这里](https://blog.csdn.net/w1418899532/article/details/91446177)

## <font color="red">题目1
getName七问七答，下面代码，输出结果分别是？

```javascript
function Foo(){
    getName=function(){
        console.log(1);
    }
    return this
}

Foo.getName=function(){
    console.log(2);
}

Foo.prototype.getName = function() {
    // body...
    console.log(3);
};

var getName = function(){
    console.log(4);
}

function getName(){
    console.log(5);
}

Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```

自我感觉这道题难度比较大，也许是我道行不行，反正是看了三遍，都没有全部作对。这是美团的一位大佬出的题啊。这道题考察的知识点包括`变量定义提升`、`this指针指向`、`运算符优先级`、`原型`、`继承`、`全局变量污染`、`对象属性及原型属性优先级`等。

作者也写了一篇博客详解此题目，比较长，[此题作者解析地址](https://www.cnblogs.com/xxcanghai/archive/2016/02/14/5189353.html)看完作者的解析后，我把我理解的记住的部分写下来，方便再次查看。详细知识点可以参考作者解析的。

1.首先这道题的答案是：2 4 1 1 2 3 3。

2.其次代码经过变量提升后为：

```javascript
function Foo(){
    getName=function(){
        console.log(1);
    }
    return this
}
//var getName，function getName()都是声明语句,
//变量和函数声明都会被提升到顶部
var getName;
function getName(){
    console.log(5);
}
//
Foo.getName=function(){
    console.log(2);
}
Foo.prototype.getName = function() {
    // body...
    console.log(3);
};
getName = function(){
    console.log(4);
}   
```
从变量提升后的代码可以知道第二问`getName();`函数的值最终是4，因为下面的getname会将提升上去的getname函数声明覆盖掉，所以答案不是5，而是4。

3.用一张图简单说明其余几问的答案。


![题目1解析](https://img-blog.csdnimg.cn/2019061117355729.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
第六问第七问作者都解释是运算符优先级的问题，第六问相当于`(new Foo()).getName()`，先执行了new Foo()；返回一个实例化对象。然后调用实例化对象的getName函数，因为在Foo构造函数中没有为实例化对象添加任何属性，遂到当前对象的原型对象（prototype）中寻找getName，找到了，所以会输出3.

最后一问与上面一问相同，最终实际执行为：`new ((new Foo()).getName)();`，先初始化Foo的实例化对象，然后将其原型上的getName函数作为构造函数再次new。返回的还是实例化对象本身。所以还是输出3.

最后两个问题我也不是特别明白。感觉使用优先级解释不通。如果你也看不到，可以去看看原作者的解析。[此题作者解析地址](https://www.cnblogs.com/xxcanghai/archive/2016/02/14/5189353.html)

## <font color="red">题目2
题目1太烧脑了，弄了一天，暂时歇歇，待续

如下代码，下面a,b,c的输出分别是什么？

```javascript
function fun(n,o) {
    console.log(o)
    return {
        fun:function(m){
            return fun(m,n);
    }
  };
}
var a = fun(0);  a.fun(1);  a.fun(2);  a.fun(3);
var b = fun(0).fun(1).fun(2).fun(3);
var c = fun(0).fun(1);  c.fun(2);  c.fun(3);
```

这是一道关于闭包的经典题目，可能大部分人都会做错，不止我自己。。。只能这样安慰自己了。答案如下:

![题目2-1](https://img-blog.csdnimg.cn/20190612172013912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

[闭包](https://blog.csdn.net/w1418899532/article/details/84785314)一文中闭包产生条件如下：
![闭包产生条件](https://img-blog.csdnimg.cn/20190612162024496.png)
上面的代码符合上面两个产生条件，可以判定为闭包。

**执行过程分析**

**重点需要知道：最内层的return出去的fun函数不是第二层fun函数，是最外层的fun函数。**

1. 第一行执行`a.fun(0)`时，等同于fun(0, undefined)，因为未传递第二参数，所以打印undefined；执行到`a.fun(1)`时，因为闭包的存在，所以n依然在内存中存在，如下图：

    ![题目2-1](https://img-blog.csdnimg.cn/20190612163149195.png)
执行到`a.fun(2)`时，相当于`fun(0,undefined).fun(2)`，m=2，所以执行`fun(m,n)`相当于执行`fun(2,0)`，结果打印0。

    执行到`a.fun(3)`时，相当于`fun(0,undefined).fun(3)`，m=3，所以执行`fun(m,n)`相当于执行`fun(3,0)`，结果打印0。

    综上第一行打印：`undefined，0，0，0`

2. 第二行`b = fun(0).fun(1).fun(2).fun(3)`是一直迭代， `fun(0).fun(1)`和上一行相同，分别执行`fun(0,undefined)`和`fun(1,0)`，打印：`undefined，0`。

     `fun(0).fun(1).fun(2)`相当于`fun(1,0).fun(2)`，执行最内层的fun(m,n)时，m=2，n=1（n是上步fun(1,0)执行后留在内存中的1），变为`fun(2,1)`，结果打印1。
     
     执行`b = fun(0).fun(1).fun(2).fun(3)`时，如下；
     ![题目2-2](https://img-blog.csdnimg.cn/20190612170819132.png)
    `b = fun(0).fun(1).fun(2).fun(3)`相当于`fun(2,1).fun(3)`，执行最内层的fun(m,n)时，m=3，n=2（n是上步fun(2,1)执行后留在内存中的2），变为`fun(3,2)`，结果打印2.。

    综上第二行结果输出： `undefined,0,1,2`

3. 第三行 `c = fun(0).fun(1);  c.fun(2);  c.fun(3)`是前两行的结合。如下解释：

    ![题目2-3](https://img-blog.csdnimg.cn/20190612171729981.png)
综上第三行输出：`undefined,0,1,1`

## <font color="red">题目3
下面代码依次输出什么？

```javascript
async function async1() {
    console.log("async1 start");
    await  async2();
    console.log("async1 end");
}

async  function async2() {
   console.log( 'async2');
}

console.log("script start");

setTimeout(function () {
    console.log("settimeout");
},0);

async1();

new Promise(function (resolve) {
    console.log("promise1");
    resolve();
}).then(function () {
    console.log("promise2");
});

console.log('script end');
```

这道题目主要考察javascript的执行机制和队列任务的优先级。答案如下：

![题目3答案](https://img-blog.csdnimg.cn/2019061217322941.png)

题目解析如下：

![题目3解析](https://img-blog.csdnimg.cn/2019061218525747.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

**知识点归纳**

1. 队列任务优先级：promise.Trick()>promise的回调>setTimeout>setImmediate。

2. javascript执行机制：先执行同步代码，遇到异步代码就先加入队列，然后按入队的顺序执行异步代码，最后执行setTimeout队列的代码。

3. await相关：

    1.await  操作符用于等待一个Promise 对象。它只能在异步函数 async function 中使用。

    2.await 表达式会暂停当前 async function 的执行，等待 Promise 处理完成。若 Promise 正常处理(fulfilled)，其回调的resolve函数参数作为 await 表达式的值，继续执行 async function。

    通俗说就是：await 表示等一下，代码就暂停到await 所在行这里，不再向下执行了，它等什么呢？等后面的promise对象执行完毕，然后拿到promise resolve 的值并进行返回，返回值拿到之后，它继续向下执行。

    3.若 Promise 处理异常(rejected)，await 表达式会把 Promise 的异常原因抛出。

    4.如果 await 操作符后的表达式的值不是一个 Promise，则返回该值本身。

4. async function 声明：用于定义一个返回 AsyncFunction 对象的异步函数。异步函数是指通过事件循环异步执行的函数，它会通过一个隐式的 Promise 返回其结果。


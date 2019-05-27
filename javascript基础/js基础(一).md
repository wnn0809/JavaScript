本篇文章早期更与我的[博客](https://blog.csdn.net/w1418899532)，更多内容详见我的博客。

## 1.web前端三层

 - w3c的规范：行内样式（淘汰）
 - 结构层：HTML  从语义角度描述页面的结构。
 - 样式层：CSS  从审美的角度美化页面。
 - 行为层：JavaScript  从交互的角度提升用户体验。
 
## 2.JavaScript书写与执行规则
 - 1.javascript与html，css一样，对空格、换行，以及缩进是不敏感的。
 - 2.如果代码末尾不加分号，必须要换行，如果不换行页面就会报错。
 - 3.每句JavaScript写完之后必须加分号；，必须换行。
 - 4.JavaScript代码从上到下依次执行。
 注意：
代码从上到下执行，但是遇到js语法错误，那么浏览器就不再执行里面的js代码。因为浏览器会先去检查js代码是否有错误，如果没有才会从上到下执行。

## 3.输出方法
document.write()：向页面中输出一句话

 - 1.alart()：在页面上打开一个弹出框，框上显示的内容就是alert()括号中的内容。
 特点：这个弹出框打开以后，页面就无法关闭。所以写代码时尽量不要使用。
 - 2.console.log(content)：向页面上输出一句话，不是在弹出框中输出。
 特点：不会让页面卡死，直接在控制台输出。
 - 3.prompt()：在页面上弹出一个输入框，输入框上面的提示文本就是prompt括号中的内容。
 -  prompt接收到的内容是string类型
 例如：
 

```html
<script>
        prompt("你知道我是谁吗？")
 </script>
```
![prompt实例](https://img-blog.csdnimg.cn/20181104212752807.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
## 4.调试js代码
 - 1.打开页面，右键检查元素。或者使用浏览器的开发人员工具。
 - 2.选择source。双击左侧01.html，出现js代码。
 - 3.若代码有错，右上方会有一个红色的×，打开console(控制台：检查页面输出的信息（自己输出的信息，以及页面的错误信息）)点击出现下图中3位置提示。
 - 4.在行号处鼠标点击，设置断点调试，下图中4位置。
 - 5.下图中5位置点击进入单步调试。
如图：
![debug](https://img-blog.csdnimg.cn/2018110421425296.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
代码修改后，F10一步一步调试。根据控制台的报错信息进行代码修改。
![debug](https://img-blog.csdnimg.cn/2018110421511681.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
## 5.js数据类型
在栈空间中开辟一块内存，将简单的数据类型存储到栈中。
1.简单数据类型
 - 1.string：字符串，一段文本
 特点：
1.0所有的字符串都是用””引起来的。
2.0可以当作直接量。
3.0字符串的引号可以是单引号’’也可以双引号””.
 - 2.number：数字。用来表示所有的数字。如果数字加上引号后就不是数字类型，而是string类型。
 - 3.Boolean：bool类型。用于判断对（true）和错（false），他的表达式有很多。
 - 4.undefined：未定义。一个变量声明了但没有赋值，就叫做undefined。

typeof：判断数据的类型。
判断方法：
1.直接将直接量放在typeof的后面，将来得到的结果会以字符串的形式返回回来。例如：
    typeof 直接量
2.将直接量放在typeof后面的括号中。
    typeof (直接量)

2.复杂数据类型
先在栈空间中开辟一块内存，将数据保存到堆空间中，然后将数据在堆空间中的存储地址放在栈里面去。
- （1） 数组：Array
    - 声明数组
    var arr = new Array();
    - 数组赋值
    arr[0] = 123;
    arr[1]=45.6;
    arr[2]=”abc”;
    arr[3]=hualala;
    - 数组的取值
    arr[下标];
    - 数组注意事项
    （1）数组的下标是以0开始。
    （2）数组声明以后，长度可以是无限长。
    （3）js中的数组可以存储任意的类型。
    - 数组的遍历
    arr.length;：是数组的一个属性。
- （2）对象：object 
    作用：可以用来存储数据。
    例如：保存小明的信息：年龄，姓名，性别，爱好。
由于通过变量来分别保存这些特征不太方便，所以用一个对象来表示小明。
    - 声明对象：
var xiaoming = new Object();
    - 给对象赋值：
xiaoming.age = 18;
xiaoming.name = xiaoming;
xiaoming.sex = “男”;
xiaoming.aihao = “女”;
    - 对象的使用
    如果要得到小明的爱好可以用：xiaoming.aihao
    直接通过xiaoming.aihao得到小明对应的爱好。
    ![堆栈](https://img-blog.csdnimg.cn/20181106205243242.png)
    
- （3）数组与对象的异同点
    1.相同点：
    都是电脑内存中的一部分。
    2.不同点：
    堆：存储空间大，运行速度慢。
    栈：存储空间小，运行速度快。



## 6.js变量
 - 作用：用来存储一些可以变化的数据。
 - 变量的声明：var 变量的名称；
例如：var sex；
![变量结构](https://img-blog.csdnimg.cn/20181105152029326.png)
 - 变量的赋值：变量名称=要赋值的内容；
例如：sex="女";
 - 变量的命名规范：
    1.变量名称的取值范围是：0-9,a-z,A-Z。
    2.数字不能作为变量名称的开始部分。
    3.变量的名称是区分大小写的。
    4.变量的名称不能是关键字和保留字（备胎）
 ![关键字和保留字](https://img-blog.csdnimg.cn/20181105152358657.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
## 7.运算符
 - 算数运算符：
内容：+（加）、-（减）、*（乘）、/（除）、%（取余）、()（提高优先级）、Math(平方，立方，4次方，sin,cos)。
1.0Math.pow(a,b)  ：//求a的b次方的值。
2.0Math.round(c)：  //将c以小数后面一位开始四舍五入。
3.0Math.ceil(d);：    //将d进行向上取整（天花板函数）
4.0Math.floor(e);：   //将e进行向下取整（地板函数）
5.0Math.max(a,b,c);：//在a,b,c中取得其中最大小的数据
6.0Math.min(a,b,c);：//在a,b,c中取得最小的数据
7.0Math.random();：//生成一个大于0小于1的随机数。
 - 逻辑运算符：
逻辑运算符一般跟boolean类型一起使用。
&& ：与（并且）
1 > 3 &&1 < 3
特点：一false都false
|| ：或 
1>3 || 1<3
特点：一true都true;
！：非（取反）
特点：取相反的值。
 - 赋值运算符
（=）：将=右边结果赋值给右边的变量。
var a=1；
var a=b+c;
var a=b=s=9;
 - 逗号运算符
声明多个变量时可以使用逗号将变量隔开。
例如：var a,d,f,r;
实际为 var a; var d; var f; var r;
 - 自增自减运算符
    自增：让数据在原来的基础上加上1.
    例：a = a + 1;可以简写成 a++(数据的自增)
    a++：特点：先运算，再自增
    ++a：特点：先自增，再运算
    自减：让数据在原来的基础上减1.
    例：b=b-1;可以简写成b--（数据的自减）
    b--:特点：先运算，再自减
    --b:特点：先自减，再运算
    【注意1】自增最多只能算两层。
    如：var a = 1;var b = a++ + ++a + a ++;（存在争议）
    【注意2】算术运算符有自己的优先级:先乘除再加减，
逻辑运算符也有自己的优先级：先算&&，再算||，如果有！先算！
 - 比较运算符
    大于（>）、小于（<）、大于等于（>=）、小于等于（<=）、等于( \==）、全等于（=\==）、不等于（！=）、全不等于（！\==）
    【注意1】\==在比较的时候比较的是内容，没有关注数据的类型
    如：
    
    var a = 3;
        var b = "3";
        var c = a == b ;
==>c =true;
    
    【注意2】===在比较的时候比较的是内容，还有类型
    如：

    var a = 3;
    var b = "3";
    var c = a===b;
    ==>c=false;
【注意3】!=比较的是内容
![！==实例](https://img-blog.csdnimg.cn/20181106133656457.png)
【注意4】！==比较的内容和类型
![！==实例](https://img-blog.csdnimg.cn/20181106133721209.png)
## 8.数据类型转换
1.显示转换

 - 1.1 转数字
    - a.Number转换
        var a = “123”;
        a = Number(a);

        （1）如果转换的内容本身就是一个数值类型的字符串，那么将来在转换的时候会返回自己。
        （2）如果转换的内容本身不是一个数值类型的字符串，那么在转换的时候结果是NaN.
        （3）如果要转换的内容是空的字符串，那以转换的结果是0.
        （4）如果是其它的字符，那么将来在转换的时候结果是NaN.
    - b.parseInt():转整数
    

        var a = “123”; 
        a = parseInt(a);
        
        （1）忽略字符串前面的空格，直至找到第一个非空字符,还会将数字后面的非 数字的字符串去掉。
    （2）如果第一个字符不是数字符号或者负号，返回NaN
    （3）会将小数取整。（向下取整）
    - parseFloat();浮点数（小数）
    与parseInt相同，区别是parseFloat可以保留小数。
- 转字符串
    作用：将其它的数据类型转成字符串。
    - String():
        
        var a = 123;
        a = String(a);

        
    - toString()
    toString()的方法来进行转换（包装类）。
        
        var a = 123;
         a = a.toString();

        
- 转boolean类型
    - Boolean()
    作用：可以将其它类型转为boolean值。
    

        var a =”true”; 
        a = Boolean(a);
        
        在进行boolean转换的时候所有的内容在转换以后结果都是true，除了：false、""（空字符串）、0、NaN、undefined
        
2.隐式转换
- 转number

    var a = “123”;
    a = +a;
    
    加（+）减（-）乘（*）除（/）以及求余（%）都可以让字符串隐式转换成number.
- 转string

    var a = 123;
    a = a + “”;
    
- 转boolean

    var a = 123;
    a = !!a;
    
## 9.流程控制（条件语句）
- 1.if else
    

    *if(判断条件/boolean值)｛
            //满足条件会执行下面的代码
            代码1;
        ｝else {
            //当上面的条件不满足，或者boolean的值为false的时候会执行下面的代码2
            代码2
        }*
        
    
    （1）if后面接有判断条件，else后面没有接判断条件
    （2）if和else只能执行一个。
    
- 2.if elseif else

    *if(判断条件/boolean值)｛
        //满足条件会执行下面的代码
        代码1;
    ｝else if(判断条件) {
        //当上面的条件不满足，或者boolean的值为false的时候会执行下面的代码2
        代码2
    }
    else if(判断条件){
    代码3
    }
    else {
        最后代码
    }*
    
    （1）在if_elseif_else结构中elseif可以有多个。
    （2）整个结构只会执行一个代码段。
    （3）条件在判断的是时候先写小范围的条件再写大范围的条件。
    （4）elseif后面要加判断条件。
    （5）一个if可以构成一个完整的结构。
    
- 3.switch_case
作用：用来判断多个可能出现的值。

    *switch(判断的值){
        case 具体值：
            要执行的代码段1;
            break;
        case 具体值2：
            要执行的代码段2;
            break;
        ......
        default:
            要执行的代码段n
            break;
            }*
            
    （1）case结构后面要跟一个具体的数值
    （2）case结构可以有无数个
    （3）如果所有的case都不满足，要执行default中的内容
    （4）defalut可以不写，并且defalut也不用写条件
    
## 10.三元运算符
作用：用于判断两个选择。

*boolean 表达式?代码段1：代码段2;*

执行过程：
判断boolean表达式是否成立，如果成立会执行代码段1，如果不成立会执行代码段2。
## 11.js循环
- 1.while循环
    作用：反复执行一段代码。

    *while(判断条件/boolean){
            代码块
        }*

    while循环的执行流程：
    当代码执行到while的时候，会先判断判断条件是否为true，如果为true，那么会执行while大括号中的代码块，代码块执行完毕以后，再次回到while中再进行判断，如果为true,再次执行while大括号中的代码块，并且再次回while，如果为false就不执行。
    【注意】
    （1）将来在写代码的时候一定要注意循环的判断条件不能一直为true,会成为一个死循环。
    （2）循环的条件和循环体一定要明确。
    （3）将来在实际开发中我们一般不会使用while循环，会使用for循环。
- break
    作用：在循环内部结束这个循环。
    用法：
    ![break用法](https://img-blog.csdnimg.cn/20181106203657148.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
- continue
    作用：在循环的内部结束本次循环，开始下一次循环。
    用法：
    ![continue用法](https://img-blog.csdnimg.cn/20181106203838257.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
- do while
    与while是一样的，唯一的区别就是while先判断再执行，do-while，先执行再判断。
    语法：
    

    *do{
        //要循环执行的代码块
    }while (条件语句/boolean)*
    
    
    执行过程：
    代码从上到下执行的过程中如果遇到了do就会先执行一次do后面的代码，执行之后再通过while来进行判断，如果判断通过那么再执行一次，如果判断不通过却结束循环。
- for循环
    作用：反复执行同一段代码。
    用法：
    

    *for(var i = 1; 判断条件; i++){
        要循环的代码块：
    }*
    
    执行步骤：
    当程序运行到for的时候，会先声明一个变量i，并且赋值为1，判断i是否满足后面的判断条件，如果满足，执行下面的要循环的代码块，代码 块执行完成之后再执行i++,再判断判断条件是否满足，如果满足再次按照上面的流程执行，如果不满足，直接结束for循环。
    
## 12，js中的方法（函数）
封装函数：将一段经常使用的代码用一个方法包起来，方便再次调用。
定义：

*function  方法名() {
            代码段。
}*

用法：
    方法名()
    
哈哈：每天进步一点点、快乐一点点、开心一点点！
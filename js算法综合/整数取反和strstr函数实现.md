文章首先更新[在这里](https://blog.csdn.net/w1418899532/article/details/95017905)

## 1.整数反转

此题目是LeetCode第7题，使用两种javascript方法求解。

1. 题目描述

	

	```javascript
	/*给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
	
   示例 1:

   输入: 123
   输出: 321
    示例 2:

   输入: -123
   输出: -321
   示例 3:

   输入: 120
   输出: 21
   注意:

   假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
   */
	```

2. 答案解析

	**方法1**
	
	1.先将整数num转成字符串后使用split()方法进行分割。

	2.然后使用reverse()方法将分割后的字符反转，再使用join()方法组合(分正数和负数两种情况)。

	3.使用Number对象将结果转成数字。

	·4.判断结果有没有超出数值范围，超出则输出0。

	```javascript
	function Reverse(num){
	     var result
	     if(num>0){
	         result= Number(String(num).split('').reverse().join('')) 
	     }else{
	         result = -Number(String(Math.abs(num)).split('').reverse().join(''))
	     }
	     // 判断数值范围
	     if(result<-Math.pow(2,31)||result>Math.pow(2,31)-1){
	         result = 0
	     }
	     return result
	 }
	 console.log(Reverse(Math.pow(2,31)))
	```

	测试结果：

	num：1200，result：21

	num：1234，result：4321

	num：-1234，result：-4321

	num：Math.pow(2,31)，result：0

	**方法2**
	
	使用数学方法反转数字，我们知道一个数字可以得到这个数字的个位数。其商乘以10再加上余数也可以得到这个数字。例如：1234多次对10求余，得到的余数分别是4、3、2、1。得到的商分别是123、12、1、0。`1234=(((0*10+4)*10+3)*10+2)*10+1`，分析可知可以使用递归方法。

	代码实现如下：

	```javascript
	var Reverse2 = function(num){
	    var result = 0
	    var x = Math.abs(num)
	    while(x!=0){
	        result = result*10 + x%10
	        // 向下取整
	        x = Math.floor(x/10) 
	        // console.log(result)
	        if(result<-Math.pow(2,31)||result>Math.pow(2,31)-1){
	            result = 0
	        }
	    }
	    
	    return (num>0)?result:-result
	}
	console.log(Reverse2(-1234))
	```

	测试结果：

	num：1200，result：21

	num：1234，result：4321

	num：-1234，result：-4321

	num：Math.pow(2,31)，result：0

## 2.实现strStr函数
此题目出自LeetCode第28题。

1. 题目描述
	
		

	```javascript
	/*
    实现 strStr() 函数。

    给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

    示例 1:

    输入: haystack = "hello", needle = "ll"
    输出: 2
    示例 2:

    输入: haystack = "aaaaa", needle = "bba"
    输出: -1
    说明:

    当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

    对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。
	*/
	```

2. 答案解析

	此题第一次解我使用了find方法，是受python语法的影响，在python中 `find(`) 方法检测字符串中是否包含子字符串 str ，如果包含子字符串返回开始的索引值，否则返回-1。

	但是javascript中find方法功能不同，于是找到了对应的`indexOf()`方法，该方法从头到尾地检索字符串 `stringObject`，看它是否含有子串 `searchvalue`。开始检索的位置在字符串的 `fromindex` 处或字符串的开头（没有指定 fromindex 时）。如果找到一个 `searchvalue`，则返回 `searchvalue` 的第一次出现的位置。`stringObject` 中的字符位置是从 0 开始的。下面介绍javascript中这两种方法。

	
	|方法名|用途  |语法|
	|--|--|--|
	| find() | 返回通过测试（函数内判断）的数组的第一个元素的值 |array.find(function(currentValue, index, arr),thisValue) |
	| indexOf()|返回某个指定的字符串值在字符串中首次出现的位置 ，如果没有出现，返回-1。**indexOf() 方法对大小写敏感**！|stringObject.indexOf(searchvalue,fromindex) |
	
		
	
	```javascript
	function strstr(haystack,needle){
	   return haystack.indexOf(needle)
	}
	console.log(strstr('hello','ll'))
	```

	测试结果：
	
	haystack："hello"，needle："ll"   `===>` 返回 2

	haystack："hello"，needle：" "   `===>` 返回 0

	haystack："aaaaa"，needle："bba"   `===>` 返回 -1


## 风只走了八百里，而我将走一千五百多公里。
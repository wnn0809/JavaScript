文章首先更新[在这里](https://blog.csdn.net/w1418899532/article/details/90065633)


今天看到一知名公司的笔试题目，于是尝试解答。

题目如下：

使用js方法把json数据转换成树形结构。

思路如下：

json数据转换成树形结构，无非就是便于渲染，将一个平铺的 ‘树’ 结构转换成一个真正的 ‘树’ 结构，就是如下效果：

## 1.树形结构效果
json格式数据：

```javascript
[
    {id: 7,  name: '快鱼',  pid: 2},
    {id: 8,  name: '海澜之家',  pid: 2},
    {id: 9,  name: '森马',  pid: 2},
    {id: 13,  name: '华为mate10',  pid: 4},
    {id: 14,  name: '华为mate20',  pid: 4},
    {id: 15,  name: '华为mate30',  pid: 4},
    {id: 4,  name: '华为',  pid: 1},
    {id: 5,  name: 'ViVo',  pid: 1},
    {id: 6,  name: 'OPPO',  pid: 1},
    {id: 10,  name: '长虹',  pid: 3},
    {id: 11, name: '飞利浦',  pid: 3},
    {id: 12, name: '松下',  pid: 3},
    {id: 1,  name: '手机'},
    {id: 2,  name: '服装'},
    {id: 3,  name: '家电'}    
]
```

转成树形结构后：

```javascript
[
     {id: 1,  name: '手机', pid: 0, children: [
         {id: 4,  name: '华为',  pid: 1, children: [
             {id: 13,  name: '华为mate10',  pid: 4},
             {id: 14,  name: '华为mate20',  pid: 4},
             {id: 15,  name: '华为mate30',   pid: 4}
         ]},
         {id: 5,  name: 'ViVo',  pid: 1, children: []},
         {id: 6,  name: 'OPPO',  pid: 1, children: []}
     ]},
     {id: 2,  name: '服装', pid: 0, children: [
         {id: 7,  name: '快鱼',  pid: 2, children: []},
         {id: 8,  name: '海澜之家',  pid: 2, children: []},
         {id: 9,  name: '森马',  pid: 2, children: []}
     ]},
     {id: 3,  name: '家电', pid: 0, children: [
         {id: 10, name: '长虹',  pid: 3, children: []},
         {id: 11, name: '飞利浦',  pid: 3, children: []},
         {id: 12, name: '松下',  pid: 3, children: []}
     ]}    
   ]
```

pid就是父亲id：parentID，如pid的值是3，那么json结构中id值为3的就是其父id。

## 2.实现思路

1. 将原json数据按照id值排序。目的是根据pid值查找父元素时，可以直接快速定位，不需要再循环查找。实现方法是新建临时变量temp，对原json数据使用for循环赋值。如下：

	![在这里插入图片描述](https://img-blog.csdnimg.cn/20190510161926604.png)
temp结果为：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190510162208460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

2. 遍历json数据中的每一个元素，然后去临时数组temp中找对应的父元素，如果找到，就填到这个父元素的children里。如果找不到对应的父元素(一般是无pid属性)，说明它就是一级元素。如下：

	![在这里插入图片描述](https://img-blog.csdnimg.cn/20190510163111590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
3. 最后将所有的一级元素push到新建的result(存储json数据转为树形结构后的结果的数组)中。如上图。

## 3.代码实现

```javascript
<script type="text/javascript">
		var _JSON_ = [
	       {id: 7,  name: '快鱼',  pid: 2},
	       {id: 8,  name: '海澜之家',  pid: 2},
	       {id: 9,  name: '森马',  pid: 2},
	       {id: 13,  name: '华为mate10',  pid: 4},
	       {id: 14,  name: '华为mate20',  pid: 4},
	       {id: 15,  name: '华为mate30',  pid: 4},
	       {id: 4,  name: '华为',  pid: 1},
	       {id: 5,  name: 'ViVo',  pid: 1},
	       {id: 6,  name: 'OPPO',  pid: 1},
	       {id: 10,  name: '长虹',  pid: 3},
	       {id: 11, name: '飞利浦',  pid: 3},
	       {id: 12, name: '松下',  pid: 3},
	       {id: 1,  name: '手机'},
	       {id: 2,  name: '服装'},
	       {id: 3,  name: '家电'}    
	   ];

	   function trans_tree(jsonData){
	   		//result存储json数据转为树形结构后的结果。
	   		//temp为临时对象，将json数据按照id值排序.
	   		//len是json长度，用于循环遍历结束的条件
		    var result = [], temp = {}, len = jsonData.length
		    for(var i = 0; i < len; i++){
		    	// 以id作为索引存储元素，可以无需遍历直接快速定位元素
		        temp[jsonData[i]['id']] = jsonData[i] 
		    }
		    for(var j = 0; j < len; j++){
		        var currentElement = jsonData[j]
		        // 临时变量里面的当前元素的父元素，即pid的值，与找对应id值
		        var tempCurrentElementParent = temp[currentElement['pid']] 
		        // 如果存在父元素，即如果有pid属性
		        if (tempCurrentElementParent) { 
		        	// 如果父元素没有chindren键
		          if (!tempCurrentElementParent['children']) { 
		          	// 设上父元素的children键
		            tempCurrentElementParent['children'] = [] 
		          }
		          // 给父元素加上当前元素作为子元素
		          tempCurrentElementParent['children'].push(currentElement) 
		        } 
		        // 不存在父元素，意味着当前元素是一级元素
		        else { 
		            result.push(currentElement);
		        }
		    }
		    return result;
		}

		console.log(trans_tree(_JSON_))
```

输出结果为：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190510161317930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)


## 每天进步一点点、充实一点点！！！！
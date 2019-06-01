更多内容[点这里](https://blog.csdn.net/w1418899532/article/details/90515969)

<font  color="orange">有时候一个网页会包含很多的图片，例如淘宝京东这些购物网站，商品图片多只之又多，页面图片多，加载的图片就多。服务器压力就会很大。不仅影响渲染速度还会浪费带宽。比如一个1M大小的图片，并发情况下，达到1000并发，即同时有1000个人访问，就会产生1个G的带宽。

<font  color="orange">为了解决以上问题，提高用户体验，就出现了懒加载方式来减轻服务器的压力，优先加载可视区域的内容，其他部分等进入了可视区域再加载，从而提高性能。


<font  color="orange">vue项目中的打包，是把html、css、js进行打包，还有图片压缩。但是打包时把css和js都分成了几部分，这样就不至于一个css和就是文件非常大。也是优化性能的一种方式。

效果动图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190524171929234.gif)


<font  color="green">进入正题------懒加载

## <font  color="red">1.懒加载原理

<font  color="green">一张图片就是一个`<img>`标签，浏览器是否发起请求图片是根据`<img>`的src属性，所以实现懒加载的关键就是，在图片没有进入可视区域时，先不给`<img>`的src赋值，这样浏览器就不会发送请求了，等到图片进入可视区域再给src赋值。

## <font  color="red">2.懒加载思路及实现

<font  color="green">实现懒加载有四个步骤，如下：

1.加载loading图片

2.判断哪些图片要加载【重点】

3.隐形加载图片

4.替换真图片

<font  color="green">1.加载loading图片是在html部分就实现的，代码如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190524163828967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

2.如何判断图片进入可视区域是关键。

引用网友的一张图，可以很清楚的看出可视区域。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190524164645631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

如上图所示，让在浏览器可视区域的图片显示，可视区域外的不显示，**所以当图片距离顶部的距离top-height等于可视区域h和滚动区域高度s之和时说明图片马上就要进入可视区了，就是说当top-height<=s+h时，图片在可视区。**

这里介绍下几个API函数：

<font  color="red">页可见区域宽： document.body.clientWidth;

网页可见区域高： document.body.clientHeight;

网页可见区域宽： document.body.offsetWidth (包括边线的宽);

网页可见区域高： document.body.offsetHeight (包括边线的宽);

网页正文全文宽： document.body.scrollWidth;

网页正文全文高： document.body.scrollHeight;

<font  color="blue">网页被卷去的高： document.body.scrollTop;

<font  color="red">网页被卷去的左： document.body.scrollLeft;

网页正文部分上： window.screenTop;

网页正文部分左： window.screenLeft;

屏幕分辨率的高： window.screen.height;

屏幕分辨率的宽： window.screen.width;

屏幕可用工作区高度： window.screen.availHeight;


<font  color="blue">HTMLElement.offsetTop 为只读属性，它返回当前元素相对于其 offsetParent 元素的顶部的距离。

window.innerHeight：浏览器窗口的视口（viewport）高度（以像素为单位）；如果有水平滚动条，也包括滚动条高度。

具体实现的js代码为：

```javascript
// onload是等所有的资源文件加载完毕以后再绑定事件
window.onload = function(){
    // 获取图片列表，即img标签列表
    var imgs = document.querySelectorAll('img');

    // 获取到浏览器顶部的距离
    function getTop(e){
        return e.offsetTop;
    }

    // 懒加载实现
    function lazyload(imgs){
        // 可视区域高度
        var h = window.innerHeight;
        //滚动区域高度
        var s = document.documentElement.scrollTop || document.body.scrollTop;
        for(var i=0;i<imgs.length;i++){
            //图片距离顶部的距离大于可视区域和滚动区域之和时懒加载
            if ((h+s)>getTop(imgs[i])) {
                // 真实情况是页面开始有2秒空白，所以使用setTimeout定时2s
                (function(i){
                    setTimeout(function(){
                        // 不加立即执行函数i会等于9
                        // 隐形加载图片或其他资源，
                        //创建一个临时图片，这个图片在内存中不会到页面上去。实现隐形加载
                        var temp = new Image();
                        temp.src = imgs[i].getAttribute('data-src');//只会请求一次
                        // onload判断图片加载完毕，真是图片加载完毕，再赋值给dom节点
                        temp.onload = function(){
                            // 获取自定义属性data-src，用真图片替换假图片
                            imgs[i].src = imgs[i].getAttribute('data-src')
                        }
                    },2000)
                })(i)
            }
        }
    }
    lazyload(imgs);

    // 滚屏函数
    window.onscroll =function(){
        lazyload(imgs);
    }
}
```

效果如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190524171459728.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

随着鼠标向下滚动，其余图片也逐渐显示并发起请求。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190524171649103.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

效果动图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190524171800631.gif)



## <font  color="red">每天进步一点点、充实一点点、加油！Girl！


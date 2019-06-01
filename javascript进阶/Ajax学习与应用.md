更多内容[点这里](https://blog.csdn.net/w1418899532/article/details/89351558)

看论坛帖子，初次测试ajax功能时大多会遇到Ajax跨域问题导致的无法正常实现ajax通信。

## 1.问题呈现

报错如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041710524416.png)

原因：

报次错原因就是由于ajax涉及到跨域问题导致的。没有在服务器环境里运行含有ajax方法的页面，而是直接通过浏览器打开文件。（类似file:///的访问形式，即file协议） 。

解决策略：

ajax()请求本地页面，须通过服务器环境运行。

## 2.一次成功的本地测试ajax通信

1. WampServer介绍

    要在本地测试AJAX，首先是环境的搭建，我在本机安装的是WampServer。
    
    WampServer是一款由法国人开发的Apache Web服务器、PHP解释器以及MySQL数据库的整合软件包。免去了开发人员将时间花费在繁琐的配置环境过程，从而腾出更多精力去做开发。
    
    WampServer就是Windows Apache Mysql PHP集成安装环境，即在window下的apache、php和mysql的服务器软件。

2. 下载安装wamp

    下载网址：[wamp下载](https://sourceforge.net/projects/wampserver/)https://sourceforge.net/projects/wampserver/

    安装：傻瓜式安装，一路点击下一步，安装完成后，双击桌面上wamp的快捷方式启动wamp。
    
    安装过程会提示在哪个浏览器中使用，默认是本机的IE浏览器，如果想使用chrome，选择chrome.exe的路径即可。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417130107696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

    问题：安装完成wamp后，双击桌面上的图标，如果提示丢失MSVCR110.DLL缺失，如下错误：

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417130220577.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

    **上述问题解决方法：**

    下载Visual C++ Redistributable for Visual Studio 2012 Update 4即可解决。

    下载网址：https://www.microsoft.com/zh-CN/download/details.aspx?id=30679

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417135826707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

    安装完成再次打开wamp，右下角图标已经由橘色变为绿色，PHP环境也正常。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417135938830.png)

    访问apache，显示界面就成功了。

    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417140022941.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

    在wamp安装目录下有一个www文件夹，用来测试的文件都保存在这个文件夹中

3. ajax通信代码编写

这个例子要创建两个页面。一个是前台的html页面ajax-get.html，用来发送请求和显示服务器返回来的响应结果，一个是后台的php文件，我命名为ajax.php，用来接收请求。

 **发送ajax请求五步骤（XMLHttpRequest 的原理）**

（1）创建XMLHttpRequest 对象。

（2）使用open方法设置请求的参数。open(method, url, 是否异步)。

（3）发送请求send()。

（4）注册事件。 注册onreadystatechange事件，状态改变时就会调用。

（5）获取返回的数据，更新UI。
    
html页面ajax-get.html的代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ajax发送get请求</title>
</head>
<body>
    <h1>Ajax发送get请求</h1>
    <input type="button" value="发送getAjax请求" id="btnAjax">

    <script type="text/javascript">
        //绑定点击事件
        document.querySelector('#btnAjax').onclick=function(){
            //发送ajax请求五步骤
            //1.创建异步对象
            if (window.XMLHttpRequest) {
                var ajaxObj = new XMLHttpRequest();
            }else{
                //用来兼容以前的IE浏览器
                var ajaxObj = new ActiveXObject('Microsoft.XMLHTTP');
            }
            

            //2.设置请求参数，方法和URL
            ajaxObj.open('get','ajax.php');

            //3.发送请求
            ajaxObj.send();

            //4.注册事件
            ajaxObj.onreadystatechange=function(){
                //
                if (ajaxObj.readyState ==4 && ajaxObj.status == 200) {
                    //5.在注册事件中获取返回的内容并修改页面显示内容
                    console.log('数据返回成功');

                    // 数据是保存在 异步对象的 属性中
                    console.log(ajaxObj.responseText);
                    
                    //修改页面的显示
                    document.querySelector('h1').innerHTML = ajaxObj.responseText;
                }
            }
        }
    </script>
</body>
</html>
```
ajax.php文件代码：
    

```php
<?php
    //echo的数据是返回给发送请求的页面
    echo 'smile';
    echo '使用ajax成功了一次';
 ?>
```

到这里简单的代码就写完了，把ajax-get.html和ajax.php保存在wamp安装目录下www文件夹下相同的目录下，我的是都放在F:\wamp\www\mytest目录下。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417142215801.png)

**然后通过本地服务器打开ajax-get.html文件**。比如我的是http://localhost/mytest/ajax-get.html。打开页面如下，当点击按钮时成功实现通信。

![前后台通信](https://img-blog.csdnimg.cn/20190417142749461.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

上面是使用get方法实现的通信，使用post方法类似。ajax-post.html代码如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417151043720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

ajaxpost.php代码如下：

```php
<?php
    //echo的数据是返回给发送请求的页面
    echo '使用post方法传输数据成功';
 ?>
```

运行结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417151223162.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417151533710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417151447533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)

## 3.onreadystatechange 事件

1. 注册 onreadystatechange 事件后，每当 readyState 属性改变时，就会调用onreadystatechange 函数。

2. readyState：（存有 XMLHttpRequest 的状态。从 0 到 4 发生变化）

    0: 请求未初始化
    
    1: 服务器连接已建立
    
    2: 请求已接收
    
    3: 请求处理中
    
    4: 请求已完成，且响应已就绪
    


## 每天进步一点点、充实一点点、开心一点点、加油哦！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417143812255.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3cxNDE4ODk5NTMy,size_16,color_FFFFFF,t_70)
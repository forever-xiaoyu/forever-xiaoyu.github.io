---
title: 使用原生 ajax 实现加载更多
date: 2017-5-2 15:53:56
tags: JavaScript
categories: JavaScript
---

使用原生ajax实现加载更多的功能，点击加载更多，按钮变为正在加载中，待内容全部加载完毕后，按钮变回加载更多，若觉得数据加载过快导致无法体现加载中这一项，可以通过设置一个延迟实现。

<!-- more -->

如何判断数据全部获取完成，举个简单的例子，比如每次从数据库中读取10条数据。先在JavaScript中定义一个变量
```
     var loadNum = 2 ;
```
用来记录点击次数（每次自增1），然后将这个变量通过PHP接收，通过
```
     $num = ($loadNum -1) * 10 ;
```
就可以判断从数据库中那一条开始取值了，只要$num小于数据总数目，就继续读取；这时，会有一个问题，如果不足10条数据的话怎么办？答案很简单，如果总数目-$num小于10条，那么只需要计算还有几条数据再读取就可以了；如果 $num - 总数目小于等于10条，那么可以判断已经没有数据可以读取到了，输出没有更多了即可。如果 $num - 总数目大于10条，那就说明已经超过有效的页数了，由于已经输出过没有更多了，不需要再进行输出，直接retuan即可。

再简单回顾一下ajax的请求过程：

 - 创建异步对象XMLHttpRequest。
 - 设置请求参数（请求方式，请求资源的相对路径，是否异步）。
 - 设置回调函数，用来处理服务器响应，使用onreadystatechange。
 - 获取异步对象的readyState属性：该属性存有服务器相应的状态信息。
		0 请求未初始化之前（在调用open（）之前）
		1 请求已提出（调用send（）之前）
		2 请求已发送（这里通常可以从响应得到内容头部）
		3 请求处理中（响应中由部分数据可用，但是服务器还没有完成响应）
		4 请求已完成（可以访问服务器响应并使用它）

 - 判断响应报文的状态，若为200说明服务器正常运行并返回响应数据。
 - 读取响应数据，可以通过responseText属性来取回有服务器返回的数据。
 
```
//最后把代码封装起来,封装起来以后，要给这个函数加上一个参数url
function ajax(url,fnSucc)
{
    if(window.XMLHttpRequest)
        {
            var oAjax = new XMLHttpRequest();
        }
        else
        {
            var oAjax = new ActiveXObject("Microsoft.XMLHTTP");//IE6浏览器创建ajax对象
        }
        oAjax.open("GET",url,true);//把要读取的参数的传过来。
        oAjax.send();
        oAjax.onreadystatechange=function()
        {
            if(oAjax.readyState==4)
            {
                if(oAjax.status==200)
                {
                    fnSucc(oAjax.responseText);//成功的时候调用这个方法
                }
                else
                {
                    if(fnfiled)
                    {
                        fnField(oAjax.status);
                    }
                }
            }
        };
}
```
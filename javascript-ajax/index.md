# Javascript原生使用Ajax


<!--more-->

> 一般情况下，直接使用原生 Javascript 的代码是通用的

```javascript
// 创建 XMLHttpRequest 对象
var xmlHttpRequestObject = null;
if (window.ActiveXObject) {
	xmlHttpRequestObject = new ActiveXObject("Microsoft.XMLHTTP");
} else {
	xmlHttpRequestObject = new XMLHttpRequest();
}
var url = "target.html"; // 目标路径
var data = ""; // 传输的数据
xmlHttpRequestObject.open("POST" , url , true); // 第三个参数默认 true 异步 false 同步
xmlHttpRequestObject.setRequestHeader("Content-Type" , "application/x-www-form-urlencoded");
xmlHttpRequestObject.onreadystatechange = function() {
	// readyState产生变化时需要运行
    if (xmlHttpRequestObject.readyState == 4 && xmlHttpRequestObject) {
    	var res = xmlHttpRequestObject.responseText;
        var jsonObject = eval("("+res+")");
	}
}
xmlHttpRequestObject.send(data);
```

原生JS使用XMLHttpRequest对象进行异步通信，下面为该对象的主要属性与方法

属性：onreadystatechange
    
    在readyState变化时运行的方法

属性：readyState

> 状态描述
> 0 请求未初始化（在调用 open() 之前）
> 1 请求已提出（调用 send() 之前）
> 2 请求已发送（这里通常可以从响应得到内容头部）
> 3 请求处理中（响应中通常有部分数据可用，但是服务器还没有完成响应）
> 4 请求已完成（可以访问服务器响应并使用它）

属性：responseText
    
    由服务器返回的数据

方法：open(type,url,true/false)
    
    初始化请求：第一个参数为请求类型，第二个参数为请求路径，第三个参数为是否异步传输

方法：send(data)
    
    发送请求：data为传输的数据

方法：setRequestHeader("header","value");
    
    设置请求头，Send之前



## Ajax精通

##### 一、简介

AJAX（Asynchronous Javascript And XML）：异步的Javascript和XML，是一种无刷新页面与服务器交互的技术（页面不刷新就能获得服务器响应）

**原来的交互：**

​		1、发送请求

​		2、服务器收到请求，调用对应的servlet进行处理，处理完成之后服务器返回信息

​		3、浏览器收到了服务器响应的数据，把页面之前的数据清空，显示新的数据

**现在的交互（XmlHttpRequest对象）：**

​		1、XMLHttpRequest对象帮我们发送请求

​		2、服务器收到请求，调用对应的servlet进行处理，处理完成之后服务器返回信息

​		3、XMLHttpRequest对象收到数据（浏览器感受不到数据的交互）

**XMLHttpRequest原生编程**

```javascript
var xhr = new XMLHttpRequest();//创建XMLHttpRequest对象
xhr.open("GET","login",true);//建立连接
xhr.send();//传输数据

//监听XMLHttpRequest对象的状态
xhr.onreadystatechange=function(){
    if(xhr.readystate === 4 && xhr.status === 200){
        document.getElementById("view").innerHTML = xhr.responseText;
    }
}
```

原生编程比较麻烦，推荐使用Jquery操作Ajax

##### 二、图解

![1563164897888](C:\Users\leer0\Pictures\Ajax图解.png)

##### 三、JQUERY操作AJAX

```jsp
<%
	pageCintext.setAttribute("ctp",request.getContextPath())
%>
<a href="getinfo" id="aBtn">获取信息</a>
<a href="getinfo" id="ajaxBtn">获取信息</a>
<script>
	/*1、$.get*/
    $("#aBtn").click(function(){
        //$.get(url,[data],[callback],[type])
        //data是要传递的数据，如键=值&键=值、js对象:会自动转换为键=值&键=值
        //callback：回调函数，随便定义一个参数，该参数封装了服务器返回到数据
        //type：返回到数据类型，可以不写，自动识别
        //type支持的返回数据格式：xml、html、script、json、text、_default
        //post()发送post请求，get()发送get请求
        $.get("${ctp}/getinfo",{lastName:"李使命"，age:18},function(abc){
            //abc代表服务器传给我们的数据，如果服务器转发到一个页面，data代表整个页面
            alert(abc);
            //var obj = JSON.parse(abc);将返回到数据转为json对象或js对象
            
            //get或post方法的最后一个参数type直接指定为json类型，然后jquery会自动将返回的数据转化为json对象
        });
        //禁用默认行为
        return false;
    });
    
    //使用底层ajax方法
    $("#ajaxBtn").click(function(){
        //发送ajax请求
        //所有的参数都可以通过一个对象设置并传递
        var options = {
            url:"${ctp}/getstuinfo",//规定请求地址
            type:"GET",//请求方式
            data:{"lastName":"lee",age:18},//发送的数据
            async:true;//默认为true，指的是异步请求
            success：function(data){
            	var lastName = data.lastName;
                var age = data.age;
                $("div").append("姓名："+lastName+"<br/>年龄："+age);
            },
            //有以下三个参数：XMLHttpRequest 对象、错误信息、（可选）捕获的异常对象。
            error:function(a,b){
                alert("请求失败了"+b+"状态码："+a.status)
            },
            dataType:"json"
        }
        $ajax.(options);
        return false;
    });
</script>
```

服务器使用json工具生成json字符串给浏览器
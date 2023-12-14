# a



阻止跳转：

1. 用“#”，语法“a href="#"”

   ~~~html
   <a href="#">百度</a>   <!--点击时它会跳转到页面的最顶端了-->
   ~~~

   

2. 用“javascript:”，语法“a href="javascript:void(0)"”

   ~~~html
   <a href="javascript:void(0)">百度</a>  <!--直接在里面添加一个阻止事件，当然直接是"javascript:"也是可以的，最好还是写全吧-->
   ~~~

   

3. 用getElementById获取a标签，使用“return false;”取消跳转。

   ~~~html
   <a id="Header1_HeaderTitle" href="www.baidu.com">百度</a>
   ~~~

   ~~~js
   let div1 = document.getElementById("Header1_HeaderTitle");
   
   div1.onclick = function(){
         return false;
   }
   ~~~

   
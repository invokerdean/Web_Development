#### 基本概念
XSS：跨站脚本攻击，英文名称是Cross Site Script，XSS攻击，通常指黑客通过注入非法的 html 标签或者 javascript 代码，从而在用户浏览网页时，控制用户浏览器的一种攻击。举个例子：某个黑客发表了一篇文章其中包含了恶意的JS代码，所有访问这篇文章的人都会执行这段JS代码，这样就完成XSS攻击。

#### 类型
DOM xss
反射型 xss
存储型 xss


#### 实现

#### 防御
1. HttpOnly：
```
  // koa
  ctx.cookies.set(name, value, {
      httpOnly: true // 默认为 true
  })
```  
2. 输入检查，一般是用于对于输入格式的检查，例如：邮箱，电话号码，用户名，密码……等，按照规定的格式输入。
3. HtmlEncode
> 当用户输入<script>window.location.href=”http://www.baidu.com”;</script>, 最终保存结果为 &lt;script&gt;window.location.href=&quot;http://www.baidu.com&quot;&lt;/script&gt;, 在展现时，浏览器会对这些字符转换成文本内容，而不是一段可以执行的代码。
 4. JavaScriptEncode:对下列字符加上反斜杠:",',\,\n,\r

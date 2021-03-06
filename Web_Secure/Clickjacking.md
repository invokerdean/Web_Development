#### 基本概念
Clickjacking： 点击劫持，是指利用透明的按钮或连接做成陷阱，覆盖在 Web 页面之上。然后诱使用户在不知情的情况下，点击那个连接访问内容的一种攻击手段。这种行为又称为界面伪装(UI Redressing) 。
大概有两种方式：

#### 攻击方式
1. 攻击者使用一个透明 iframe，覆盖在一个网页上，然后诱使用户在该页面上进行操作，此时用户将在不知情的情况下点击透明的 iframe 页面；
2. 攻击者使用一张图片覆盖在网页，遮挡网页原有的位置含义。

>黑客创建一个网页利用 iframe 包含目标网站；隐藏目标网站，使用户无法无法察觉到目标网站存在；构造网页，诱变用户点击特点按钮,用户在不知情的情况下点击按钮，触发执行恶意网页的命令。

![avatar](https://user-gold-cdn.xitu.io/2017/10/11/c47f50c6712710f7686e249458dce62d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 防御
* X-FRAME-OPTIONS HTTP 响应头是用来给浏览器指示允许一个页面可否在<frame>, <iframe> 或者 <object> 中展现的标记。网站可以使用此功能，来确保自己网站内容没有被嵌到别人的网站中去，也从而避免点击劫持的攻击。
  
 * js 判断顶层窗口跳转，可轻易破解，意义不大

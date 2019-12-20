## 修改Text和HTML
jQuery对象的text()和html()方法分别获取节点的文本和原始HTML文本
> 无参数调用text()是获取文本，传入参数就变成设置文本，HTML也是类似操作
```
<!-- HTML结构 -->
<ul id="test-ul">
    <li class="js">JavaScript</li>
    <li name="book">Java &amp; JavaScript</li>
</ul>
```
分别获取文本和HTML：
```
$('#test-ul li[name=book]').text(); // 'Java & JavaScript'
$('#test-ul li[name=book]').html(); // 'Java &amp; JavaScript'
```
**一个jQuery对象可以包含0个或任意个DOM对象，它的方法实际上会作用在对应的每个DOM节点上**,如果该对象没有返回任何dom节点，则无效（无需添加if）
## 修改CSS
调用jQuery对象的css('name', 'value')方法，
> **jQuery对象的所有方法都返回一个jQuery对象（可能是新的也可能是自身）可以链式调用**
```
var div = $('#test-div');
div.css('color'); // '#000033', 获取CSS属性
div.css('color', '#336699'); // 设置CSS属性
div.css('color', ''); // 清除CSS属性
```
```
//修改class
var div = $('#test-div');
div.hasClass('highlight'); // false， class是否包含highlight
div.addClass('highlight'); // 添加highlight这个class
div.removeClass('highlight'); // 删除highlight这个class
```

## 显示隐藏DOM
jQuery直接提供show()和hide()方法，不用关心它是如何修改display属性的
## 获取DOM信息
1. 宽高,wdith(),height()

```
// 浏览器可视窗口大小:
$(window).width(); // 800
$(window).height(); // 600

// HTML文档大小:
$(document).width(); // 800
$(document).height(); // 3500

var div = $('#test-div');
div.width(); // 600
div.height(); // 300
div.width(400); // 设置CSS属性 width: 400px，是否生效要看CSS是否有效
div.height('200px'); // 设置CSS属性 height: 200px，是否生效要看CSS是否有效
```
2. attr()和removeAttr()方法用于操作DOM节点的属性：
```
// <div id="test-div" name="Test" start="1">...</div>
var div = $('#test-div');
div.attr('data'); // undefined, 属性不存在
div.attr('name'); // 'Test'
div.attr('name', 'Hello'); // div的name属性变为'Hello'
div.removeAttr('name'); // 删除name属性
div.attr('name'); // undefined
```

3. prop(),attr(),is();
对于checked这样可以没有值的属性
```
<input id="test-radio" type="radio" name="test" checked value="1">
```
```
var radio = $('#test-radio');
radio.attr('checked'); // 'checked'
radio.prop('checked'); // true
radio.is(':checked'); // true
//类似地
xxx.is(':selected');
```
## 操作表单
对于表单元素，jQuery对象提供val()方法获取和设置对应的value属性：
```
 <select id="test-select" name="city">
        <option value="BJ" selected>Beijing</option>
        <option value="SH">Shanghai</option>
        <option value="SZ">Shenzhen</option>
    </select>
```
```
select.val(); // 'BJ'
select.val('SH'); // 选择框已变为Shanghai
```
## 修改DOM结构
#### 增
调用append()方法,除了接受字符串，append()还可以传入原始的DOM对象，jQuery对象和函数对象：
```html
<div id="test-div">
    <ul>
        <li id='scheme'><span>JavaScript</span></li>
        <li><span>Python</span></li>
        <li><span>Swift</span></li>
    </ul>
</div>
```
```
ul.append('<li><span>Haskell</span></li>');

// 创建DOM对象:
var ps = document.createElement('li');
ps.innerHTML = '<span>Pascal</span>';
// 添加DOM对象:
ul.append(ps);

// 添加jQuery对象:
ul.append($('#scheme'));//JavaScript会变动到最后一个子元素位置.其原理是首先从文档移除，然后再添加

// 添加函数对象:
ul.append(function (index, html) {
    return '<li><span>Language - ' + index + '</span></li>';
});//传入函数时，要求返回一个字符串、DOM对象或者jQuery对象。
//jQuery的append()可能作用于一组DOM节点，只有传入函数才能针对每个DOM生成不同的子节点。
```
> 类似的方法还有prepend()，将元素添加到调用者的第一个子元素位置
对于同级节点可以用after()或者before()方法，添加到调用者的前后

#### 删
要删除DOM节点，拿到jQuery对象后直接调用remove()方法就可以了。如果jQuery对象包含若干DOM节点，实际上可以一次删除多个DOM节点：
```
var li = $('#test-div>ul>li');
li.remove(); // 所有<li>全被删除
```




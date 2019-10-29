#### 为什么用Vue
小而美（语法、开发方式灵活），生态丰富（模块，插件，开发团队），广泛的应用

#### 结构
基础知识点：模板语法，条件渲染，列表渲染等
核心技术：vue-router，vuex
集成Vue2.x

#### NVM
node version manager
nvm ls:本地安装了哪些版本
nvm ls remote：服务器上版本
nvm install v11.0.0：安装 
nvm use v8.12.0：切换版本

npm i -g vue-cli
vue-cli version

#### 基本语法
文件结构：template,style,script
插值语法：{{msg}}数据,js表达式
指令（指令缩写）@click,v-if,:herf

#### watch和computed
* watch
```JavaScript
new Vue({
  el:'#app',
  data:{
    msg:'hello vue'
  },
  watch:{
    msg:function(newVal,oldVal){
      console.log('newVal is:'+newVal);
      console.log('oldVal is:'+oldVal);
    }
  }
})
```
* computed
```JavaScript
var arr='test'
new Vue({
  el:'#app',
  data:{
    msg:'hello vue'
  },
  computed:{
    msg1:function(){
      return 'computed:'+this.msg+arr; 
    }
  }
})
```
vue实例外的变量也会加入运算，但实例外变量的改变不会触发计算更新，只有实例内的变量改变时，才会一同更新
>watch常用于异步场景，computed常用于数据联动

#### 样式绑定
```
//1.
v-bind:style="{color:'red'}"
//2.
v-bind:class='{active:true}'//对象的key引号通常可加可不加，会自动转换为字符串,除非有unexpected token类似‘-’
//3.
v-bind:class="['active','add','more',{'another':true}]"
```

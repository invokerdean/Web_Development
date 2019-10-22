#### 5.2实质问题
当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使是在其词法作用域之外执行

> 无论通过何种手段将内部函数传递到所在的词法作用域以外，它都会持有对原始定义作用域的引用，无论何处执行这个函数都会使用闭包

#### 5.3现在有点懂了
本质上无论何时，如果将函数作为第一级的值类型传递，就会与闭包相关，在定时器，AJAX，事件监听，跨窗口通信，Web Workers，或其他任何异步（同步）任务，只要使用了回调函数，实际就是在使用闭包

#### 5.4循环与闭包
```
for(var i=1;i<=5;i++){
    setTimeout(function(){
        console.log(i);
    },i*1000);
}//6,6,6,6,6
```
修改方法：
```
for(var i=1;i<=5;i++){
    (function(){
       var j=i;
       setTimeout(function(){
          console.log(j);
      },j*1000);
    })()
}
//或者
for(var i=1;i<=5;i++){
    (function(j){
       setTimeout(function(){
          console.log(j);
      },j*1000);
    })(i)
}
//或者
for(var i=1;i<=5;i++){
    let j=i;
    setTimeout(function(){
        console.log(j);
    },j*1000);
}
//或者
for(let i=1;i<=5;i++){
    setTimeout(function(){
        console.log(i);
    },i*1000);
}//每个块作用域里i的值不同
```

#### 5.5模块
```
function CoolModule(){
    var something='cool';
    var anonther=[1,2,3];
    function doSomething(){
        console.log(something);
    }
     function doAnother(){
        console.log(another.join('!'));
    }
    return {
        doSomething,
        doAnother
    }
}
var foo=CoolModule();
foo.doSomething();//cool
foo.doAnother()://1!2!3
```
可以将这个对象返回值看做本质上是模块的公共API
> 最常见使用模块模式的方法是模块暴露，模块中返回对象并非必须，也可以直接返回一个内部函数

```
//每次调用会创建一个新的模块实例
var foo=CoolModule();
foo.doSomething();//cool
foo.doAnother();//1!2!3
var bar=CoolModule();
bar.doSomething();//cool
bar.doAnother();//1!2!3
console.log(foo===bar);
```
> 模块也是普通的函数，因此可以接受参数

* 单例模式：利用IIFE
var foo=(function CoolModule(){...})()
> 通过在模块实例内部保留对api对象的内部引用，可以从内部进行修改，现代的模块大多数依赖加载器/管理器本质上都是将模块定义封装进一个友好的API

```
var MyModule=(function Manager(){
    var modules={};
    function define(name,deps,impl){//名称，依赖，模块api
        for(var i=0;i<deps.length;i++){
            deps[i]=modules[deps[i]];//deps为依赖列表
        }
        modules[name]=impl.apply(impl,deps);
    }
    function get(name){
        return modules[name];
    }
    return {
        define,
        get
    }
})();
MyModule.define('bar',[],function(){
    function hello(who){
        return 'hello '+who;
    }
    return {
        hello
    };
});
var bar=MyModule.get("bar");
console.log(bar.hello('doctor'));//hello doctor
MyModule.define('foo',['bar'],function(){
    var hungry='John';
    function awesome(){
        console.log(bar.hello(hungry).toUpperCase());
    }
    return{
        awesome
    }
})
var foo=MyModule.get("foo");
foo.awesome();//HELLO JOHN
```


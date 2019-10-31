* 实际上，我个人认为this就是调用函数的对象

#### 2.1默认绑定
对于独立函数调用，this指向全局对象
* 实际上调用位置的上下文对象是window；
> foo运行在strict mode下绑定undefined，但strict mode下调用foo可以默认绑定

#### 2.2隐式绑定
函数调用位置有上下文对象时。隐式绑定规则会把函数调用中的this绑定到这个上下文对象，对象属性引用链只有最后一层起作用
```JavaScript
obj.foo();//绑定的是obj
obj1.obj2.foo();//绑定的是obj2
```
 ###### 隐式丢失
 ```JavaScript
 var bar=obj.foo;
 bar()//绑定的是全局对象
 ```
 类似的情况发生在回调函数(即函数作为参数传递，发生了隐式赋值)
 ```JavaScript
 setTimeout(obj.foo,100);//绑定的是全局对象
 ```
 实际上，调用回调函数的函数内部可能会修改this的绑定，这是不可控的(除非调用函数是自己写的)
 
 #### 2.3显示绑定
 指在某个对象上强制调用函数，利用call和apply,如果传入的是原始值，会被转换成对象(new String(),new Boolean,new Number)
 ```JavaScript
 foo.call(obj);//绑定的是obj
 ```
 ###### 硬绑定（基于该原理，ES5提出了bind函数）
 1. 创建一个包裹函数，负责接收参数并返回值
 ```JavaScript
 var bar=function(){
  foo.call(obj);
 }
 ```
 2. 创建一个辅助函数
 ```
 function bind(fn,obj){
  return function(){
    return fn.apply(obj,arguments)
  }
 }
 ```
 ###### api调用上下文
 例如数组的forEach方法，提供一个可选参数用于绑定this，实际都是通过call和apply实现
 #### 2.4new绑定
 JavaScript中的构造函数只是一些被new操作符调用的普通函数，也就是说，**并不存在所谓的构造函数，只有对函数的构造调用**
 构造调用过程：
 1. 创建一个新对象
 2. 新对象进行[[Prototype]]连接
 3. 新对象绑定到函数调用的this
 4. 如果该函数没有返回其他对象，则自动返回该新对象（否则返回其他对象）
 
 #### 2.5优先级
 **new>显示>隐式>默认**
 ```javascript
  function foo(a,b){
    this.val=a+b;
  }
  var bar=foo.bind(null,'p1');
  var baz=new bar('p2');
  console.log(baz.val);//p1p2
 ```
 
 #### 2.6例外
 null，undefined传入call，apply，bind时，绑定的是全局对象（场景：展开数组，或者this不关心，取一个null站位）
 安全的绑定：
 ```
 var o=Object.create(null);
 foo.apply(0,[2,3]);
 ```

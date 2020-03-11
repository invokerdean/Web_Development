## 原理
JavaScript的所有对象都是由根对象Object.prototype克隆而来，若需要使用别的特性，可以将构造器的原型指针转向别的对象进行继承。

以构造器Person()为例，new的步骤如下：
1. obj=new Object()获得一个根对象的克隆
2. 将obj的原型指针_proto_指向构造器的原型来继承原型的属性方法
3. ret=constructor.apply(obj,arguments)使用构造器对obj进行属性设置
4. 确保返回的是对象，若ret是对象则返回ret，若不是则返回obj

> 第4步造成了new的特性，若构造器返回的是对象，则返回该对象，若是基本类型，则返回obj
## Code
```JavaScript
function Person(name){
  this.name=name;
}

Person.prototype.sayName=function(){
  console.log(this.name);
}

function ObjectFactory(){
  var obj=new Object(),
      Constructor=[].shift.call(arguments);
  obj.__proto__=Constructor.prototype;
  var ret=Constructor.apply(obj,arguments);
   
  return ret instanceof Object ? ret : obj;
}

var person1=ObjectFactory(Person,'james');
person1.sayName();//james
console.log(person1.name);//james
```



#### 4.2编译器
变量提升本质是js编译执行造成的，由于在执行前编译器对词法单元进行解析的时候会先进行LHS,RHS查询作用域，因此实际上，```var a```在编译阶段就执行了，```a=2```赋值操作留在原地等待执行阶段

>每个作用域都会进行变量提升，并且函数表达式不会提升，因为视为赋值操作
```
foo();//TypeError
var foo=function(){
    //...
};
```

```
foo();//TypeError
bar();//ReferenceError
var foo=function bar(){
  console.log('a');
};
```
实际上：函数表达式中具名函数会被当做匿名函数赋值，并在其函数作用域中含有var bar=...self...
```
var foo=function bar(){
  console.log('a');
};
bar();//Uncaught ReferenceError: bar is not defined,
```

#### 4.3函数优先
函数会首先被提升，然后是变量，(后出现的同名变量提升由于重复声明会被忽略，但出现在后面的函数声明会覆盖前面）

```
foo();//Uncaught TypeError: foo is not a function
var a=true;
if(a){
    function foo(){console.log('a')};
}else{
    function foo(){console.log('b')};
}
```
块内部声明的函数不会提升，尽量避免，该行为的结果并不可靠，可能会随着标准改变（不同浏览器处理方式不同，可能为else中的function）

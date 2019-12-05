## 1.Node基础
#### 1.1基本概念
网络应用开发平台，是一个完整的解决方案，包括
* 标准类库
* 模块系统(commonJS)
* npm( 第三方类库包管理)
整体架构：
1. 最底层：V8引擎(js运行环境),libeio(维护线程池),libev(提供eventloop)
2. 中层：C++ binding，将底层接口暴露给js使用
3. 高层：标准类库
 #### 1.2关键机制
 ###### Event Loop
 阻塞式I/O的方案：同步，多线程，多进程，eventloop
 ###### REPL
 Node交互式运行环境(类似shell)
 > Read->eval->Print->Loop->Read...
 
 ## 2.Node事件编程
 #### eventloop执行顺序
 timers->I/O callbacks->idle,prepare->poll->check->close callback->timers...
 
 每个阶段维护一个FIFO队列，到达该阶段就进行处理直到队列为空或超出回调上限
 * setTimeout(()=>{},0)对应timer阶段
 * setImmediate()当eventloop过了poll进入check阶段就会执行，不需要等下一个eventloop(即微任务)
 * process.nextTick():无视当前eventloop所处阶段，只要当前操作完成就会立即执行
 
 #### EventEmitter
 是大多数node类库的基类，通过on和emit进行事件的监听和发送，从而完成观察者模式
 ```JavaScript
 const EventEmitter=require('events');
 class MyEmitter extends EventEmitter {};
 const myEmitter=new MyEmitter();
 myEmitter.on('event',(a,b)=>{
   console.log(a,b,this);//a b {}
 });
 myEmitter.emit('event','a','b');
 ```
 #### event进阶
* 只监听一次：
```JavaScript
//server是eventEmitter实例
server.once('connection',(stream)=>{
  console.log('Ah,we have our first user');
});
```
* 改变插入顺序：(即改变默认的压栈行为)```emitter.prependListener()```,```emitter.prependOnceListener()```,在事件队列最前面添加事件
* 删除：```emitter.removeListener(eventName,listener)```,```emitter.removeAllListeners([eventName])```
* 设置最大阈值：emitter默认只能添加10个监听器，可以通过``emitter.setMaxListeners(n)```进行设置

## 3.Stream
![QGhFk6.png](https://s2.ax1x.com/2019/12/05/QGhFk6.png)
节省内存并可以实时进入callback阶段
#### 类型
```
var Stream=require('stream');
var Readable=Stream.Readable;
var Writable=Stream.Writable;
var Duplex=Stream.Duplex;
var Transform=Stream.Transform; 
```

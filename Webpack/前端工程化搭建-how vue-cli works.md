## How vue-cli works
vue-cli的原理是使用了webpack,是一种广泛适用的初始化方法，而公司常常需要定制化

#### 1.*npm init*
初始化，填入信息，获得package.json
#### 2.*npm i webpack vue vue-loader*
此时生成了node_modules文件夹和lock文件，根据警告安装依赖

> 此时，可以新建src目录，创建app.vue文件，但vue文件是一个组件不能直接在浏览器运行，需要配置入口文件
#### 3.*webpack.config.js和index.js*
以index.js作为入口文件进行配置
```JavaScript
//index.js
import Vue from 'vue'
import app from './app'

const root=document.createElement('div');
document.body.append(root);

new Vue({
  render:(h)=>h(app)//h参数是vue里面的createapp
}).$mount(root)//挂载到root节点
```
```JavaScript
//webpack.config.js
const path=require('path');
module.exports={
  entry:path.join(__dirname,'src/index.js'),
  ouput:{
    filename:'bundle.js',
    path:path.join(__dirname,'dist'),//新建一个当前目录下的dist文件夹
  }
  //由于webpack原生只支持es5js，需要loader加载vue文件
  module:{
    rules:[
      {
        test:/.vue$/,
        lodaer:'vue-loader'
      }
    ]
  }
}
```
```JavaScript
//package.json
{
  //...
  "script": {
    //...
    "build":"webpack --config webpack.config.js"
  }
  //...
}
```
> 此时运行npm run build 即可打包

#### 4.*此时输出的bundle.js构成*
> 把不同类型静态资源打包成一个js
1. webpack处理依赖的代码
2. vue的源码（尚未分离处理）
3. ...开发代码

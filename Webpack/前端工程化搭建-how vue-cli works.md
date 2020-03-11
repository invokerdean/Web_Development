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
  render:(h)=>h(app)//h参数是vue里面的createElement
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
        test:/\.vue$/,
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

#### 5.*项目加载各种静态资源和css预处理器*
###### 静态资源
```JavaScript
//webpack.config.js
const path=require('path');
module.exports={
  entry:path.join(__dirname,'src/index.js'),
  ouput:{
    filename:'bundle.js',
    path:path.join(__dirname,'dist'),//新建一个当前目录下的dist文件夹
  }
  module:{
    rules:[
      {
        test:/\.vue$/,
        lodaer:'vue-loader'
      },
      {
        test:/\.css$/
        use:[
          'style-loader',
          'css-loader',
        ]
      },
      {
        test:/\.(jpeg|gif|png|svg|jpg)&/,
        use:[
          {
            loader:'url-lodaer',//是file-loader的封装，可以将图片转换成base64代码,install时需要安装file-loader依赖
            options:{
              limit：1024,//如果小于1024，就转码保存到代码，
              name:'[name]-aaa.[ext]'//输出文件名
            }
          },
        ],
      },
    ]
  }
}
```
```JavaScript
//index.js
import '.../test.css'
import '.../bg.jpeg'
```
```css
//test.css
body {
  color:red;
  background-image:url('../images/done.svg');
}
```
输出：
/dist:bg-aaa.jpeg,bundle.js
```
"....background-image:url("+__webpack_require__(17)+"...""//代码编号

//17号代码为done.svg的base64编码
```
> 可见css中的图片依赖也会被一起打包
###### css预处理器
```JavaScript
//预处理器打包，以stylus为例
{
  test:/\.styl$/,
  use：[
    'style-loader',//用于写入到html
    'css-loaer',//最终会变成css文件来处理
    'stylus-loader'//只处理styl文件，转化成css代码，每层只负责自己的事
  ]
}
```
#### 6.*devServer*
**是什么**：webpack-dev-server包
```JavaScript
//package.json
{
  //...
  //设置运行脚本
  "script": {
    //...
    "build":"webpack --config webpack.config.js",
    "dev":"webpack-dev-server --config webpack.config.js"//专门用于开发环境
  }
  //...
}
```
> 此时需要给script设置环境变量来标识是什么环境：开发环境/生产环境...

* Mac:NODE_ENV = 'production'即可读取环境变量
* Windows:使用set方式
* 跨平台:使用cross-env包:
```
//package.json
{
  //...
  "script": {
    //...
    "build":"cross-env NODE_ENV=production webpack --config webpack.config.js",
    "dev":"cross-env NODE_ENV=development webpack-dev-server --config webpack.config.js"//专门用于开发环境
  }
  //...
}
```
```JavaScript

//webpack.config.js
const HTMLPlugin=require('html-webpack-plugin');//此时需要HTML-webpack-plugin，包含bundle.js，否则跑不了
const webpack=require('webpack')
const isDev=process.env.NODE_ENV==='development'//启动脚本中设置的环境变量都保存在process.env里

//module.exports={
const config={//保存配置
  target:'web'//?
  //...
  plugin:[
    new webpack.DefinePlugin({
      'process.env':{
        NODE_ENV:isDev?'"development"':'"production"',
       }
    }), //便于源码和webpack调用process.env，用于对不同环境使用不同版本的框架
    new HTMLPlugin(),
  ]
}

if(isDev){
  config.devtool='#cheap-module-eval-source-map';//调试代码，在浏览器中映射成编译前的源代码
  config.devServer={
    port:8000,
    host:'0.0.0.0',
    overlay:{
      errors:true//webpack编译错误显示到网页上
    }
    //open:true 运行脚本自动打开浏览器
    //historyFallback:{} 单页应用定位路由到index.html
    hot:true//热更新，修改代码只重新渲染该部分组件，配合插件
  }
}
module.exports=config
```

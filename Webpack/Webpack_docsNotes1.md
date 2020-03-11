## 基本结构
* entry 入口文件
* output 出口文件路径，和文件名定义
> {path,filename}
* loader 用于转换某些类型的模块
> 用法：module.rules对象数组中，每个对象是一条规则，规则中定义test和use属性指定对哪些文件使用哪些loader
* plugin 插件，用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。
> 用法：require该插件，并在plugins数组添加new实例
* mode 启动webpack对相应环境的优化
> development, production 或 none 之中的一个

*兼容：webpack 支持所有符合 ES5 标准 的浏览器（不支持 IE8 及以下版本）。webpack 的 import() 和 require.ensure() 需要 Promise。如果你想要支持旧版本浏览器，在使用这些表达式之前，还需要 提前加载 polyfill。*

## 入口

#### vue-cli
安装：```npm install -g @vue/cli```

检查：```vue --version```

创建项目：
1. ```vue create hello-world```
> Vue CLI >= 3 和旧版使用了相同的 vue 命令，所以 Vue CLI 2 (vue-cli) 被覆盖了。如果你仍然需要使用旧版本的 vue init 功能，你可以全局安装一个桥接工具：
```npm install -g @vue/cli-init```
`vue init` 的运行效果将会跟 `vue-cli@2.x` 相同
```vue init webpack my-project```

[![KHRilR.md.png](https://s2.ax1x.com/2019/11/01/KHRilR.md.png)](https://imgchr.com/i/KHRilR)

```
cd hello-world
npm run serve
```

2. ```vue ui```
图形化创建

#### 组件化
* 特点：独立，整体，可复用
* 好处：实现功能模块复用，高执行效率，开发单页复杂应用（拆分逻辑）
* 拆分方式：300行原则，复用（例如头部导航，侧边栏），业务复杂性
* 问题：组件状态管理（Vuex），多组件混合使用，多页面，复杂业务（vue-router），组件间传参、消息、事件管理（props，emit/on bus）

#### Vue-router

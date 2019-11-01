#### 1.vue-cli
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

#### 2.组件化
* 特点：独立，整体，可复用
* 好处：实现功能模块复用，高执行效率，开发单页复杂应用（拆分逻辑）
* 拆分方式：300行原则，复用（例如头部导航，侧边栏），业务复杂性
* 问题：组件状态管理（Vuex），多组件混合使用，多页面，复杂业务（vue-router），组件间传参、消息、事件管理（props，emit/on bus）

#### 3. Vue-router
```html
<router-link to='/info'>Info</router-link>
```
```javascript
//Router.js
//导入组件
import Info from  './views/Info.vue';
Vue.use(Router);
//创建路由
export default new Router({
  routes:[
    {
      path:'/info',
      name:'info',
      component:Info
    }
  ]
}) 
```
#### 4.Vuex
* 单向数据流：actions->states->views
Vuex应用：
1. 多个视图依赖于同一个状态(例如菜单导航，点击某个tab，其他tab变成未激活状态)
2. 来自不同视图的行为改变同一个状态(例如评论弹幕)
* 概念：为Vue提供状态管理模式，组件状态集中管理，组件状态改变遵循统一规则
状态管理页
```javascript
//Store.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  //状态
  state:{
    count:0
  },
  //改变状态的方法集
  mutations:{
    increase(){
      this.state.count++
    }
  },
  actions:{}
}) 
```
改变状态的页面
```javascript
//info.vue
<script>
import store from '@/store'
export default{
  name:'info',
  store,
  methods:{
    add(){
      console.log('add Event from info ')//挂载的事件触发函数
      store.commit('increase')//提交一次increase，此时Vuex中的count就会变化
    }
  }
}
</script>

```
状态接收页
```javascript
//about.vue
<script>
import store from '@/store'
export default{
  name:'about',
  store,
  data(){
    return {
      msg:store.state.count
    }
  }
}
</script>

```

#### 5.调试
1. debugger,console.log,alert
[![KbFg9U.md.png](https://s2.ax1x.com/2019/11/01/KbFg9U.md.png)](https://imgchr.com/i/KbFg9U)
2. vue-develop-tool插件
3. window.vue=this(此时window.vue绑定的就是当前组件)

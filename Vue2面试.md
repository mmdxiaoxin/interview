## Vue2面试总结

### 1.生命周期

**1.1 生命周期有哪些？发送请求在created还是mounted？**

Vue2.x系统自带有8个

```vue
beforeCreate
created
beforeMount
mounted
beforeUpdate
updated
beforeDestroy
destroyed
```

发送请求在created还是mounted？

```vue
根据业务情况
```

**1.5 第二次或者第N次进去组件会执行哪些生命周期？**
如果当前组件加入了keep-alive，只会执行一个生命周期

```vue
activated
```

没有加入则正常执行4个生命周期

**1.7 加入keep-alive会执行哪些生命周期**

如果使用了keep-alive组件，当前的组件会额外增加2个生命周期

```vue
activated
deactivated
```

如果当前组件加入了keep-alive第一次进入这个组件会执行5个生命周期

```vue
beforeCreate
created
beforeMount
mounted
activated
```

**1.8 你在什么情况下用过哪些生命周期？**

```
created   ===> 单组件请求
mounted   ===> 同步可以获取dom，如果先子组件请求后父组件请求
activated ===> 判断id是否相同，如果不相同发起请求
destroyed ===> 关闭页面，记录视频播放时间，初始化从上一次的历史开始播放
```

### 2.关于组件

**2.1 组件传值（通信）的方式**

```
父 -> 后代 （后代拿到了父的数据）
1. 父组件引入子组件，绑定数据。子组件通过props来接收。
	(子不能直接修改父组件的数据)

2. 子组件直接调用this.$parent.xxx使用父组件的数据
	(子可以直接修改父组件的数据)

3. 依赖注入
	优势：父组件可直接向孙子辈节点传递数据。（兄弟辈不行）
```

```vue
后代 -> 父 （父拿到了后代的数据）
1. 子组件传值给父组件
	子组件定义自定义时间 this.$emit
	
2. 父组件直接拿到子组件的数据
	<List ref='child'></List>
	this.$refs.chile[.value]
```

```vue
平辈<->平辈 （兄弟可以拿到数据）
	通过新建bus.js (export default new Vue())文件实现
```

**2.2 父组件直接修改子组件的值**

```vue
<List ref='child'></List>
this.$refs.chile[.value]
```

**2.3 子组件如何直接修改父组件的值**

```vue
子组件可以使用：this.$parent.xxx去修改
```

**2.4 如何找到父组件？**

```vue
this.$parent
```

**2.5 如何找到根组件？**

```vue
this.&root
```

**2.6 keep-alive**

```vue
keep-alive是什么：缓存当前组件
```

**2.7** **slot插槽**

```vue
匿名插槽   : 没有名字 <slot>
```

```vue
具名插槽   : 含有名字 <slot name='name'>  <template #name>引用
```

```vue
作用域插槽 : 传值   <slot name='footer' :arr='custom_arr'> 自定义组件
	 接收 :        <CustomComponents>
    				<template #footer='arr'>arr是对象</template>
    				<template #footer='{arr}'>arr是数组</template>
    				<template #footer='{arr, str}'>接收多个传值</template>
    			  </CustomComponents>
```

**2.8 provide/inject**

```
provide/inject ===> 依赖注入 
```

**2.9 如何封装组件**

```
组件要复杂，涉及的知识点 : slot, 组件通信
```

### 3.关于Vuex

**3.1 Vuex有哪些属性？**

```
state ==> 全局共享属性
```

**3.2 Vuex使用state**

```vue
this.$store.state.xxx
```

```
辅助函数：mapState
```

以上两种都可以获取state的值，但是第一种可以修改state的值，第二种不可以

**3.3 Vuex的getters值修改**



3.4 



3.5



### 4.关于路由

**4.1 路由的模式和区别**

```
路由的模式：history hash
```

```
区别：
1. 关于找不到当前页面发送请求的问题
	history会给后端默认发送一次GET请求,而hash不会（可以配置404页面）
2. 关于项目打包前端自测问题
	hash是可以看到内容的
	history默认情况是看不到内容的
3. 关于表象不同
	hash：#
	history：/
```

**4.2 子路由和动态路由**

```

```

**4.3 路由传值**

```

```

**4.4 导航故障**

```

```

解决：

```javascript
import VueRouter form 'vue-router';
const routerPush = VueRouter.prototype.push;
VueRouter.prototype.push = function (loacation) {
    return routerPush.call(this, location).catch(error => error);
}
```

**4.5 `$router` 和 `$route`的区别** 

```javascript
$router：不仅包含当前路由，还包含整个路由的属性和方法
$route：包含当前路由对象
```

**4.6 导航守卫**

```vue
1. 全局守卫
	
	beforeEach 路由进入之前
	afterEach 路由进入之后
	
2. 路由独享守卫
	
	beforeEnter 路由进入之前
	
3. 组件内守卫
	
	beforeRouteEnter 路由进入之前
	beforeRouteUpdate 路由更新之前
	beforeRouteLeave 路由离开之前
```



### 5.关于API

**5.1 $set**

```javascript
有没有碰到过，数据更新视图没有更新的问题 ==> $set
this.$set(target, 'key', 'value');
target：引用类型数据
key：数据项
value：修改后的值
```

**5.2 $nextTick**

```JavaScript
$nextTick返回的参数【函数】，是一个异步的。
```

源码：

```javascript
class Vue {
    constructor(options) {
        options.created.bind(this);
        this.$el = document.querySelector(options.el);
        options.mounted.bind(this);
    }
    $nextTick(callback) {
        return Promise.resolve().then(()=>{callback()});
    }
}
```

```
功能：获取更新后的dom
```

**5.3 $refs**

```
获取DOM
```

**5.4 $el**

```
获取当前组件的根节点
```

**5.5 $data** 

```
获取当前组件的data数据
```

**5.6 $children**

```
获取当前组件的所有子组件
```

**5.7 $parent**

```
找到当前组件的父组件，找不到返回自身
```

**5.8 $root**

```
找到根组件
```

**5.9 data定义数据**

```
数据定义在data的return内和return外的区别：

1.return外：单纯修改这个数据时不会更新视图的，因为没有被get/set
2.return内：是可以修改的（但是也会同时更新return外数据的修改）
```

**5.10 computed计算属性**

```
computed计算属性的结果值，可以修改吗？
	可以的，需要通过get/set写法

当前组件v-model绑定的值是computed来到，那么可以修改吗？
	可以的，需要get/set写法
```

**5.11 watch**

```
值发生改变去执行,以下为一个示例：
```

```vue
<script>
	export default {
        watch: {
            obj: {
                handler(newVal, oldVal) {
                    console.log(...)
                },
                immediate:true,
                deep:true //深度监听，监听对象中内容的改变
            }
        }
    }
</script>
```

**5.12 methods 和 computed 的区别**

```
computed是有缓存机制的，methods是没有缓存机制的（调用几次执行几次）
```



### 6.关于指令

**6.1 如何自定义指令**

```javascript
全局：main.js
Vue.directive('demo', {
	inserted: function (a, b, c) {
		console.log(a, b, c);
	}
})
```

```vue
局部：某一个组件内
<script>
	export default {
        directives: {
            demo: {
                bind: function (el) {
                    console.log (1);
                }
            }
        }
    }
</script>
```

**6.2 vue 单项绑定**

```
双向绑定: v-model
```

```
单向绑定: v-bind
```

**6.3 v-if 和 v-for的优先级**

```JavaScript
vue2中：v-for 优先级大于 v-if

源码：
export function genElement (el: ASTElement, state: CodegenState): string {
  if (el.parent) {
    el.pre = el.pre || el.parent.pre
  }
  if (el.staticRoot && !el.staticProcessed) {
    return genStatic(el, state)
  } else if (el.once && !el.onceProcessed) {
    return genOnce(el, state)
  } else if (el.for && !el.forProcessed) { //注意这里
    return genFor(el, state)
  } else if (el.if && !el.ifProcessed) {
    return genIf(el, state)
  } else if (el.tag === 'template' && !el.slotTarget && !state.pre) {
    return genChildren(el, state) || 'void 0'
  } else if (el.tag === 'slot') {
    return genSlot(el, state)
  } else {
    // component or element
    ...
}
```

```
vue3中：v-if 优先级大于 v-for
```

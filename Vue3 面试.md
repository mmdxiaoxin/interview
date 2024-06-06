## Vue3 面试

1.Vue2和Vue3的区别？

```javascript
1.vue2和vue3双向绑定 方法不同
	
	vue2: Object.defineProperty()

	*** 后添加的属性是劫持不到的


	vue3: new Proxy()

	*** 即使后添加的也可以劫持到
	*** 还不需要循环

2. $set 在vue3 中没有，因为可以直接劫持到， Proxy

3. v-if和v-for优先级不同


```

2.Vue3如果用setup写怎么组织代码？



3.Vue3如果用setup写如何获取类似于Vue2中的this？



4.Vue3常用的API有哪些？




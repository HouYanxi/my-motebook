 ## keep-alive组件应用

[TOC]



### 案例说明

* 实现主页页面路由跳转相关页面，再返回主页页面时候，页面里面的数据仍然未变
* 组件进行切换时会销毁数据，因此再次返回时数据不复存在，而被<keep-alive>包裹的路由，相关的组件都会被缓存。

### 实现步骤

> vue 命令的前提是全局安装vue-cli工具哦	`yarn global add @vue/cli`;

#### 路由搭建

1. 新建路由项目  `vue create 项目名  `；(*创建项目时选择Router插件，空格选中*)

   ![image-20200302145354210](C:\Users\AS\AppData\Roaming\Typora\typora-user-images\image-20200302145354210.png)

   > 项目搭建好后运行项目 `yarn serve` (*查看package.json/scripts属性即可*)

2. 在src/views/xxx.vue编写页面级组件

3. src/router/index.js配置路由实现跳转

4. 在其共同的父组件上编写导航代码，配置路由出口

#### 未缓存的组件效果

![未缓存](E:\GiFCam图片\GIF.gif)

#### 使用keep-alive缓存相应的组件

##### 第一种  **v-if**   +  **router**

*在App.vue里面*

![image-20200302175147382](C:\Users\AS\AppData\Roaming\Typora\typora-user-images\image-20200302175147382.png)

*在router/router.js里面*

```
const routes=[{
	path:'/',
	name:'Home',
	component:Home,
	meta:{
		keepAlive:true,
	}
},
	{
	path:'/about',
	name:'About',
	component:About,
	meta:{
		keepAlive:false,
	}
	}
]
```
*效果*

![](E:\GiFCam图片\GIF3.gif)

**关于activated钩子函数**

* 只有被keep-alive包裹的，并且指定缓存的页面，才会执行activated()
* 运用缓存组件时，类似发送请求,渲染数据，都只能在activated()里，mouted和created只会在首次加载缓存组件时起作用。

----------



##### 第二种   **keep-alive的include和exclude属性**

> include被缓存的组件名，exclude不被缓存的组件名，include取值还可以是正则，但要搭配v-bind动态属性使用

*在App.vue里*


```
<keep-alive include="home">
	<router-view>
</keep-alive>
```

*将被缓存的组件里*

```
export default{
	name:'home',
}
```




# Vue组件的概念

> 重新演示一遍原生vue页面的编写

1、创建一个xxx.html页面，引入vue.js文件，页面模板如下：

```
<!DOCTYPE html>
<html>
	<head>
		<title>我是你姐<title>
	</head>
	<body>
		<div id="app">
			<h1>｛｛username｝｝</h1>
			<h2 v-text='pwd'></h2>
		</div>
		<script src="../vue.js"></script>
		<script>
			new Vue({
				el:#app,
				data:{
					username:'小明',
					pwd:12345,
				},
				methods:{
					f(){
					
					}
				}
			})
		</script>
	</body>
</html>
```

2、像html页面一样运行即可，更多指令在本电脑，[代码示意](F:\20190723H5\JS\VUE\instructions)

> 进入正题，Vue的组件，组件封装起来就是让人快速、高效复用的

## Vue组件分类

> Vue组件分为全局组件和局部组件

* 全局组件

  ```
  <div id="app">
  	<hello>123</hello>
  </div>
  <div id="app1">
  	<hello>123</hello>
  </div>
  
  <script src="../vue.js"></script>
  <script> 
  //全局组件在Vue对象上定义，每个id都是一个实例，
  //全局组件在每个实例中都能使用
      Vue.component('hello',{
     		template:'<h1>Hello Vue</h1>',
      })
      new Vue({
     		el:'#app',
      })
      new Vue({
      	el:'#app1',
      })
  </script>
  ```

* 局部组件

  ```
  <div id="app">
  //多次调用hello组件
  	<hello>123</hello>
  	<hello>123</hello>
  	<hello>123</hello>
  </div>
  <div id="app1">
  	<hello>123</hello>
  </div>
  <script src="../vue.js"></script>
  <script>
  //   局部组件在app实例中被定义，只能在当前实例中使用
  //   一个实例可以定义多个组件,所以是components对象
      new Vue({
      	el: "#app",
      components: {
      	hello: {
      		template: "<h1>hello world</h1>"
      		}
      	}
      })
  </script>
  ```

## Vue指令与属性

### v-on修饰符

v-on指令的修饰符， 常用的按键名`enter sapce esc`

![image-20210708161554123](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210708161608.png)

```
 <!-- v-on修饰符 -->
    <input type="text" v-model="username" @keyup.13="haha">
    </div>
    <script src="vue.js"></script>
    <script>
        var app=new Vue({
            el:'#app',
            data:{
                username:''
            },
            methods:{
                haha(){
                    console.log(this.username);
                }
            }
        })
        console.log(app);
    </script>
```

### 过滤器  

如果我想实现一个“字符串首字母大写”的效果，如果页面上很多处需要这样修改的话，费时费力。

![image-20210708162503456](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210708162504.png)

> 引入过滤器filters:{}，过滤器是一个函数，不改变原有变量的情况下，可以将变量过滤后显示出来
>
> 1、定义过滤器
>
> ​	filters:{
>
> ​		自定义函数名（形参变量）｛
>
> ​				return 变量相关操作；
>
> ​		｝
>
> }
>
> 2、使用过滤器
>
> ｛｛变量	|	自定义函数名｝｝

![image-20210708163622810](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210708163623.png)

![image-20210708163741539](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210708163741.png)

### computed计算属性

![image-20210708164630753](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210708164631.png)

实现效果：字符串顺序反转（数组才有颠倒元素顺序的方法哦）

```
 <h1>{{getReverse}}</h1>
 </div>
 <script src="vue.js"></script>
 <script>
     var app=new Vue({
         el:'#app',
         data:{
             str:'Weeknd',
         },
         computed:{
             getReverse(){
            	 return this.str.split('').reverse().join('');
             }
         }
     })
</script>
```

## Vue模板的概念

在vue/cli中，都是用<template>标签构成页面结构的，在组件里面加载各种模板，有以下四种形式，最常用的是第三种。

```
<div id="app">
	<hello></hello>
</div>
<template id="myTemp">
    <div>
        <h1>hello</h1>
        <p>world</p>
    </div>
</template>
<script type="text/html" id="myscript">
    <div>
        <h1>Who</h1>
        <p>are you</p>
    </div>
</script>
<script src="../vue.js"></script>
<script>
    Vue.component('hello', {
        // 1、单行字符串模板
        // template:"<h1>Hello Vue</h1>"
        //2、多行字符串模板（必须要有根标签）
        // template: `<div>
        // <h1>hello Vue</h1>
        // <p>who are you</p>
        // </div>`
        //3、<template>模板，成为hello组件的模板（必须要有根标签）
        // template:"#myTemp",
        //4、<script>模板
        template: "#myscript",
    })
    new Vue({
    	el: "#app"
    })
</script>
```

## Vue组件内数据必须是函数

![image-20210707174756988](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210707174916.png)

**原因**：当data是一个函数时，数据以返回值的形式返回，每复用一次组件，就返回一份新的data,类似给每个组件实例创建了一个数据私有空间（数据之间独立不影响）。

单纯写成对象形式，所有组件共用一份data,一个变化全部变化。 

 **非要在组件内实现共享数据呢？**

```
<script>
//定义一个公共的全局变量
	var data={counter:1}
	new Vue({
		el:#app,
		components:{
			login:{
				data:function(){
					return data;
				}
			}
		}
	})
</script>
```

## 父子组件

### 父传子

1、定义：在一个组件里再定义一个子组件，在父组件模板中引入子组件

2、数据传递：父传子，从上往下

3、在子组件中通过props:[]来获取父组件内的数据（`以自定义属性的方式传递`）

```
<div id="app">
	<myform></myform>
</div>
<script src="../vue.js"></script>
<script>
    new Vue({
    	el:"#app",
        components:{
            myform:{
                template:`<form>
                <row str="用户名" type="text"/>
                <row str="密码" type="password"/>
                <row type="button" value="提交"/>
                </form>`,
                components:{
                    row:{
                    //1、获去父组件指定属性 
                    props:["str","type","value"],
                    //2、动态渲染指定属性的值
                    template:`<p>{{str}}<input :type="type" :value="value"/></p>`,
                    }
                }
            }
        }
    })
</script>
```

### 子传父

> 通过在子组件自定义事件，触发事件传参数，实现子组件传数据给父组件
>
> this.$emit('自定义事件名称'，参数);

1. 子组件代码

![image-20210709163050193](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210709163054.png)

2. 父组件代码

   `三步走	==》`

![image-20210709164054867](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210709164055.png)

![image-20210709163741935](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210709163742.png)



### 数据传递（对象传组件）

* 数据传递既可以体现在将对象数据传递给组件，也可以将父组件数据传递给子组件。

* 对象中的data不能在组件中使用

* 如何在组件中获取对象中的data,并显示在页面上，四步走

  1. 在data中定义要显示的数据

  2. 在组件中用props:[]数组对象来接受,接受的数据`必须打引号`

  3. 在组件模板里面`渲染`出来

  4. 在页面上用v-bind获取`动态属性`值

     ![image-20210707184657134](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210707184701.png)

## 生命周期

1、什么是生命周期？

​	 生命周期就是一个实例或组件从创建到销毁的过程

2、什么是生命周期函数？

​	生命周期函数就是组件或实例从创建到销毁的各个阶段，允许用户自己写入的代码来执行的函数。

3、 常用的生命周期函数，一般在哪一步发送异步请求

*  `beforeCreate`

* created

* beforeMount

* mounted

* beforeUpdate

* Updated

* beforeDestroy

* destroyed

* `actived` keep-alive专属，组件被激活时调用

* `deactived` keep-alive专属，组件被销毁时调用

> 通常在created、beforeMount、mounted阶段发送异步请求，因为此时data已经创建，可以将服务器端返回的数据进行赋值。

## 构建一个vue项目

1. 安装vue脚手架(已安装npm或yarn)

> npm install -g @vue/cli

2. 测试是否脚手架安装成功，有版本号

> vue -V

3. 在你要创建项目的目录下`cmd`,创建vue项目

> vue create 项目名

4. npm中配置快捷

* 出现多选项，`空格键`代表选中
* `ctrl+c`代表终止操作
* `ctrl+enter`代表赋值

5. 运行项目

   * 将生成的服务器地址复制到地址栏中，或ctrl+单击
   * 在项目名根目录下cmd中运行`npm run serve`,(README文件里有)

   

6、node_modules 文件太大，上传时可以将其删掉，如何恢复？

* 执行`npm i`,自动查找package.json里面的依赖
* 再次安装，可能出现部分插件版本兼容的问题=>手动卸载，重安相应版本的依赖

## Vue Router

### 生成路由

1. 在index.js界面配置路由

   * 引入路由`vue-router`依赖
   * 注册路由成为全局组件
   * 定义路由线路
   * 暴露路由对象
   * 在入口文件main.js中引入路由对象，实例化对象中挂载

2. 编写导航代码

   * 在顶级组件App.vue中使用路由组件标签

     ```
     <template>
       <div id="app">
       <!-- 编写路由导航的html代码,这里的to与路由的path匹配-->
         <div id="nav">
           <router-link to="/">首页</router-link> |
           <router-link to="/login">登录</router-link>|
           <router-link to="/regist">注册</router-link>
         </div>
         <router-view></router-view>
       </div>
     </template>
     ```

     * <router-link>组件：跳转页面，生产a标签

       生成：<a href='#/login'></a>	

       > JS中登录是满足条件的跳转:  location.href='#/login'；
     
     * <router-view>组件：显示当前页面对应组件

### 路由传参

> 实际上是组件之间的参数传递，通过路由来传递，会出现在地址栏中
>
> id的值实际上就是要传递的数据

* 与当前路由对象耦合

1. 在路由中定义参数

   ｛path：‘/new/:id’，component:组件｝

2. 在组件中接收参数

   template:'<div>{{$route.params.id}}</div>>'

* 无耦合（推荐使用，便于移植）

1. 在路由中定义参数

   ｛path：‘/new/:id’，component:组件，props:true｝

2. 在组件中（跳转的目标）接收参数

   props:['id']

3. 在路由组件（跳转的起始）传递数据

<a href="#/new/1"></a>

## axios

axios是一个http请求包，vue官网推荐使用axios进行http请求调用

1. 安装axios(--save是将axios加入依赖列表的意思)

   > npm i axios --save

2. 引入axios

   `import axios from 'axios'`

3. 使用axios

   ```
   //格式
   axios.get('url',`username=${变量1}&pwd=${变量2}&`)
   ```
   
   
   
   ```
   created(){
   	axios.get('http://localhost:8080/api/users.php?act=all').then((res)=>{
   		console.log(res);
   	})	
   
   }
   ```
   
   * 异步请求可以在created生命周期函数里面调用
   * axios请求结果返回一个Promise对象，可以用.then()查看结果
   * 因为是箭头函数，所以this指向不是axios，而是上一级作用域created(){},也就是组件。

![image-20210709144715504](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210709144720.png)

* 返回的Promise对象，数据存在Object.data.data里面，并且是数组

![image-20210709205141791](https://gitee.com/houyanxi/personal-drawing-bed/raw/master/personal-drawing-bed/20210709205146.png)

# Vue项目梳理

## Vue登录注册

> 效果：实现一个vue+vuex+vue-router+axios+node.js+mysql的登录注册应用

1. 首先是vue创建项目
   * 创建时手动选择Babel（ES6编译成浏览器兼容的ES5）、ESLint（语法检测工具）、vuex、vuerouter
   * 手动添加axios模块

2. 确定项目所需的功能，要配置哪些路由跳转、页面组件
   * 首页home.vue               '/'（默认APP.vue显示根路由的页面）
   * 登陆页login.vue		'/login'
   * 注册页register.vue        '/regist'

3. 分别编写项目组件页面、路由配置index.js，在顶级组件App.vue编写路由代码
   * <router-link>跳转至相应路由
   * <view-router>路由出口，显示跳转下相应路由页面

4. 接下来是具体功能，比如登陆功能

   * 登陆页面，获取用户名、密码，v-model绑定username，方便数据操作data(){return{username:''}}
   * 点击按钮触发登陆验证,methods(){js代码},里面可以在对输入框字段进行校验
     * 通过axios向服务器提交数据，调用后台提供的接口（接口地址和请求参数）
     * 返回后验证数据是否成功，alert提示登陆失败还是成功
     * 返回后的数据可以存储到vuex里面，便于权限设置

   * 权限设置，获取state里面的数据，结合<v-if><v-else>，数据存在就显示相应的内容

5. 注册功能

   * 对输入框字段进行校验（return false;阻止代码向后执行，跳出函数）

   * axios向后台提交数据
   * 通过返回数据验证注册成功，成功就跳转登陆页面，location.href='#/login'


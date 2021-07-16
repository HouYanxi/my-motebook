# MVC和MVVM原理

> MVC和MVVM并不是一种框架，而是一种设计思想，便于理解，提升编码效率

## MVC模型

### MVC的含义

* M:Model,数据模型
* V:View,视图（用户界面）
* C:Controller,控制器，用于处理业务逻辑    `修改数据模型，再通过获取DOM重新渲染视图`

### MVC示例

1. 发送消息例子：通过点击按钮，获取输入框的值并显示到用户界面上，更新原来的数据

2. 达到的效果

   ![](E:\GiFCam图片\GIF2.gif)

3. 代码如下：

   ```
   <body>
   	<div id="root"></div>
   </body>
   <script>
   	//数据模型
   	let model={
   		text:"你的是谁",
   	};
   	//视图
   	function render(){
   		document.getElementById("root").innerHTML=`
   		<div>${model.text}</div>
   		<input type="text" class="textinput"/>
   		<button id="btn" onclick="sendText()">发送</button>
   		`;
   	}
   	render();
   	//业务逻辑
   	let newVal="";
   	function sendText(){
   		let val=document.querySelector('.textinput').value;
   		newVal+="<p>"+val+"</p>";
   		model.text=newVal;
   		render();
   	}
   </script>
   ```

## MVVM模型

### MVVM含义

* M:Model,数据模型

* V:View,视图（用户界面）

* VM:ViewModel,视图-数据

  ![image-20200303115428149](C:\Users\AS\AppData\Roaming\Typora\typora-user-images\image-20200303115428149.png)

> Vue中，ViewModel通过监听数据模型的数据变化，再使用指令而非DOM操作更新视图，Vue基于数据的双向绑定来更新视图，只需关注数据的操作维护即可

### MVVM示例

1. 仍是发送消息例子，利用Object.defineProperty(obj,property,{set(){},get(){}});监听对象下属性的变化

2. 核心代码

   ```
   let obj={};
   for(let key in model){
   	obj[`_${key}`]=model[key];//保留原始数据模型，更改都在自定义obj对象里
   	Object.defineProperty(model,key，{
   		set(val){
   			obj[`_${key}`]=val;//监听变化的属性,修改数据
   			render();//将数据渲染到页面
   		},
   		get(){
   			return obj[`_${key}`];//获取更新后的值，否则页面为undefined
   		}
   	})
   }
   
   ```

3.由此可看，主要适用于mvc中大量的DOM操作（页面渲染性能降低，加载速度变慢），有大量数据操作的场景
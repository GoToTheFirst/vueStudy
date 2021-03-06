# vueStudy
### day01
#### 1、
	(1)MVVM 
		M model : 模型数据对象（data） 
		V view : 视图 模板页面
		VM viewModel :  视图模型 （vue实例 即 var vm = new vue()）
	(2)绑定数据 实时更新
		<input type="text" v-model="message"/>
		<p>hello??? {{message}}</p>
#### 1、vue语法介绍
	(1)基本语法： {{内写js代码}}
		<p>{{msg}}</p>	
	(2)强制数据绑定 v-bind:src=""
		<img v-bind:src="imgURL">
		<img :src="imgURL"> 简写方式
	(3)强制事件绑定 传参可以是js变量名，函数必须写在methods:{}里面
		<button type="button" v-on:click="test">强制事件绑定</button>
		<button type="button" @click="test">强制事件绑定简写</button>
		<button type="button" @click="test1(msg)">强制事件绑定-传参</button>
#### 3、计算属性
	(1)computed 放入方法，在页面中使用{{方法名}}来显示计算结果
		computed: {
			fullName1() {
				return this.firstName + " " + this.lastName
			}
		}
	(2)watch 监视属性 用watch配置属性，或者用vm.$watch配置
		watch: {
			// 监视第一个值是否发生变化，发生变化则执行下面函数
			firstName: function(value) { // value是变化过的新值
				// computed watch 中的this都是指 Vue
				this.fullName2 = value + ' ' + this.lastName
			}
		}
	(3)setter getter方法 
		getter方法在需要读取函数内的值时执行 多次读取执行一次，其他在缓存中读取
		setter方法在内容变化时调用
		// 回调函数
		fullName3: {
			// 当需要读取函数内部的值时执行
			get() {
				console.log('get')
				return this.firstName + ' ' + this.lastName
			},
			set(value) {
				console.log('set')
				let names = value.split(' ')
				this.firstName = names[0]
				this.lastName = names[1]
			}
		}
#### 4、class style绑定改变样式
	(1)class绑定 :class="xxx"  <p :class="className"></p>
		· xxx是字符串 data里面设置 className: 'aClass'
		· xxx是对象 aClass为类名 isA是属性值 true/false 在data里面设置 
		<p :class="{aClass: isA, cClass: isC}">class绑定 对象</p>
		· xxx是数组 数组里面是类名
		<p :class="['bClass', 'cClass']">class绑定 数组</p>
	(2)style绑定 改变style值
		<p :style="{color: activeColor, fontSize: fontsize + 'px'} ">
#### 5、条件渲染 频繁切换用 v-show
	(1) v-if v-else 不渲染
		<p v-if="ok">成功了</p>
		<p v-else>失败了</p>
	(2)v-show 渲染出来但是隐藏起来 
		<p v-show="ok">表白成功</p>
		<p v-show="!ok">表白失败</p>
#### 6、
	(1)Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。
		这些被包裹过的方法包括：
		push()
		pop()
		shift()
		unshift()
		splice()
		sort()
		reverse()
	(2)person[index] = {name:'cat', age: 22} 不能被检测 可以用上述方法中的splice()方法代替
	(3)遍历数组   v-for="(p, index) in persons"
		<tr v-for="(p, index) in persons">
			<td>{{p.name}}</td>
			<td>{{p.age}}</td>
			<td><button type="button" @click="delate(index)">删除</button></td>
			<td><button type="button" @click="update(index, {name:'cat',age:22})">更新</button></td>
		</tr>
	(4)遍历对象   v-for="(item, key) in persons[1]"
		<ul>
			<li v-for="(item, key) in persons[1]">
				<!-- item为属性值 key为属性名，特别标志可用key 不同的属性名 -->
				{{item}}-----{{key}}
			</li>
		</ul>
#### 7、排序和搜索
	(1)搜索方法
		filterPersons() {
			// 获取 persons 和 searchName orderType 写在computed中，当数据变化时会自动调用
			const {
				persons,
				searchName,
				orderType
			} = this
			// 搜索过滤
			let filterPersons = persons.filter(p => p.name.indexOf(searchName) >= 0)
			// 排序情况，当点击不同按钮时 设置不同的排序方式
			if (orderType != 0) {
				filterPersons.sort(function(p1, p2) {
					if (orderType == 1) return p1.age - p2.age
					else return p2.age - p1.age
				})
			}
			return filterPersons
		}
#### 8、监听事件
	(1)绑定监听 传入event  @click="test2(123, $event)
		<button type="button" @click="test2(123, $event)">传参数可传入$event获取event</button>
		test2(num, event){
			console.log("参数：  "+num)
			console.log(event.target.innerHTML)
		},
	(2)事件修饰符 @click.stop="test4()" 除去冒泡事件
		<div @click="test3()" style="width: 200px; height: 200px; background-color: red;">
			<div @click.stop="test4()" style="width: 100px; height: 100px; background-color: blue">
			</div>
		</div>
	(3)按键修饰符  @keyup.enter="test5"
		<!-- 阻止默认事件发生 -->
		<a href="http://www.baidu.com" @click.prevent="test6">去百度</a>
	(4)
#### 9、表单数据收集 
	(1)阻止跳转 消除默认行为
		<form action="/xxx" method="post" @submit.prevent="getMsg">
	(2)绑定下拉选项 在select处绑定
		<select v-model="selectCity">
			<option value="">未选择</option>
			<option :value="c.name" v-for="(c, index) in allCity" :key="index" >{{c.name}}</option>
		</select>
	(3)绑定多选 设置为数组 每个都需要绑定
		<span>爱好</span>
		<input type="checkbox" name="hobby" value="baskerball" v-model="hobby" />
		<span>篮球</span>
		<input type="checkbox" name="hobby" value="football" v-model="hobby" />
	(4)绑定单选
		<input type="radio" value="男" v-model="gender" />
		<span>男</span>
		<input type="radio" value="女" v-model="gender" />
		<span>女</span>

### day02
#### 1、生命周期 -- 常用 mounted初始化显示之后  beforeDestroy 销毁前
	(1)mounted 函数初始化显示之后立即执行, 只执行一次
		mounted() {  // 初始化显示之后立即执行, 只执行一次
			// 正常函数this是内部的 箭头函数不传this 向上查找 找到this
			this.timer = setInterval(() => {   // 用this.timer赋值，保存在vm中
				console.log("zhixing")
				this.isShow = !this.isShow
			}, 1000)
		}
	(2)beforeDestroy 在销毁前调用 只执行一次
		beforeDestroy(){  // 在销毁前调用 只执行一次
		clearInterval(this.timer)
		},
	(3)$destroy() 方法销毁vm 可用 this.$destory()
		beforeDestroy(){  // 在销毁前调用 只执行一次
			clearInterval(this.timer)
		},
	(4)全部声明周期  常用 mounted初始化显示之后  beforeDestroy 销毁前
		// 1、初始化 创建前 创建后 显示前 显示后 执行一次
			beforeCreate(){
				console.log('beforeCreate')
			},
			created() {
				console.log('created')
			},
			beforeMount() {
				console.log('beforeMount')
			},
			mounted() {
				console.log("mounted")
			},
		2、更新阶段 更新前 更新后 执行n次
			beforeUpdate() {
				console.log('beforeUpdate')
			},
			updated() {
				console.log('updated')
			},
		3、销毁阶段 销毁前 销毁后
			beforeDestroy(){  // 在销毁前调用 只执行一次
				console.log('beforeDestroy')
			},
			destroyed() {
				console.log('destroyed')
			}
#### 2、过度动画
	(1)动画 消失效果
	1、HTML中
		<div id="test2">
			<button type="button" @click="changeShow">toggle</button>
			<transition name="xxx">
				<p v-show="isShow" >hello</p>
			</transition>
		</div>
	2、css中
		显示时的过度效果
		.yyy-enter-active{
			transition: all 1s;
		}
		隐藏时的过度效果
		.yyy-leave-active{
			transition: all 2s;
		}
		.yyy-enter, .yyy-leave-to{
			opacity: 0;
			transform: translateY(20px);
		}
	(2)放大效果
	1、HTML中
		<div id="test4">
			<button type="button" @click="changeShow">toggle</button>
			<br>
			<transition name="scal">
				<p v-show="isShow" style="display: inline-block;">放大效果</p>
			</transition>
		</div>
	2、css中
		放大缩小效果
		.scal-enter-active{  // 显示效果
			animation: bounce-in 1s;
		}
		.scal-leave-active{  隐藏效果
			animation: bounce-in 1s reverse;
		}
		过度效果 
		@keyframes bounce-in{  
			0%{
				transform: scale(0);
			}
			50%{
				transform: scale(1.5);
			}
			100%{
				transform: scale(1);
			}
		}
#### 3、过滤器 语法：Vue.filter('filterStr', function(){})
	(1)moment 格式化事件参考文档：https://momentjs.bootcss.com/
#### 4、指令
	(1)防止闪现表达式 v-cloak 同时使用css配合
		<p v-cloak >{{msg}}</p>
		[v-cloak] {
			display: none;
		}
	(2)其他指令
		v:text 更新内容 同{{}} 写法一样
		v-html 更新元素的innerhtml
		v-if v-else
		v-show
		v-for
		v-on 可以简写为 @
		v-bind 强制绑定解析表达式 可以简写为 :
		v-model 双向数据绑定
		ref 注册一个唯一标识，在vue中通过 this.$refs.content 获得对象元素
	(3)自定义指令
		1、全局有效
		// 全局写法
		Vue.directive('upper-text', function(el, binding){
			// console.log(binding)
			el.textContent = binding.value.toUpperCase()
		})
		2、局部指令，在vue实例中使用directives
		directives:{
			'lower-text': function(el, binding){
				el.textContent = binding.value.toLowerCase()
			}
		}


### day03
#### 1、语法准备
	(1)Array.prototype.slice.call(lis) 伪数组转数组
		// es6伪数组转数组
		const lis2 = Array.from(lis)
		// es5伪数组转数组
		const lis3 = Array.prototype.slice.call(lis) //使slice成为lis的方法并回调
	(2)node.nodeType 得到节点类型
		const eleNode = document.getElementById('test')
		const attrNode = eleNode.getAttributeNode('id')
		const textNode = eleNode.firstChild
		// nodeType为1 nodeName为DIV
		console.log(eleNode.nodeType, eleNode.nodeName)
		// nodeType为2 nodeName为id
		console.log(attrNode.nodeType, attrNode.nodeName)
		// nodeType为3 nodeName为#text
		console.log(textNode.nodeType, textNode.nodeName)
	(3)Object.defineProperty(obj, 'fullName', descriptor）
		属性描述符
		1，数据描述符
		configurable 该属性的描述符才能够被改变,即是否可以重新定义
		enumerable 是否可以枚举 该属性的 enumerable 键值为 true 时，该属性才会出现在对象的枚举属性中。
		value 该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）
		writable value是否可变
		2，访问描述符
		get回调函数，根据其他属性动态计算得到当前值
		set回调函数，监视当前属性值变化，更新其他相关值value
		es6支持，ie8不支持ie6，所以ie8不支持vue
		Object.defineProperty(obj, 'fullName', {
			get() {
				return this.firstName + '-' + this.lastName
			},
			set(value) {
				console.log('set检测到值的变化-----' + value)
				const names = value.split('-')
				this.firstName = names[0]
				this.lastName = names[1]
			}
		})
	(4)Object.keys(obj) 返回所有的属性名 enumerable默认为false
		const keys = Object.keys(obj)
		console.log(keys)
	(5)DocumentFragment 为document整个更改内容
		let fragment = document.createDocumentFragment()
		// 1、创建对象
		let fragment = document.createDocumentFragment()
		let ul = document.getElementById('fragment')
		let child
		// 将li添加到fragment中，同时去掉原来内容
		while (child = ul.firstChild) {
			fragment.appendChild(child)
		}
		// console.log(fragment)
		// 2、更改node中的内容
		Array.prototype.slice.call(fragment.childNodes).forEach(node => {
			console.log(node.nodeType)
			// 判断node类型是否为节点，若是节点则更改内容
			if (node.nodeType == 1) {
				node.textContent = '这是测试'
			}
		})
		// 3、更改后添加回原来位置
		ul.appendChild(fragment)
### day04
#### 1、导入导出，变量常量函数和类都可导出， 引入时都需要 ./
	(1)默认导出，导入时重命名
		// 默认导出
		export default fun
		import def from './01export.js'
		// 全部导入,访问时用all的属性
		export let num = 20
		import * as all from './01export.js'
		console.log(all.name)
#### 2、webpack
	(1)webpack是一个前端模块化管理工具，
		grunt/grulp更强调的是前端流程自动化，webpack更强调模块化管理压缩合并、预处理是附加功能
	(2)webpack可以打包commenJS语法和es6语法的文件，但是一个文件中只能用一种方式导出
		导出语句：webpack .\src\main.js .\dist\boundle.js
		commenJS模块化规范
			导出方式module.exports = {add, mul}
			导入方式：const {add, mul} = require('./mathUtil.js')
		es6语法模块化规范
			es6导出方式：export const name = 'xiaoming'
			es6导入方式：import {name, age, height} from './info.js'
		配置：
		module.exports = {
			entry: './src/main.js',
			// output必须是绝对路径，用node中的方法，const path = require('path')
			output: {
				path: path.resolve(__dirname, 'dist'),
				filename: 'bundle.js'
			},
		}
	(3)npm init初始化构建，生成package.json 其中scripts中映射执行时运行脚本
		scripts中配置脚本命令 优先使用本能地，本地没有使用全局
		"scripts": {
		  "test": "echo \"Error: no test specified\" && exit 1",
			// 当执行npm run build时自动执行webpack 
			"build": "webpack"
		},
	(4)安装"style-loader", "css-loader" 在main.js中引入css文件，参考webpack官网配置
		注意 webpack3.6.0 需要 css-loader2.0.2，版本过高不能打包会提醒
		npm uninstall css-loader（卸载）
		npm install css-loader@2.0.2 --save-dev 本地局部安装，开发时使用
		配置：
		module.exports = {
			module: {
				rules: [{
					test: /\.css$/i,
					// css-loader 只负责加载css
					// style-loader 只负责把样式添加到DOM中
					// loader读取时从右向左
					use: ['style-loader', "css-loader"],
				}, ],
			},
		}
	(5)url-loader  转化url
		配置：
		{
			test: /\.(png|jpg|gif)$/i,
			use: [{
				loader: 'url-loader',
				options: {
					// 当图片大小大于limit时 会用file-loader进行打包,配置打包后后寻找路径
					// 当图片大小小于limit时 会用url-loader打包
					limit: 8196,
					// 配置图片命名和位置 在img文件夹下 名字+8位哈希数字+扩展名
					name: 'img/[name].[hash:8].[ext]'
				}
			}]
		}
	(6)babel-loader  将es6转为es5
		配置：
		{
			test: /\.m?js$/,
			// 排除调以下文件夹中的文件,即node_modules和bower_components文件夹下的文件不进行解析
			exclude: /(node_modules|bower_components)/,
			use: {
				// babel 将es6转为es5,例如es5中没有const
				loader: 'babel-loader',
				options: {
					presets: ['es2015']
				}
			}
		}





/*
#### 1、
	(1)
	(2)
	(3)
	(4)
	(5)
*/

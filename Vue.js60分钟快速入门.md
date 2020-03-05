# [Vue.js&mdash;&mdash;60分钟快速入门](https://www.cnblogs.com/keepfool/p/5619070.html)

# Vue.js介绍

[Vue.js](https://github.com/vuejs/vue)是当下很火的一个JavaScript MVVM库，它是以数据驱动和组件化的思想构建的。相比于Angular.js，Vue.js提供了更加简洁、更易于理解的API，使得我们能够快速地上手并使用Vue.js。

如果你之前已经习惯了用jQuery操作DOM，学习Vue.js时请先抛开手动操作DOM的思维，因为Vue.js是数据驱动的，你无需手动操作DOM。它通过一些特殊的HTML语法，将DOM和数据绑定起来。一旦你创建了绑定，DOM将和数据保持同步，每当变更了数据，DOM也会相应地更新。

当然了，在使用Vue.js时，你也可以结合其他库一起使用，比如jQuery。

本文的Demo和源代码已放到GitHub，如果您觉得本篇内容不错，请点个赞，或在GitHub上加个星星！

[v-for Demo](https://keepfool.github.io/vue-tutorials/01.GettingStarted/v-for.html)

[v-bind Demo](https://keepfool.github.io/vue-tutorials/01.GettingStarted/v-bind.html)

[Page Demo](https://keepfool.github.io/vue-tutorials/01.GettingStarted/simple-demo.html)

[GitHub Source](https://github.com/keepfool/vue-tutorials)

## MVVM模式

下图不仅概括了MVVM模式（Model-View-ViewModel），还描述了在Vue.js中ViewModel是如何和View以及Model进行交互的。

![MVVM](http://cn.vuejs.org/images/mvvm.png)

**ViewModel是Vue.js的核心，它是一个Vue实例。**Vue实例是作用于某一个HTML元素上的，这个元素可以是HTML的body元素，也可以是指定了id的某个元素。

当创建了ViewModel后，双向绑定是如何达成的呢？

首先，我们将上图中的DOM Listeners和Data Bindings看作两个工具，它们是实现双向绑定的关键。
从View侧看，ViewModel中的DOM Listeners工具会帮我们监测页面上DOM元素的变化，如果有变化，则更改Model中的数据；
从Model侧看，当我们更新Model中的数据时，Data Bindings工具会帮我们更新页面中的DOM元素。

## Hello World示例

了解一门语言，或者学习一门新技术，编写Hello World示例是我们的必经之路。
这段代码在画面上输出"Hello World!"。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>

	<body>
		<!--这是我们的View-->
		<div id="app">
			{{ message }}
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		// 这是我们的Model
		var exampleData = {
			message: 'Hello World!'
		}

		// 创建一个 Vue 实例或 "ViewModel"
		// 它连接 View 与 Model
		new Vue({
			el: '#app',
			data: exampleData
		})
	</script>
</html>
```

使用Vue的过程就是定义MVVM各个组成部分的过程的过程。

1. **定义View**
2. **定义Model**
3. **创建一个Vue实例或"ViewModel"，它用于连接View和Model**

在创建Vue实例时，需要传入一个**选项对象**，选项对象可以包含数据、挂载元素、方法、模生命周期钩子等等。

在这个示例中:

**选项对象**的**el属性**指向**View**，`el: '#app'`表示该Vue实例将挂载到`<div id="app">...</div>`这个元素；

**data属性**指向**Model**，`data: exampleData`表示我们的Model是exampleData对象。



Vue.js有多种`数据绑定`的语法，最基础的形式是`文本插值`，使用`一对大括号`语法，在运行时`{{ message }}`会被数据对象的message属性替换，所以页面上会输出"Hello World!"。



## 双向绑定示例

MVVM模式本身是实现了双向绑定的，在Vue.js中可以使用 **v-model** 指令在表单元素上创建双向数据绑定。

```vue
<!--这是我们的View-->
<div id="app">
	<p>{{ message }}</p>
	<input type="text" v-model="message"/>
</div>
```


将message绑定到文本框，当更改文本框的值时，`<p>{{ message }}</p>` 中的内容也会被更新。

[![1](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065311062-227248599.gif)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065309718-2118753556.gif)

反过来，如果改变message的值，文本框的值也会被更新，我们可以在Chrome控制台进行尝试。

[![2](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065313046-1157492348.gif)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065312015-708112076.gif)

Vue实例的data属性指向exampleData，它是一个引用类型，改变了exampleData对象的属性，同时也会影响Vue实例的data属性。

# Vue.js的常用指令

Vue.js的指令是以**v-**开头的，它们**作用于HTML元素**，指令提供了一些特殊的特性，将指令绑定在元素上时，指令会为绑定的目标元素添加一些特殊的行为，我们可以将指令看作特殊的HTML特性（attribute）。

Vue.js提供了一些常用的内置指令，接下来我们将介绍以下几个内置指令：

- v-if指令 

- v-show指令 

- v-else指令 

- v-for指令 

- v-bind指令 

- v-on指令 

Vue.js具有良好的扩展性，我们也可以开发一些自定义的指令，后面的文章会介绍自定义指令。

## v-if指令

`v-if`是条件渲染指令，它根据表达式的真假来删除和插入元素，它的基本语法如下：

    v-if="*expression*"

**expression是一个返回bool值的表达式**，表达式可以是一个bool属性，也可以是一个返回bool的运算式。例如：

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h1>Hello, Vue.js!</h1>
			<h1 v-if="yes">Yes!</h1>
			<h1 v-if="no">No!</h1>
			<h1 v-if="age >= 25">Age: {{ age }}</h1>
			<h1 v-if="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		
		var vm = new Vue({
			el: '#app',
			data: {
				yes: true,
				no: false,
				age: 28,
				name: 'keepfool'
			}
		})
	</script>
</html>
```

**注意：**yes, no, age, name这4个变量都来源于Vue实例选项对象的data属性。

[![image](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065314546-1211050706.png)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065313781-1005102718.png)

这段代码使用了4个表达式：

- 数据的`yes`属性为true，所以"Yes!"会被输出； 

- 数据的`no`属性为false，所以"No!"不会被输出； 

- 运算式`age >= 25`返回true，所以"Age: 28"会被输出； 

- 运算式`name.indexOf('jack') >= 0`返回false，所以"Name: keepfool"不会被输出。 

**注意：**v-if指令是根据条件表达式的值来执行**元素的插入或者删除行为。**

这一点可以从渲染的HTML源代码看出来，面上只渲染了3个<h1>元素，`v-if`值为false的<h1>元素没有渲染到HTML。

[![image](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065316265-161676773.png)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065315515-1776406933.png)

为了再次验证这一点，可以在Chrome控制台更改age属性，使得表达式`age >= 25`的值为false，可以看到

`<h1>Age: 28</h1>`元素被删除了。

[![3](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065319406-1162614113.gif)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065317577-1183006666.gif)

age是定义在选项对象的data属性中的，为什么Vue实例可以直接访问它呢？
这是因为**每个Vue实例都会代理其选项对象里的data属性。**

## v-show指令

`v-show`也是条件渲染指令，和v-if指令不同的是，使用`v-show`指令的元素始终会被渲染到HTML，它只是简单地为元素设置CSS的style属性。

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h1>Hello, Vue.js!</h1>
			<h1 v-show="yes">Yes!</h1>
			<h1 v-show="no">No!</h1>
			<h1 v-show="age >= 25">Age: {{ age }}</h1>
			<h1 v-show="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		
		var vm = new Vue({
			el: '#app',
			data: {
				yes: true,
				no: false,
				age: 28,
				name: 'keepfool'
			}
		})
	</script>
</html>
```

[![image](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065321359-1780927154.png)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065320577-322007545.png)

在Chrome控制台更改age属性，使得表达式`age >= 25`的值为false，可以看到`<h1>Age: 24</h1>`元素被设置了style="display:none"样式。

[![4](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065324343-1072339483.gif)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065322656-1570128969.gif)

## v-else指令

可以用`v-else`指令为`v-if`或`v-show`添加一个“else块”。`v-else`元素必须立即跟在`v-if`或`v-show`元素的后面——否则它不能被识别。

```vue
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h1 v-if="age >= 25">Age: {{ age }}</h1>
			<h1 v-else>Name: {{ name }}</h1>
			<h1>---------------------分割线---------------------</h1>
			<h1 v-show="name.indexOf('keep') >= 0">Name: {{ name }}</h1>
			<h1 v-else>Sex: {{ sex }}</h1>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				age: 28,
				name: 'keepfool',
				sex: 'Male'
			}
		})
	</script>
</html>
```

`v-else`元素是否渲染在HTML中，取决于前面使用的是`v-if`还是`v-show`指令。
这段代码中`v-if`为true，后面的`v-else`不会渲染到HTML；`v-show`为tue，但是后面的`v-else`仍然渲染到HTML了。

[![image](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065326546-136301763.png)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065325609-935780863.png)

## v-for指令

`v-for`指令基于一个数组渲染一个列表，它和JavaScript的遍历语法相似：

    v-for="item in items"

items是一个数组，item是当前被遍历的数组元素。

```vue
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
		<link rel="stylesheet" href="styles/demo.css" />
	</head>

	<body>
		<div id="app">
			<table>
				<thead>
					<tr>
						<th>Name</th>
						<th>Age</th>
						<th>Sex</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="person in people">
						<td>{{ person.name  }}</td>
						<td>{{ person.age  }}</td>
						<td>{{ person.sex  }}</td>
					</tr>
				</tbody>
			</table>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				people: [{
					name: 'Jack',
					age: 30,
					sex: 'Male'
				}, {
					name: 'Bill',
					age: 26,
					sex: 'Male'
				}, {
					name: 'Tracy',
					age: 22,
					sex: 'Female'
				}, {
					name: 'Chris',
					age: 36,
					sex: 'Male'
				}]
			}
		})
	</script>

</html>
```

我们在选项对象的data属性中定义了一个people数组，然后在#app元素内使用`v-for`遍历people数组，输出每个person对象的姓名、年龄和性别。

[![image](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065327859-2129607250.png)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065327202-406012811.png)

[View Demo](https://keepfool.github.io/vue-tutorials/01.GettingStarted/v-for.html)

## v-bind指令

`v-bind`指令可以在其名称后面带一个参数，中间放一个冒号隔开，这个参数通常是HTML元素的特性（attribute），例如：`v-bind:class`

    v-bind:*argument*="*expression*"

expression中，可以写合法的JS表达式

下面这段代码构建了一个简单的分页条，v-bind指令作用于元素的class特性上。
这个指令包含一个表达式，表达式的含义是：高亮当前页。

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<link rel="stylesheet" href="styles/demo.css" />
	</head>
	<body>
		<div id="app">
			<ul class="pagination">
				<li v-for="n in pageCount">
					<a href="javascripit:void(0)" v-bind:class="activeNumber === n + 1 ? 'active' : ''">{{ n + 1 }}</a>
				</li>
			</ul>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				activeNumber: 1,
				pageCount: 10
			}
		})
	</script>
</html>
```

注意`v-for="n in pageCount"`这行代码，pageCount是一个整数，遍历时*n从0开始，然后遍历到pageCount –1*结束。

[![image](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065329281-1176325976.png)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065328499-156638431.png)

[View Demo](https://keepfool.github.io/vue-tutorials/01.GettingStarted/v-bind.html)

## v-on指令

`v-on`指令用于给**监听DOM事件**，它的用语法和v-bind是类似的，例如监听<a>元素的点击事件：

    <a v-on:click="doSomething">

有两种形式调用方法：**绑定一个方法（让事件指向方法的引用），或者使用内联语句。
**Greet按钮将它的单击事件直接绑定到greet()方法，而Hi按钮则是调用say()方法。

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<p><input type="text" v-model="message"></p>
			<p>
				<!--click事件直接绑定一个方法-->
				<button v-on:click="greet">Greet</button>
			</p>
			<p>
				<!--click事件使用内联语句-->
				<button v-on:click="say('Hi')">Hi</button>
			</p>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				message: 'Hello, Vue.js!'
			},
			// 在 `methods` 对象中定义方法
			methods: {
				greet: function() {
					// // 方法内 `this` 指向 vm
					alert(this.message)
				},
				say: function(msg) {
					alert(msg)
				}
			}
		})
	</script>
</html>
```

[![5](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065331124-2002999272.gif)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065330109-1825840680.gif)

## v-bind和v-on的缩写

Vue.js为最常用的两个指令`v-bind`和`v-on`提供了缩写方式。**v-bind指令可以缩写为一个冒号，v-on指令可以缩写为@符号。**

```vue
<!--完整语法-->
<a href="javascripit:void(0)" v-bind:class="activeNumber === n + 1 ? 'active' : ''">{{ n + 1 }}</a>
<!--缩写语法-->
<a href="javascripit:void(0)" :class="activeNumber=== n + 1 ? 'active' : ''">{{ n + 1 }}</a>

<!--完整语法-->
<button v-on:click="greet">Greet</button>
<!--缩写语法-->
<button @click="greet">Greet</button>
```

## v-cloak

```vue
<!-- 使用 v-cloak 能够解决 插值表达式闪烁的问题 -->
<p v-cloak>++++++++ {{ msg }} ----------</p>
```

## v-text

默认 v-text 是没有闪烁问题的 
v-text 会覆盖元素中原本的内容，但是 插值表达式  只会替换自己的这个占位符，不会把 整个元素的内容清空 

## v-html



## v-on 事件修饰符

- .stop       阻止冒泡

- .prevent    阻止默认事件

- .capture    添加事件侦听器时使用事件捕获模式

- .self       只当事件在该元素本身（比如不是子元素）触发时触发回调

- .once       事件只触发一次

  

`.stop 和 .self 的区别:`

`.self 只会阻止自己身上冒泡行为的触发，并不会真正阻止 冒泡的行为`



## v-model



## v-for

1. 循环普通数组

```json
list: [1, 2, 3, 4, 5, 6]
```

```html
<p v-for="(item, i) in list">索引值：{{i}} --- 每一项：{{item}}</p>
```

2. 循环对象数组

```json
list: [
  { id: 1, name: 'zs1' },
  { id: 2, name: 'zs2' },
  { id: 3, name: 'zs3' },
  { id: 4, name: 'zs4' }
]
```

```html
<p v-for="(user, i) in list">Id：{{ user.id }} --名字：{{ user.name }} --- 索引：{{i}}</p>
```

3. 循环对象中的属性

```json
user: {
  id: 1,
  name: '托尼·屎大颗',
  gender: '男'
}
```

```html
<p v-for="(val, key, i) in user">值是： {{ val }} --- 键是： {{key}} -- 索引： {{i}}</p>
```

4. 循环数字

```html
<p v-for="count in 10">这是第 {{ count }} 次循环</p>
```

> 2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。
>
> key 属性只能使用 number 或者 string

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。

为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。



## v-if对比v-show

v-if 的特点：每次都会重新删除或创建元素
v-show 的特点： 每次不会重新进行DOM的删除和创建操作，只是切换了元素的 display:none 样式

> 一般来说，v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。
>
> 因此，如果需要频繁切换 `v-show `较好，如果在运行时条件不大可能改变 ` v-if` 较好。



##

# 综合示例

现在我们已经介绍了一些Vue.js的基础知识了，结合以上知识我们可以来做个小Demo。

```vue
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
		<link rel="stylesheet" href="styles/demo.css" />
	</head>

	<body>
		<div id="app">

			<fieldset>
				<legend>
					Create New Person
				</legend>
				<div class="form-group">
					<label>Name:</label>
					<input type="text" v-model="newPerson.name"/>
				</div>
				<div class="form-group">
					<label>Age:</label>
					<input type="text" v-model="newPerson.age"/>
				</div>
				<div class="form-group">
					<label>Sex:</label>
					<select v-model="newPerson.sex">
					<option value="Male">Male</option>
					<option value="Female">Female</option>
				</select>
				</div>
				<div class="form-group">
					<label></label>
					<button @click="createPerson">Create</button>
				</div>
		</fieldset>
		<table>
			<thead>
				<tr>
					<th>Name</th>
					<th>Age</th>
					<th>Sex</th>
					<th>Delete</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="person in people">
					<td>{{ person.name }}</td>
					<td>{{ person.age }}</td>
					<td>{{ person.sex }}</td>
					<td :class="'text-center'"><button @click="deletePerson($index)">Delete</button></td>
				</tr>
			</tbody>
		</table>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				newPerson: {
					name: '',
					age: 0,
					sex: 'Male'
				},
				people: [{
					name: 'Jack',
					age: 30,
					sex: 'Male'
				}, {
					name: 'Bill',
					age: 26,
					sex: 'Male'
				}, {
					name: 'Tracy',
					age: 22,
					sex: 'Female'
				}, {
					name: 'Chris',
					age: 36,
					sex: 'Male'
				}]
			},
			methods:{
				createPerson: function(){
					this.people.push(this.newPerson);
					// 添加完newPerson对象后，重置newPerson对象
					this.newPerson = {name: '', age: 0, sex: 'Male'}
				},
				deletePerson: function(index){
					// 删一个数组元素
					this.people.splice(index,1);
				}
			}
		})
	</script>

</html>
```

[![6](https://images2015.cnblogs.com/blog/341820/201606/341820-20160627065333452-768092605.gif)](http://images2015.cnblogs.com/blog/341820/201606/341820-20160627065332359-1941638431.gif)

在我的GitHub Pages查看该Demo：

[View Demo](https://keepfool.github.io/vue-tutorials/01.GettingStarted/simple-demo.html)

# 总结

本文简单介绍了Vue.js的MVVM模型和它的双向绑定机制，然后以一个Hello World示例开始了我们的Vue.js之旅，接着我们了解了几个比较常用的指令，最后根据这些知识我们构建了一个简单的示例。
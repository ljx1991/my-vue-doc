Vue.js&mdash;&mdash;基于$.ajax实现数据的跨域增删查改 - keepfool - 博客园
        var currentBlogId = 99820;
        var currentBlogApp = 'keepfool';
        var cb_enable_mathjax = false;
        var isLogined = false;
    

关注我

[![返回主页](/skins/custom/images/logo.gif)](https://www.cnblogs.com/keepfool/)
# [Vue.js&mdash;&mdash;基于$.ajax实现数据的跨域增删查改](https://www.cnblogs.com/keepfool/p/5648674.html)

# 概述

之前我们学习了Vue.js的一些基础知识，以及如何开发一个组件，然而那些示例的数据都是local的。
在实际的应用中，几乎90%的数据是来源于服务端的，前端和服务端之间的数据交互一般是通过ajax请求来完成的。

说起ajax请求，大家第一时间会想到jQuery。除了拥有强大的DOM处理能力，jQuery提供了较丰富的ajax处理方法，它不仅支持基于XMLHttpRequest的ajax请求，也能处理跨域的JSONP请求。

之前有读者问我，Vue.js能结合其他库一起用吗？答案当然是肯定的，Vue.js和jQuery一起使用基本没有冲突，尽可放心大胆地使用。

本文的主要内容如下：

- 同源策略和跨域概念  
- 跨域资源共享  
- JSONP概念  
- REST Web Services  
- 基于$.ajax实现跨域GET请求  
- 基于$.ajax实现JSONP请求  
- 基于$.ajax实现跨域POST请求  
- 基于$.ajax实现跨域PUT请求  
- 基于$.ajax实现跨域DELETE请求

本文的服务端程序和客户端程序是部署在不同服务器上的，本文所有示例请求都是跨域的。
源代码已放到GitHub，如果您觉得本篇内容不错，请点个赞，或在GitHub上加个星星！

[CORS GET](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-cors-get.html)

[JSONP GET](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-jsonp.html)

[CORS POST](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-cors-post.html)

[CORS PUT](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-cors-put.html)

[完整CURD示例](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-cors-delete.html)[GitHub Source](https://github.com/keepfool/vue-tutorials)

# 基础概念

在进入本文正题之前，我们需要先了解一些基础概念（如果你已经对这些基础有所了解，可跳过此段落）。

## 同源策略和跨域概念

**同源策略（Same-orgin policy）**限制了一个源（orgin）中加载脚本或脚本与来自其他源（orgin）中资源的交互方式。
如果两个页面拥有相同的**协议**（protocol），**端口**（port）和**主机**（host），那么这两个页面就属于同一个源（orgin）。

同源之外的请求都可以称之为跨域请求。
下表给出了相对[http://store.company.com/dir/page.html](http://store.company.com/dir/page.html)同源检测的示例： 

| URL                                             | 结果 | 原因       |
| ----------------------------------------------- | ---- | ---------- |
| http://store.company.com/dir2/other.html        | 成功 |            |
| http://store.company.com/dir/inner/another.html | 成功 |            |
| https://store.company.com/secure.html           | 失败 | 协议不同   |
| http://store.company.com:81/dir/etc.html        | 失败 | 端口不同   |
| http://news.company.com/dir/other.html          | 失败 | 主机名不同 |

**我们可以简单粗暴地理解为同一站点下的资源相互访问都是同源的，跨站点的资源访问都是跨域的。**

## 跨域资源共享

**跨域资源共享（CORS）**是一份浏览器技术的规范，提供了Web服务器从不同网域传来沙盒脚本的方法，以避开浏览器的同源策略，是JSONP模式的现代版。与JSONP不同，CORS除了支持GET方法以外，还支持其他HTTP方法。用CORS可以让网页设计师用一般的XMLHTTPRequest，这种方式的错误处理比JSONP要来的好。另一方面，JSONP可以在不支持CORS的老旧浏览器上运作，现代的浏览器都支持CORS。  

在网页[http://caniuse.com/#feat=cors](http://caniuse.com/#feat=cors)上列出了主流浏览器对CORS的支持情况，包含PC端和移动端的浏览器。  

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063337264-1168569844.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063336046-1167128707.png)

## JSONP概念

由于同源策略，一般来说不允许JavaScript跨域访问其他服务器的页面对象，但是HTML的<script>元素是一个例外。利用 <script> 元素的这个开放策略，网页可以得到从其他来源动态产生的 JSON资料，而这种使用模式就是所谓的 JSONP。**用 JSONP 抓到的资料并不是 JSON，而是任意的JavaScript，用 JavaScript 直译器执行而不是用 JSON 解析器解析。**

## REST Web Services简介

HTTP协议是Web的标准之一，HTTP协议包含一些标准的操作方法，例如：`GET, POST, PUT, Delete`等，用这些方法能够实现对Web资源的CURD操作，下表列出了这些方法的操作定义。

| HTTP方法 | 资源处理           | 说明                                                         |
| -------- | ------------------ | ------------------------------------------------------------ |
| GET      | 读取资源（Read）   | **获取**被请求URI（Request-URI）指定的信息（以实体的格式）。 |
| POST     | 创建资源（Create） | 在服务器上**创建**一个新的资源，并返回新资源的URI。          |
| PUT      | 更新资源（Update） | 指定URI资源存在则**更新**资源，指定URI资源不存在则**创建**一个新资源。 |
| DELETE   | 删除资源（Delete） | **删除**请求URI指定的资源。                                  |

REST的最佳应用场景是公开服务，使用HTTP并遵循REST原则的Web服务，我们称之为RESTful Web Service。RESTful Web Service从以下三个方面进行资源定义：  
- 直观简短的资源地址：URI，比如：http://example.com/resources/  
- 传输的资源：Web Service接受与返回的互联网媒体类型，比如JSON，XML等  
- 对资源的操作：Web Service在该资源上所支持的一系列请求方法（比如：GET，POST，PUT或Delete）

下图展示了RESTful Web Service的执行流程

[![image (1)](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063338842-1111707533.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063338202-2124262622.png)

本文的服务端程序是基于ASP.NET Web API构建的。
在了解了这些基础知识后，我们就分别来构建服务端程序和客户端程序吧。

# 服务端环境准备

如果您是前端开发人员，并且未接触过ASP.NET Web API，可以跳过此段落。

## 新建Web API应用程序

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063340796-948196980.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063340171-1144295924.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063342124-519227366.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063341483-103277154.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063343858-796405481.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063342655-501369181.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063345311-1658803848.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063344608-1007526608.png)

## 添加Model, Controller

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063346311-1584194037.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063345827-1806160700.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063347624-1207183107.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063346983-321771304.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063348561-1328175388.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063348124-286822031.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063349780-1944718525.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063349077-1261208860.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063351046-484812217.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063350374-505572545.png)

## 初始化数据库

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063352374-813681000.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063351874-129248163.png)

分别执行以下3个命令：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063353577-1676338239.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063353030-1489493401.png)

打开VS的Server Explorer，选择刚创建好的数据库，右键New Query，执行以下sql语句：

    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (1, N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders', N'Sales Representative', N'Obere Str. 57', N'Berlin', NULL, N'12209', N'Germany', N'030-0074321', N'030-0076545', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (2, N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo', N'Owner', N'Avda. de la Constitución 2222', N'México D.F.', NULL, N'05021', N'Mexico', N'(5) 555-4729', N'(5) 555-3745', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (3, N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno', N'Owner', N'Mataderos  2312', N'México D.F.', NULL, N'05023', N'Mexico', N'(5) 555-3932', NULL, 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (4, N'AROUT', N'Around the Horn', N'Thomas Hardy', N'Sales Representative', N'120 Hanover Sq.', N'London', NULL, N'WA1 1DP', N'UK', N'(171) 555-7788', N'(171) 555-6750', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (5, N'BERGS', N'Berglunds snabbköp', N'Christina Berglund', N'Order Administrator', N'Berguvsvägen  8', N'Luleå', NULL, N'S-958 22', N'Sweden', N'0921-12 34 65', N'0921-12 34 67', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (6, N'BLAUS', N'Blauer See Delikatessen', N'Hanna Moos', N'Sales Representative', N'Forsterstr. 57', N'Mannheim', NULL, N'68306', N'Germany', N'0621-08460', N'0621-08924', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (7, N'BLONP', N'Blondesddsl père et fils', N'Frédérique Citeaux', N'Marketing Manager', N'24, place Kléber', N'Strasbourg', NULL, N'67000', N'France', N'88.60.15.31', N'88.60.15.32', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (8, N'BOLID', N'Bólido Comidas preparadas', N'Martín Sommer', N'Owner', N'C/ Araquil, 67', N'Madrid', NULL, N'28023', N'Spain', N'(91) 555 22 82', N'(91) 555 91 99', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (9, N'BONAP', N'Bon app''', N'Laurence Lebihan', N'Owner', N'12, rue des Bouchers', N'Marseille', NULL, N'13008', N'France', N'91.24.45.40', N'91.24.45.41', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (10, N'BOTTM', N'Bottom-Dollar Markets', N'Elizabeth Lincoln', N'Accounting Manager', N'23 Tsawassen Blvd.', N'Tsawassen', N'BC', N'T2F 8M4', N'Canada', N'(604) 555-4729', N'(604) 555-3745', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (11, N'BSBEV', N'B''s Beverages', N'Victoria Ashworth', N'Sales Representative', N'Fauntleroy Circus', N'London', NULL, N'EC2 5NT', N'UK', N'(171) 555-1212', NULL, 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (12, N'CACTU', N'Cactus Comidas para llevar', N'Patricio Simpson', N'Sales Agent', N'Cerrito 333', N'Buenos Aires', NULL, N'1010', N'Argentina', N'(1) 135-5555', N'(1) 135-4892', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (13, N'CENTC', N'Centro comercial Moctezuma', N'Francisco Chang', N'Marketing Manager', N'Sierras de Granada 9993', N'México D.F.', NULL, N'05022', N'Mexico', N'(5) 555-3392', N'(5) 555-7293', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (14, N'CHOPS', N'Chop-suey Chinese', N'Yang Wang', N'Owner', N'Hauptstr. 29', N'Bern', NULL, N'3012', N'Switzerland', N'0452-076545', NULL, 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (15, N'COMMI', N'Comércio Mineiro', N'Pedro Afonso', N'Sales Associate', N'Av. dos Lusíadas, 23', N'Sao Paulo', N'SP', N'05432-043', N'Brazil', N'(11) 555-7647', NULL, 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (16, N'CONSH', N'Consolidated Holdings', N'Elizabeth Brown', N'Sales Representative', N'Berkeley Gardens 12  Brewery', N'London', NULL, N'WX1 6LT', N'UK', N'(171) 555-2282', N'(171) 555-9199', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (17, N'DRACD', N'Drachenblut Delikatessen', N'Sven Ottlieb', N'Order Administrator', N'Walserweg 21', N'Aachen', NULL, N'52066', N'Germany', N'0241-039123', N'0241-059428', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (18, N'DUMON', N'Du monde entier', N'Janine Labrune', N'Owner', N'67, rue des Cinquante Otages', N'Nantes', NULL, N'44000', N'France', N'40.67.88.88', N'40.67.89.89', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (19, N'EASTC', N'Eastern Connection', N'Ann Devon', N'Sales Agent', N'35 King George', N'London', NULL, N'WX3 6FW', N'UK', N'(171) 555-0297', N'(171) 555-3373', 1)
    INSERT [dbo].[Customers] ([CustomerId], [CustomerCode], [CompanyName], [ContactName], [ContactTitle], [Address], [City], [Region], [PostalCode], [Country], [Phone], [Fax], [CustomerType]) VALUES (20, N'ERNSH', N'Ernst Handel', N'Roland Mendel', N'Sales Manager', N'Kirchgasse 6', N'Graz', NULL, N'8010', N'Austria', N'7675-3425', N'7675-3426', 1)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063354827-1763893568.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063354171-437177896.png)

 

## 让Web API以CamelCase输出JSON

C#偏向于PascalCase的命名规范，而JavaScript则偏向于camelCase的命名规范，为了让JavaScript接收到的JSON数据是camelCase的，在Global.asax文件中添加以下几行代码：

    var formatters = GlobalConfiguration.Configuration.Formatters;
    var jsonFormatter = formatters.JsonFormatter;
    var settings = jsonFormatter.SerializerSettings;
    settings.Formatting = Formatting.Indented;
    settings.ContractResolver = new CamelCasePropertyNamesContractResolver();

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063355842-1316529719.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063355311-1795216393.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063356905-1863987657.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063356405-1864522356.png)

可以在以下地址访问构建好的Web API：

[http://211.149.193.19:8080/Help](http://211.149.193.19:8080/Help)

访问Customers数据：

[http://211.149.193.19:8080/api/Customers](http://211.149.193.19:8080/api/Customers)

# 创建组件和AjaxHelper

本文的示例仍然是表格组件的CURD，只不过这次咱们的数据是从服务端获取的。
在实现数据的CURD之前，我们先准备好一些组件和Ajax帮助方法。

## 创建和注册simple-grid组件

simple-grid组件用于绑定和显示数据

```vue
<template id="grid-template">
	<table>
		<thead>
			<tr>
				<th v-for="col in columns">
					{{ col | capitalize}}
				</th>
			</tr>
		</thead>
		<tbody>
			<tr v-for="(index,entry) in dataList">
				<td v-for="col in columns">
					{{entry[col]}}
				</td>
			</tr>
		</tbody>
	</table>
</template>

<script src="js/vue.js"></script>
<script>
	Vue.component('simple-grid', {
		template: '#grid-template',
		props: ['dataList', 'columns']
	})
</script>
```


## 创建和注册modal-dialog组件

数据的新建和编辑将使用模态对话框，modal-dialog组件的作用就在于此。

```vue
<template id="dialog-template">
	<div class="dialogs">
		<div class="dialog" v-bind:class="{ 'dialog-active': show }">
			<div class="dialog-content">
				<div class="close rotate">
					<span class="iconfont icon-close" @click="close"></span>
				</div>
				<slot name="header"></slot>
				<slot name="body"></slot>
				<slot name="footer"></slot>
			</div>
		</div>
		<div class="dialog-overlay"></div>
	</div>
</template>

<script>
	Vue.component('modal-dialog', {
		template: '#dialog-template',
		props: ['show'],
		methods: {
			close: function() {
				this.show = false
			}
		}
	})
</script>
```


## AjaxHelper

基于$.ajax声明一个简单的AjaxHelper构造器，AjaxHelper构造器的原型对象包含5个方法，分别用于处理`GET, POST, PUT, DELETE和JSONP`请求。

```js
function AjaxHelper() {
	this.ajax = function(url, type, dataType, data, callback) {
		$.ajax({
			url: url,
			type: type,
			dataType: dataType,
			data: data,
			success: callback,
			error: function(xhr, errorType, error) {
				alert('Ajax request error, errorType: ' + errorType +  ', error: ' + error)
			}
		})
	}
}
AjaxHelper.prototype.get = function(url, data, callback) {
	this.ajax(url, 'GET', 'json', data, callback)
}
AjaxHelper.prototype.post = function(url, data, callback) {
	this.ajax(url, 'POST', 'json', data, callback)
}

AjaxHelper.prototype.put = function(url, data, callback) {
	this.ajax(url, 'PUT', 'json', data, callback)
}

AjaxHelper.prototype.delete = function(url, data, callback) {
	this.ajax(url, 'DELETE', 'json', data, callback)
}

AjaxHelper.prototype.jsonp = function(url, data, callback) {
	this.ajax(url, 'GET', 'jsonp', data, callback)
}

AjaxHelper.prototype.constructor = AjaxHelper
```

# 实现GET请求

## 发送get请求

```js
var ajaxHelper = new AjaxHelper()

var demo = new Vue({
	el: '#app',
	data: {
		gridColumns: ['customerId', 'companyName', 'contactName', 'phone'],
		gridData: [],
		apiUrl: 'http://localhost:15341/api/Customers'
	},
	ready: function() {
		this.getCustomers()
	},
	methods: {

		getCustomers: function() {
			// 定义vm变量，让它指向this,this是当前的Vue实例
			var vm = this,
				callback = function(data) {
					// 由于函数的作用域，这里不能用this
					vm.$set('gridData', data)
				}
			ajaxHelper.get(vm.apiUrl, null, callback)
		}
	}
})
```


由于客户端和服务端Web API是分属于不同站点的，它们是不同的源，这构成了跨域请求。
这时请求是失败的，浏览器会提示一个错误：
No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin '[http://127.0.0.1::8020'](http://127.0.0.1::8020') is therefore not allowed access.          

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063358092-224783088.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063357499-1932320741.png)

## 跨域解决方案

现在碰到了请求跨域的问题，结合前面讲的一些概念，我们大致可以猜到解决跨域请求的两种方式：

- 在服务端启用CORS。 

- 让无服务端拥有处理JSONP的能力。

这两种跨域解决方案的区别是什么呢？

- JSONP只支持GET请求；CORS则支持GET、POST、PUT、DELETE等标准的HTTP方法 

- 使用JSONP时，服务端要处理客户端请求的callback参数（"callback"这个名称是可以指定的）；而使用CORS则不需要提供这样的处理。 

- JSONP从服务端获取到的是script文件；CORS则是一段XML或JSON或其他格式的数据 

- JSONP支持IE8, IE9复古的浏览器；CORS则支持现代主流的浏览器

选择JSONP还是CORS？除了极少数的情况，我们都应当选择CORS作为最佳的跨域解决方案。

## 启用CORS

在Nuget Package Manager Console下输入以下命令:

    Install-Package Microsoft.AspNet.WebApi.Cors

首先，在WebApiConfig中启用CORS

    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
           	// ...
            config.EnableCors();
        }
    }


然后在CustomersController上添加EnableCors特性，origins、headers和methods都设为*

    [EnableCors(origins: "*", headers: "*", methods: "*")]
    public class CustomersController : ApiController
    {
     
    }


Web API的CORS配置说明及原理，下面这个地址已经讲得很清楚了：
[http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)

刷新页面，现在数据可以正常显示了。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063359108-804553340.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063358546-499654757.png)

[View Demo](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-cors-get.html)

在Chrome开发工具的Network选项下，可以看到Response Header的Content-Type是application/json。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063400014-150358110.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063359546-1683855889.png)

文章开头也说了，CORS的跨域方式支持现代的主流浏览器，但是不支持IE9及以下这些古典的浏览器。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063404764-2050665787.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063403717-174884779.png)

如果我们偏要在IE9浏览器中（Vue.js不支持IE8及以下的浏览器，所以不考虑IE 6,7,8）实现跨域访问，该怎么做呢？
答案是使用JSONP请求。

# 实现JSONP请求

将getCustomers方法的get请求变更为jsonp请求：

```js
getCustomers: function() {
	// 定义vm变量，让它指向this,this是当前的Vue实例
	var vm = this,
		callback = function(data) {
			// 由于函数的作用域，这里不能用this
			vm.$set('gridData', data)
		}
	ajaxHelper.jsonp(vm.apiUrl, null, callback)
}
```


刷新页面，请求虽然成功了，但画面没有显示数据，并且弹出了请求错误的消息。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063406030-168027036.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063405327-1659043697.png)

## 让WebAPI支持JSONP请求

在Nuget Package Manager中输入以下命令：

    Install-Package WebApiContrib.Formatting.Jsonp

然后在Global.asax的Application_Start()方法中注册JsonpMediaTypeFormatter：

    // 注册JsonpMediaTypeFormatter，让WebAPI能够处理JSONP请求
    config.Formatters.Insert(0, new JsonpMediaTypeFormatter(jsonFormatter));


刷新页面，使用JSONP请求也能够正常显示数据了。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063407452-1685315256.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063406671-683217428.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063408671-99251497.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063408092-697177282.png)

**注意：**使用JSONP时，服务端返回的不再是一段JSON了，而是一个script脚本。

在IE9下查看该页面，simple-grid组件也能正常显示数据：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063409780-964155033.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063409171-1833562381.png)

[View Demo](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-jsonp.html)

# 实现POST请求

## 1. 创建表单对话框

添加一个Create按钮，然后使用modal-dialog组件创建一个表单对话框：

```vue
<div id="app">

	<!--...已省略-->
	<div class="container">
		<div class="form-group">
			<button @click="this.show = true">Create</button>
		</div>
	</div>
	<modal-dialog v-bind:show.sync="show">

		<header class="dialog-header" slot="header">
			<h1 class="dialog-title">Create New Customer</h1>
		</header>

		<div class="dialog-body" slot="body">
			<div v-show="item.customerId" class="form-group">
				<label>Customer Id</label>
				<input type="text" v-model="item.customerId" disabled="disabled" />
			</div>
			<div class="form-group">
				<label>Company Name</label>
				<input type="text" v-model="item.companyName" />
			</div>

			<div class="form-group">
				<label>Contact Name</label>
				<input type="text" v-model="item.contactName" />
			</div>

			<div class="form-group">
				<label>Phone</label>
				<input type="text" v-model="item.phone" />
			</div>
			<div class="form-group">
				<label></label>
				<button @click="createCustomer">Save</button>
			</div>
		</div>
	</modal-dialog>
</div>
```

**注意：**在新建Customer时，由于item.customerId为空，所以item.customerId关联的表单不会显示；在修改Customer时，item.customerId关联的表单会显示出来。另外，item.customerId是不可编辑的。

## 2. 修改Vue实例

```js
var demo = new Vue({
	el: '#app',
	data: {
		show: false,
		gridColumns: [{
			name: 'customerId',
			isKey: true
		}, {
			name: 'companyName'
		}, {
			name: 'contactName'
		}, {
			name: 'phone'
		}],
		gridData: [],
		apiUrl: 'http://localhost:15341/api/Customers',
		item: {}
	},
	ready: function() {
		this.getCustomers()
	},
	methods: {
		// ... 已省略
		createCustomer: function() {
			var vm = this,
				callback = function(data) {
					vm.$set('item', {})
						// 添加成功后，重新加载页面数据
					vm.getCustomers()
				}
				// 将vm.item直接POST到服务端
			ajaxHelper.post(vm.apiUrl, vm.item, callback)
			this.show = false
		}
	}
})
```


**修改Vue实例的data选项：**

- 添加`show`属性：用于显示或隐藏表单对话框 

- 修改`gridColumns`属性：列包含两个属性，name表示列名称，isKey表示列是否为主键列 

- 添加`item`属性：用于新增Customer或修改Customer

**添加createCustomer方法：**

```js
createCustomer: function() {
	var vm = this,
		callback = function(data) {
			vm.$set('item', {})
			// 添加成功后，重新加载页面数据
			vm.getCustomers()
		}
		// 将vm.item直接POST到服务端
		ajaxHelper.post(vm.apiUrl, vm.item, callback)
		this.show = false
}
```

[![24](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063413702-313371360.gif)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063411280-438273948.gif)

[View Demo](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-cors-put.html)

# 实现PUT请求

## 1. 修改sample-grid的template

给主键列添加链接，绑定click事件，事件指向loadEntry方法，loadEntry方法用于加载当前选中的数据。

```vue
<template id="grid-template">
	<table>
		<thead>
			<tr>
				<th v-for="col in columns">
					{{ col.name | capitalize}}
				</th>
			</tr>
		</thead>
		<tbody>
			<tr v-for="(index,entry) in dataList">
				<td v-for="col in columns">
					<span v-if="col.isKey"><a href="javascript:void(0)" @click="loadEntry(entry[col.name])">{{ entry[col.name] }}</a></span>
					<span v-else>{{ entry[col.name] }}</span>
				</td>
			</tr>
		</tbody>
	</table>
</template>
```


## 2. 修改simple-grid的methods选项

在simple-grid的methods选项下，添加一个`loadEntry`方法，该方法调用$.dispatch向父组件派发事件`load-entry`，并将key作为事件的入参，`load-entry`是绑定在父组件的事件名称。

```js
Vue.component('simple-grid', {
	template: '#grid-template',
	props: ['dataList', 'columns'],
	methods: {
		loadEntry: function(key) {
			this.$dispatch('load-entry', key)
		}
	}
})
```


## 3. 修改Vue实例的HTML

在Vue实例的simple-grid标签上绑定自定义事件`load-entry`，`load-entry`事件指向`loadCustomer`方法，`loadCustomer`方法用于加载选中的Customer数据。

```vue
<div id="app">
	<!--...已省略-->
	<div class="container">
		<simple-grid :data-list="gridData" :columns="gridColumns" v-on:load-entry="loadCustomer">
		</simple-grid>
	</div>
	<!--...已省略-->
</div>
```

我们应将2和3结合起来看，下图阐述了从点击simple-grid数据的链接开始，到最终打开对话框的完整过程：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063415421-318462166.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063414936-203186305.png)

**注意：`load-entry`**是Vue实例的事件，而不是simple-grid组件的事件，尽管load-entry是写在simple-grid标签上的。详情请参考上一篇文章的编译作用域

由于在新建模式和修改模式下标题内容是不同的，因此需要修改modal-dialog的`slot="header"`部分。
根据`item.customerId`是否有值确定修改模式和新建模式，修改模式下显示"Edit Customer - xxx"，新建模式下显示"Create New Customer"

```vue
<modal-dialog v-bind:show.sync="show">

	<header class="dialog-header" slot="header">
		<h1 class="dialog-title">{{ item.customerId ? 'Edit Customer - ' + item.contactName : 'Create New Customer' }}</h1>
	</header>
</modal-dialog>
```

## 4. 修改Vue实例

为data选项添加`title`属性，用于显示对话框的标题。

```vue
var demo = new Vue({
	el: '#app',
	data: {
		// ...已省略
		title: ''
		// ...已省略
	}
	// ...已省略
})
```


然后追加3个方法：`loadCustomer`, `saveCustomer`和`updateCustomer`。

```js
loadCustomer: function(customerId) {
	var vm = this
	vm.gridData.forEach(function(item) {
		if (item.customerId === customerId) {
			// 使用$.set设置item
			vm.$set('item', item)
			return
		}
	}),
	vm.$set('show', true)
},
saveCustomer: function() {
	this.item.customerId ? this.updateCustomer() : this.createCustomer()
	this.show = false
},
updateCustomer: function() {
	// 定义vm变量，让它指向this,this是当前的Vue实例
	var vm = this,
		callback = function(data) {
			// 更新成功后，重新加载页面数据
			vm.getCustomers()
		}
	// 将vm.item直接PUT到服务端
	ajaxHelper.put(vm.apiUrl + '/' + vm.item.customerId, vm.item, callback)
}
```

`saveCustomer`方法根据`item.customerId`是否有值确定修改模式和新建模式，如果是新建模式则调用`createCustomer`方法，如果是修改模式则调用`updateCustomer`方法。 

另外，需要将Save按钮的click事件绑定到saveCustomer方法。

```vue
<div class="dialog-body" slot="body">
	<!--...已省略-->
	<div class="form-group">
		<label></label>
		<button @click="saveCustomer">Save</button>
	</div>
	<!--...已省略-->
</div>
```


​    

[![25](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063422405-1690162830.gif)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063417905-2016008223.gif)

## 5. 添加$watch

使用$watch跟踪data选项show属性的变化，每当关闭对话框时就重置item。

    demo.$watch('show', function(newVal, oldVal){
    	if(!newVal){
    		this.item = {}
    	}
    })


为什么要这么做呢？因为每次打开对话框，不知道是以新建模式还是修改模式打开的，如果不重置item，倘若先以修改模式打开对话框，再以新建模式打开对话框，新建模式的对话框将会显示上次打开的数据。

[View Demo](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-cors-put.html)

# 实现DELETE请求

## 1. 修改simple-grid组件

在methods选项中添加方法`deleteEntry`：

```js
deleteEntry: function(entry) {
	this.$dispatch('delete-entry', entry)
}
```


调用`$.dispatch`向父组件派发事件`delete-entry`。

## 2. 修改Vue实例的HTML

在simple-grid标签上绑定自定义事件`delete-entry`，该事件指向`deleteCustomer`方法。

```vue
<div id="app">
	<!--...已省略-->
	<div class="container">
		<simple-grid :data-list="gridData" :columns="gridColumns" 
			v-on:load-entry="loadCustomer" 
			v-on:delete-entry="deleteCustomer">
		</simple-grid>
	</div>
	<!--...已省略-->
</div>
```


[![26](https://images2015.cnblogs.com/blog/341820/201607/341820-20160707063425999-1879150714.gif)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160707063424921-625242796.gif)

[View Demo](http://211.149.193.19:8090/vue-tutorials/03.Ajax/jquery-zepto/ajax-cors-delete.html)

# 总结

本篇介绍了同源策略、跨域、CORS和REST等概念，并结合Vue.js和$.ajax实现了一个简单的CURD跨域示例。
下一篇，我们将使用Vue的插件vue-resource来完成这些工作。
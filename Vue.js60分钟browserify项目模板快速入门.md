Vue.js&mdash;&mdash;60分钟browserify项目模板快速入门 - keepfool - 博客园
        var currentBlogId = 99820;
        var currentBlogApp = 'keepfool';
        var cb_enable_mathjax = false;
        var isLogined = false;
    

关注我

[![返回主页](/skins/custom/images/logo.gif)](https://www.cnblogs.com/keepfool/)
# [Vue.js&mdash;&mdash;60分钟browserify项目模板快速入门](https://www.cnblogs.com/keepfool/p/5677032.html)

# 概述

在之前的一系列vue.js文章，我们都是用传统模式引用vue.js以及其他的js文件的，这在开发时会产生一些问题。

首先，这限定了我们的开发模式是基于页面的，而不是基于组件的，组件的所有代码都直接写在页面中，这对于一些复杂的页面来说，不是好事情，代码的可读性会较差，也不便于管理。

其次，定义在HTML页面中的组件，无法被其他页面重用，我们只能通过复制粘贴的方式将组件代码拷贝到其他页面，这也违反了DRY原则。既然组件是Vue.js的重要概念，我们就应该利用好它。

为了解决这些问题，Vue.js官方提供了一些开发模板，这使得我们能够快速地配置好vue.js的开发环境。基于这些开发模板，将使用.vue文件定义组件，使组件代码从HTML页面代码中分离出来。在定义组件时，使用一些ES2015的便捷语法，将会使组件开发变得更加轻松。

Vue.js官方提供了两种类型的模板项目：

- **基于vue cli和browserify的项目模板 **
- **基于vue cli和webpack的项目模板**

vue cli是Vue.js官方提供的命令行创建Vue.js项目脚手架的工具。这两种类型的项目模板分别提供了**简单模式和完全模式**，简单模式仅包含基于vue.js开发必要的一些资源和包，使用该模式可以快速地投入到开发。完全模式则还包括ESLink、单元测试等功能。

今天我们首先介绍基于vue cli和browserify的项目模板。

本文的Demo和源代码已放到GitHub，如果您觉得内容不错，请点个赞，或在GitHub上加个星星！

[vue-browserify-simple示例](https://keepfool.github.io/vue-tutorials/05.OfficialTemplates/my-browserify-simple-demo/)

[GitHub Source](https://github.com/keepfool/vue-tutorials/tree/master/05.OfficialTemplates)

# browserify介绍

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184655623-1535852840.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184654404-1711561150.png)

> Browsers don't have the require method defined, but Node.js does. With Browserify you can write code that uses require in the same way that you would use it in Node.

Browserify是一个CommonJS风格的模块及依赖管理工具，它不仅是一个打包工具，更关键的是其JavaScript的模块及依赖管理能力。

Browserify参照了Node中的模块系统，约定用`require()`来引入其他模块，用`module.exports`来引出模块。在我看来，Browserify不同于RequireJS和Sea.js的地方在于，它没有着力去提供一个“运行时”的模块加载器，而是强调进行预编译。预编译会带来一个额外的过程，但对应的，你也不再需要遵循一定规则去加一层包裹。因此，相比较而言，Browserify提供的组织方式更简洁，也更符合CommonJS规范。  

Browserify主页：[http://browserify.org/](http://browserify.org/)

# vueify介绍

所谓vueify，就是使用.vue格式的文件定义组件，**一个.vue文件就是一个组件。**

在.vue文件定义的组件内容包括3部分：

- **<style></style>标签**：定义组件样式  
- **<template></template>标签**：定义组件模板  
- **<script></script>标签**：定义组件的各种选项，比如data, methods等。

vue.js已经拥抱ES6了，使用.vue文件定义组件时，需要使用ES6的一些语法，ES6提供了一些较为优秀的特性，这使得定义组件的代码变得更加简洁。

以下app.vue文件定义了一个组件：

```vue
// app.vue
<style>
  .red {
    color: #f00;
  }
</style>

<template>
  <h1 class="red">{{msg}}</h1>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>
```

你也可以在组件文件中使用预处理语言：

```vue
// app.vue
<style lang="stylus">
.red
  color #f00
</style>

<template lang="jade">
h1(class="red") {{msg}}
</template>

<script lang="coffee">
module.exports =
  data: ->
    msg: 'Hello world!'
</script>
```

如果你不太熟悉预处理语言的语法，你可以使用第一种方式来定义组件。

vueify主页：[https://github.com/vuejs/vueify](https://github.com/vuejs/vueify)

# 安装vue cli

vue cli是什么呢？官方的介绍如下：

> A simple CLI for scaffolding Vue.js projects.

像Visual Studio这样的工具，在创建项目时可以直接选择Visual Studio内置的一些项目模板，例如：创建一个ASP.NET MVC或ASP.NET Web API项目。
Vue.js提供了的一系列项目模板，但它没有Visual Studio这种可视化的工具。通过什么方式来使用项目模板呢？这就是vue cli要干的事儿。

安装要求：安装vue cli需要Node.js和Git，Node.js需要4.x版本以上。

我使用的是node 6.3.0版本：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184657623-392255933.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184656639-1878962257.png)

执行以下命令安装vue cli

```shell
npm install -g vue-cli
```

安装完vue cli后，我们就可以在git bash窗口中使用命令创建Vue.js的项目了。

# 使用vue-browserify-simple模板

## 1.生成项目

右键运行Git Bash Here

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716190001107-596718277.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716190000232-1016802342.png)

在git bash下输入以下命令：

```shell
vue init browserify-simple my-browserify-simple-demo
```

`browserify-simple`是项目模板的名称，`my-browserify-simple-demo`是你要生成的项目名称。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184701920-166074753.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184700654-211890725.png)

执行这个命令时，会有一些提示，一路按回车键。提示private (Y/n)时，根据需要输入Y或n，这里我输入了Y作为私有项目。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184704482-1738437096.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184703639-1336382246.png)

目录下生成了一个文件夹my-browserify-simple-demo。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184706186-876496426.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184705451-1179074601.png)

## 2. 项目结构说明

打开my-browserify-simple-demo文件夹，看到以下目录结构：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184707920-986892451.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184707045-1048070516.png)

打开index.html文件，注意它所引用的build.js文件并不存在，后面我会告诉你dist/build.js文件是如何生成的。

```vue
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>my-browserify-simple-demo</title>
  </head>
  <body>
    <app></app>
    <script src="dist/build.js"></script>
  </body>
</html>
```

文件树结构如下：

```shell
├─.babelrc	// babel配置文件
├─.gitignore	
├─index.html	// 主页
├─package.json	// 项目配置文件
├─README.md  
├─dist		// 发布目录
│   ├─.gitkeep       
├─src		// 开发目录	
│   ├─App.vue	// App.vue组件
│   ├─main.js	// 预编译入口
```

这里要特别说明两个文件：`package.json`和`.babelrc`文件。

### package.json

有很多初学者看到package.json文件就会头痛，这个文件到底是啥玩意儿，一堆看不懂的东西。

```js
{
  "name": "my-browserify-simple-demo",
  "description": "A Vue.js project",
  "author": "keepfool <crmug@outlook.com>",
  "scripts": {
    "watchify": "watchify -vd -p browserify-hmr -e src/main.js -o dist/build.js",
    "serve": "http-server -c 1 -a localhost",
    "dev": "npm-run-all --parallel watchify serve",
    "build": "cross-env NODE_ENV=production browserify src/main.js | uglifyjs -c warnings=false -m > dist/build.js"
  },
  "dependencies": {
    "vue": "^1.0.0"
  },
  "devDependencies": {
    "babel-core": "^6.0.0",
    "babel-plugin-transform-runtime": "^6.0.0",
    "babel-preset-es2015": "^6.0.0",
    "babel-preset-stage-2": "^6.0.0",
    "babel-runtime": "^6.0.0",
    "cross-env": "^1.0.6",
    "babelify": "^7.2.0",
    "browserify": "^12.0.1",
    "browserify-hmr": "^0.3.1",
    "http-server": "^0.9.0",
    "npm-run-all": "^1.6.0",
    "uglify-js": "^2.5.0",
    "vueify": "^8.5.2",
    "watchify": "^3.4.0"
  },
  "browserify": {
    "transform": [
      "vueify",
      "babelify"
    ]
  }
}
```


package.json文件是项目配置文件，除了项目的一些基本信息外，有3个重要的节点我说明一下：

- dependencies：项目发布时的依赖 

- devDependencies：项目开发时的依赖 

- scripts：编译项目的一些命令

### .babelrc文件

.babelrc文件定义了ES6的转码规则，基于ES6编写的js代码在编译时都会被babel转码器转换为ES5代码。

```js
{
  "presets": ["es2015", "stage-2"],
  "plugins": ["transform-runtime"]
}
```


为什么要将ES6转换为ES5呢？因为我们将使用ES6的一些语法来编写.vue组件，而有些浏览器目前还没有全面支持ES6。

babel是一个非常有名的ES6转码器，babel主页：[https://babeljs.io/](https://babeljs.io/)

## 3. 安装依赖

执行以下命令安装项目依赖：

```shell
cd my-browserify-simple-demo
npm install
```

安装完成后，目录下会产生一个node_modules文件夹。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184710217-1962015618.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184709279-2073475162.png)

项目相关的依赖都存放在这个文件夹下了。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184712357-359005155.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184711529-646375648.png)

## 4. 运行示例

执行以下命令运行示例：

```shell
npm run dev
```


[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184713764-787157120.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184713045-147688229.png)

在执行该命令后，dist目录下会生成一个build.js文件。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184715420-1589144188.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184714576-1255392326.png)

打开127.0.0.1:8080，可以看到以下画面：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184716873-1392989401.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184716139-421371947.png)

**注意：**之前我使用的node是4.4.5的版本，执行这个命令会出现一个错误：cann't find module vue-hot-reload api..，我将node.js升级到6.3.0版本后，删除node_modules文件夹，然后再次执行npm install命令重新安装依赖以后，该问题就解决了。

## 5. 编译过程说明

build.js文件是怎么产生的呢？我们把它分为两种情况：

- 开发时生成build.js 

- 发布时生成build.js

何为开发时和发布时？开发过.NET童鞋，可以简单的理解为debug模式和release模式。

### 开发时成成build.js

在package.json文件的scripts节点下，有3行配置：

```js
"scripts": {
  "watchify": "watchify -vd -p browserify-hmr -e src/main.js -o dist/build.js",
  "serve": "http-server -c 1 -a localhost",
  "dev": "npm-run-all --parallel watchify serve"
}
```

`npm run dev`命令执行的是`dev`节点对应的命令`npm-run-all --parallel watchify serve` ，这行命令是依赖于`watchify`的，也就是下面这行命令：

```shell
watchify -vd -p browserify-hmr -e src/main.js -o dist/build.js
```

你不用纠结这行命令的每个参数是什么意思，只需要理解src/main.js –o dist/build.js的含义：

编译src/main.js文件，然后输出到dist/build.js

serve节点的命令`http-server -c 1 -a localhost`用于开启http-server，http-server是一个简易的web服务器。
下面这幅流程图描述了这个编译过程：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184718279-2110763538.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184717545-22817188.png)

在git bash窗口执行`npm run dev`命令时，也能够观察到编译过程。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184719607-1367589299.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184718967-1851470058.png)

`watchify`是什么东东呢？`browserify`的编译是比较缓慢的，当一个`browserify`项目开始变大时，编译打包的时间会变得较长。`watchify`是一个`browserify`的封装，其在package.json中的配置和browserify是一样的。它可以持续件监视文件的改动，**并且只重新打包必要的文件**。在一个大型的项目中使用`watchify`，第一次打包的时候可能还会花较长时间，但是后续的编译打包将消耗较短的时间。

更多的watchify信息，请参考：[https://github.com/substack/watchify/](https://github.com/substack/watchify/)

### 发布时生成build.js

在理解了上述编译过程后，发布时build.js就不难理解了。

    "build": "cross-env NODE_ENV=production browserify src/main.js | uglifyjs -c warnings=false -m > dist/build.js"

执行`npm run build`命令可以生成发布时的build.js。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184721686-484078224.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184720217-1606368274.png)

uglifyjs是基于nodejs的压缩程序，用于压缩HTML/CSS/JS，执行`npm run build`命令后生成的build.js就是经过压缩的。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184722889-1670429757.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184722295-523118632.png)

# 使用vue-browserify模板

`simple-browserify`模板用于构筑基于browserify和vueify的开发环境，`browserify`模板则提供了更多内容：

- 提供了ESLint：用于格式化JavaScript和检查JavaScript代码风格。 

- 提供了单元测试：使用PhantomJS with Karma + karma-jasmine + karma-browserify的单元测试, 支持ES2015和mock。

## 1.生成项目

重新打开一个git bash窗口，执行以下命令：

    vue init browserify my-browserify-demo

`browserify`是项目模板，`my-browserify-demo`是项目名称。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184724389-470826111.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184723701-1664616654.png)

目录下生成了一个文件夹my-browserify-demo：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184726873-1175633397.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184725811-377223471.png)

文件目录结构如下：

    ├─.babelrc		// babel配置文件
    ├─.eslintrc		// eslint配置文件
    ├─.gitignore	
    ├─index.html		// 主页
    ├─karma.conf.js		// karma配置文件
    ├─package.json		// 项目配置文件
    ├─README.md  
    ├─dist			// 发布目录
    │   ├─.gitkeep       
    ├─src			// 开发目录	
    │   ├─App.vue		// App.vue组件
    │   ├─main.js		// 预编译入口
    │   ├─components		// 组件目录
    │   ├──Hello.vue		// Hello.vue组件
    ├─test			// 单元测试目录	
    │   ├─unit	 
    │   ├──Hello.spec.js	// Hello.vue单元测试

## 2. 安装依赖

执行以下两行命令，安装项目依赖：

    cd my-browserify-demo
    npm install

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184728357-264865877.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184727623-1420385631.png)

（安装过程较为缓慢，需要耐心等待…）

## 3. 运行示例

执行npm run dev命令启动http-server。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184730311-1801911152.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184729373-907765518.png)

在浏览器中输入地址127.0.0.1:8080，可以看到以下画面：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184731920-688392957.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184730967-1528554710.png)

## 示例说明

注意main.js, App.vue和Hello.vue三者之间的关系。

### main.js

```js
import Vue from 'vue'
import App from './App.vue'

/* eslint-disable no-new */
new Vue({
  el: 'body',
  components: { App }
})
```


`import Vue from 'vue'`这行代码表示引入vue.js，在编译时会从node_modules文件夹下寻找vue.js。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184733857-744026862.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184732811-1306677049.png)

`import App from './App.vue' `表示引入同目录下的App.vue组件。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184735045-533826827.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716184734482-1001876066.png)

### App.vue

```vue
<template>
  <div id="app">
    <hello></hello>
  </div>
</template>

<script>
import Hello from './components/Hello.vue'

export default {
  components: {
    Hello
  }
}
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
}
</style>
```

`import Hello from './components/Hello.vue' `表示引入components目录下的`Hello.vue`组件。

`Hello.vue`是`App.vue`的子组件：

```js
export default {
  components: {
    Hello
  }
}
```


### Hello.vue

```vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // Note: modifying `msg` below will not cause changes to occur with
      // hot-reload. As reloaded components preserve their initial state,
      // modifying these values will have no effect.
      msg: 'Hello World!'
    }
  }
}
</script>
```


`Hello.vue`组件没有任何依赖，它仅仅输出一段简单的文字。

## vue-browserify-simple示例

介绍完了这两种模板，我们就来做个小示例吧，下面这个示例使用的是vue-browserify-simple模板。

    ├─.babelrc		// babel配置文件
    ├─.gitignore	
    ├─index.html		// 主页
    ├─package.json		// 项目配置文件
    ├─README.md  
    ├─dist			// 发布目录
    │   ├─.gitkeep       
    ├─src			// 开发目录	
    │   ├─App.vue		// App.vue组件
    │   ├─main.js		// 预编译入口
    │   ├─components		// 组件目录
    │   ├──SimpeGrid.vue	// SimpleGrid.vue组件
    │   ├─assets		// 静态资源文件目录
    │   ├──app.css		// app.css

该示例的代码我就不贴出来了，大家可以到本文开头的GitHub地址去下载。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716213822389-1191960818.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160716213821389-1803490985.png)

[View Demo](https://keepfool.github.io/vue-tutorials/05.OfficialTemplates/my-browserify-simple-demo)

# 总结

vue.js官方提供的browserify项目模板，可以让我们很轻松地投入到vue.js的组件开发中。熟练使用这样的模板项目，不仅将组件代码给分离出来了，使得组件可以被重复利用，而且有助于组件代码的管理。
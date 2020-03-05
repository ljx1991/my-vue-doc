# [Vue.js&mdash;&mdash;60分钟webpack项目模板快速入门](https://www.cnblogs.com/keepfool/p/5678427.html)

# 概述

browserify是一个 CommonJS风格的模块管理和打包工具，上一篇我们简单地介绍了Vue.js官方基于browserify构筑的一套开发模板。webpack提供了和browserify类似的功能，在前端资源管理这方面，它提供了更加出色的功能。
官方基于webpack提供了两种项目模板，分别是vue-webpack-simple模板和vue-webpack模板，今天我们将介绍官方提供的这两种项目模板，并用vue-webpack-simple模板创建一个简单的示例。

本文的Demo和源代码已放到GitHub，如果您觉得内容不错，请点个赞，或在GitHub上加个星星！

[vue-webpack-simple示例](https://keepfool.github.io/vue-tutorials/05.OfficialTemplates/my-webpack-simple-demo/)

[GitHub Source](https://github.com/keepfool/vue-tutorials/tree/master/05.OfficialTemplates)

# Webpack简介

Webpack是当下最热门的前端资源模块化管理和打包工具，它可以将很多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源，还可以将按需加载的模块进行代码分割，等到实际需要的时候再异步加载。通过 `loader` 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。

下图是官方对Webpack的简介，通过这幅图也不难看出，一些相互依赖的模块文件，会被打包成一个或多个js文件，可以减少HTTP的请求次数。

 

![enter image description here](http://cdn2.infoqstatic.com/statics_s1_20150616-0050/resource/articles/react-and-webpack/zh/resources/0602005.jpg)

Webpack支持的特性：

1. 支持CommonJs和AMD模块，意思也就是我们基本可以无痛迁移旧项目。  
2. 串联式模块加载器以及插件机制，让其具有更好的灵活性和扩展性，例如提供对CoffeeScript、ES6的支持。  
3. 可以通过配置或智能分析打包成多个文件，实现公共模块或按需加载。  
4. 将样式文件和图片等静态资源也可视为模块进行打包。配合loader加载器，可以支持sass，less等CSS预处理器。  
5. 内置有source map，即使打包在一起依旧方便调试。

由于本文不是webpack的教程，所以webpack相关的一些知识还需要各位去了解。

webpack主页：[http://webpack.github.io/](http://webpack.github.io/)

# 环境准备

Node.js和Git是必备的工具，NPM的版本最好是3.x版本以上，NPM 3.x提供了更有效的包依赖管理。

![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160716184657623-392255933.png)

在使用这两种项目模板前，需要先安装vue cli，执行以下命令安装vue cli

```shell
npm install -g vue-cli
```

安装完vue cli后，我们就可以基于vue-webpack-simple模板和vue-webpack模板创建项目了。

# 使用vue-webpack-simple模板

## 1.生成项目

在某个目录下右键运行Git Bash Here，比如我的目录是D:\VueOfficialTemplates。 

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133620779-1384790589.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133619904-580584075.png)

在git bash下输入以下命令：
```shell
vue init webpack-simple my-webpack-simple-demo
```

`webpack-simple`是项目模板的名称，`my-webpack-simple-demo`是你要生成的项目名称。 

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133622857-1460743175.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133621623-1043651008.png)

目录下生成了一个文件夹my-webpack-simple-demo。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133624717-1010670908.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133623842-1364171941.png)

## 2. 项目结构说明

打开my-webpack-simple-demo文件夹，看到以下目录结构：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133626404-792607337.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133625701-518861607.png)

文件树结构如下：

```js
├─.babelrc		// babel配置文件
├─.gitignore	
├─index.html		// 主页
├─package.json		// 项目配置文件
├─README.md  
├─webpack.config.js	// webpack配置文件
├─dist			// 发布目录
│   ├─.gitkeep       
├─src			// 开发目录	
│   ├─App.vue		// App.vue组件
│   ├─main.js		// 预编译入口
```

相比于browserify-simple模板，多了一个webpack.config.js文件。

### package.json

```js
{
  "name": "my-webpack-simple-demo",
  "description": "A Vue.js project",
  "author": "keepfool <crmug@outlook.com>",
  "private": true,
  "scripts": {
    "dev": "webpack-dev-server --inline --hot",
    "build": "cross-env NODE_ENV=production webpack --progress --hide-modules"
  },
  "dependencies": {
    "vue": "^1.0.0",
    "babel-runtime": "^6.0.0"
  },
  "devDependencies": {
    "babel-core": "^6.0.0",
    "babel-loader": "^6.0.0",
    "babel-plugin-transform-runtime": "^6.0.0",
    "babel-preset-es2015": "^6.0.0",
    "babel-preset-stage-2": "^6.0.0",
    "cross-env": "^1.0.6",
    "css-loader": "^0.23.0",
    "file-loader": "^0.8.4",
    "json-loader": "^0.5.4",
    "url-loader": "^0.5.7",
    "vue-hot-reload-api": "^1.2.0",
    "vue-html-loader": "^1.0.0",
    "vue-loader": "^8.2.1",
    "vue-style-loader": "^1.0.0",
    "webpack": "^1.12.2",
    "webpack-dev-server": "^1.12.0"
  }
}
```

开发时依赖babel、各种loader和webpack，发布时依赖vue和babel-runtime。
scripts节点同样定义了开发时和发布时的编译命令，和browserify不同的是，编译的输入和输出是定义在webpack.config.js文件中的。

### webpack.config.js

```js
var path = require('path')
var webpack = require('webpack')

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    filename: 'build.js'
  },
  resolveLoader: {
    root: path.join(__dirname, 'node_modules'),
  },
  module: {
    loaders: [
      {
        test: /\.vue$/,
        loader: 'vue'
      },
      {
        test: /\.js$/,
        loader: 'babel',
        exclude: /node_modules/
      },
      {
        test: /\.json$/,
        loader: 'json'
      },
      {
        test: /\.html$/,
        loader: 'vue-html'
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'url',
        query: {
          limit: 10000,
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  },
  devServer: {
    historyApiFallback: true,
    noInfo: true
  },
  devtool: '#eval-source-map'
}

if (process.env.NODE_ENV === 'production') {
  module.exports.devtool = '#source-map'
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    }),
    new webpack.optimize.OccurenceOrderPlugin()
  ])
}
```

webpack.config.js内容还是比较好理解的，它采用了CommonJS的写法，entry节点配置了编译入口，output节点配置了输出。
这段entry和output配置的含义是：编译src/main.js文件，然后输出到dist/build.js文件。

## 3. 安装项目依赖

执行以下命令安装项目依赖：
    cd my-webpack-simple-demo
    npm install

安装完成后，目录下会产生一个node_modules文件夹。 

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133627967-922755769.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133627186-544400345.png)

项目相关的依赖都存放在这个文件夹下了，比vue-browserify-simple项目模板的依赖要多得多。 

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133629389-1306182826.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133628670-1743593464.png)

## 4. 运行示例

执行以下命令运行示例：

```shell
npm run dev
```




打开127.0.0.1:8080，可以看到以下画面：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133630904-874327744.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133630154-958590976.png)

注意：和browserify不同的是，执行`npm run dev`命令后并不会在dist目录下生成build.js文件，开发环境下build.js是在运行内存中的。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133632639-741702640.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133631857-1876366857.png)

## 5. 发布

执行以下命令会生成发布时的build.js，并且是经过压缩的。

    npm run build

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133633936-1015118286.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133633217-2057026477.png)

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133635342-1516498936.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133634529-1537321746.png)

# 使用vue-webpack模板

重新打开一个git bash窗口，执行以下命令：

```shell
vue init webpack my-webpack-demo
```



 `webpack`是项目模板，`my-webpack-demo`是项目名称。 

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133637186-234798806.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133636107-683263063.png)

目录下生成了一个文件夹my-webpack-demo：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133639717-1377541468.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133638795-1492790433.png)

文件目录结构如下（参考：[https://vuejs-templates.github.io/webpack/structure.html](https://vuejs-templates.github.io/webpack/structure.html)）

    ├── build/                      # webpack config files
    │   └── ...
    ├── config/                     
    │   ├── index.js                # main project config
    │   └── ...
    ├── src/
    │   ├── main.js                 # app entry file
    │   ├── App.vue                 # main app component
    │   ├── components/             # ui components
    │   │   └── ...
    │   └── assets/                 # module assets (processed by webpack)
    │       └── ...
    ├── static/                     # pure static assets (directly copied)
    ├── test/
    │   └── unit/                   # unit tests
    │   │   ├── specs/              # test spec files
    │   │   ├── index.js            # test build entry file
    │   │   └── karma.conf.js       # test runner config file
    │   └── e2e/                    # e2e tests
    │   │   ├── specs/              # test spec files
    │   │   ├── custom-assertions/  # custom assertions for e2e tests
    │   │   ├── runner.js           # test runner script
    │   │   └── nightwatch.conf.js  # test runner config file
    ├── .babelrc                    # babel config
    ├── .editorconfig.js            # editor config
    ├── .eslintrc.js                # eslint config
    ├── index.html                  # index.html template
    └── package.json                # build scripts and dependencies

## 2. 安装依赖

执行以下两行命令，安装项目依赖：

  ```shell
cd my-webpack-demo
npm install
  ```



[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133640982-1534130289.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133640311-294822937.png)

（安装过程较为缓慢，需要耐心等待…）

## 3. 运行示例

执行以下命令运行示例：
    npm run dev

打开127.0.0.1:8080，可以看到以下画面：

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133643170-1869318949.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133642139-244405191.png)

## 4. 发布

执行以下命令生成发布：

```shell
npm run build
```

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133644764-1870919522.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133644107-2090524277.png)

和vue-simple-webpack模板不同的是，所有的静态资源，包括index.html都生成到dist目录下了。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133646264-2054001117.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133645514-1214989084.png)

这意味着你可以直接拿着dist目录去发布应用，例如在IIS下将dist目录发布为一个网站。

# vue-simple-webpack示例

昨天我们使用了vue-simple-browserify模板做了一个小示例，今天我们就用vue-simple-webpack模板完成同样的示例。

该示例的代码我就不贴出来了，大家可以到本文开头的GitHub地址去下载。

[![image](https://images2015.cnblogs.com/blog/341820/201607/341820-20160717133647654-1292163824.png)](http://images2015.cnblogs.com/blog/341820/201607/341820-20160717133646951-437806922.png)

[View Demo](https://keepfool.github.io/vue-tutorials/05.OfficialTemplates/my-webpack-simple-demo/)

# 总结

browserify和webpack都是打包和模块依赖管理工具，webpack拥有比browserify更强的功能，使用哪种工具取决于你们个人的偏好、项目的要求。
毋庸置疑的是，Vue.js官方基于vue cli, browserify, webpack构筑的几个项目模板，确实能够给我们带来很大的便利，可以让我们快速地投入到开发中。
在后面的篇章中，我将尽可能地基于这些项目模板来讲解Vue.js的点点滴滴。
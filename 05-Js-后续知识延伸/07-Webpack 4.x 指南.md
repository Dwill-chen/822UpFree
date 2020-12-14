**参考文献**

1. <https://webpack.github.io/>
2. <https://webpack.docschina.org/>



## 一、概述

![](./assets/webpack-des.png)

webpack 是一个打包工具，打包静态资源。当 webpack 处理应用程序时，它会递归地构建一个[依赖关系图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，其中包含应用程序需要的每个[模块](https://webpack.docschina.org/concepts/modules/)，然后将所有这些模块打包成一个或多个 bundle。

从图中我们可以看出，*Webpack* 可以将多种静态资源 js、css、sass 转换成一个静态文件，减少了页面的请求。



### 1.1 什么是webpack？

> WebPack可以看做是**模块打包机**：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Sass，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。在3.0出现后，Webpack还肩负起了优化项目的责任。

这段话有三个重点：

- 打包：可以把多个Javascript文件打包成一个文件，减少服务器压力和下载带宽。
- 转换：把拓展语言转换成为普通的JavaScript，让浏览器顺利运行。
- 优化：前端变的越来越复杂后，性能也会遇到问题，而WebPack也开始肩负起了优化和提升性能的责任。



### 1.2 为什么需要webpack？

Webpack 是现代前端技术的基石，常规的开发方式，比如 jQuery、HTML、CSS 静态网页开发已经落后了。现在是MVVM的时代，数据驱动视图，Webpack 将现代js开发中的各种新型有用的技术，集合打包。通过下图理解webpack生态圈：

![](./assets/webpack-ecosphere.jpg)



### 1.3 模块化

模块化是一种处理复杂系统分解为更好的可管理模块的方式，简单来说就是解耦。通常一个文件就是一个模块，有自己的作用域，只向外暴露特定的变量和函数。

其优势为：简化开发、按需加载、便于管理、可复用。

目前流行的js模块化规范有CommonJS、AMD、CMD以及ES6的模块系统



#### a. 耦合

- 耦合是指两个或两个以上的体系或两种运动形式间通过相互作用而彼此影响以至联合起来的现象。
- 在软件工程中，对象之间的耦合度就是对象之间的依赖性。对象之间的耦合越高，维护成本越高，因此对象的设计应使类和构件之间的耦合最小。
- 分类：有软硬件之间的耦合，还有软件各模块之间的耦合。耦合性是程序结构中各个模块之间相互关联的度量。它取决于各个模块之间的接口的复杂程度、调用模块的方式以及哪些信息通过接口。



#### b. 解耦

- 解耦，字面意思就是解除耦合关系。
- 在软件工程中，降低耦合度即可以理解为解耦，模块间有依赖关系必然存在耦合，理论上的绝对零耦合是做不到的，但可以通过一些现有的方法将耦合度降至最低。
- 设计的核心思想：尽可能减少代码耦合，如果发现代码耦合，就要采取解耦技术。让数据模型，业务逻辑和视图显示三层之间彼此降低耦合，把关联依赖降到最低，而不至于牵一发而动全身。原则就是A功能的代码不要写在B的功能代码中，如果两者之间需要交互，可以通过接口，通过消息，甚至可以引入框架，但总之就是不要直接交叉写。
- 观察者模式：观察者模式存在的意义就是「解耦」，它使观察者和被观察者的逻辑不再搅在一起，而是彼此独立、互不依赖。比如网易新闻的夜间模式，当用户切换成夜间模式之后，被观察者会通知所有的观察者「设置改变了，大家快蒙上遮罩吧」。QQ消息推送来了之后，既要在通知栏上弹个推送，又要在桌面上标个小红点，也是观察者与被观察者的巧妙配合。



#### c. [commonJS 规范](http://www.commonjs.org/specs/modules/1.0/)

CommonJS就是一个JavaScript模块化的规范，是用在服务器端的node的模块规范，webpack也是对CommonJS原生支持的。

> a. 特点：

- 模块输出的是一个值的拷贝， 模块是运行时加载，同步加载
- CommonJS 模块的顶层 `this` 指向当前模块

> b. API：

- require：导入
- module.exports 或者exports：导出

> c. 示例：新建两个模块 A.js、B.js

```javascript
// A.js
// writing one:
let a = 10, b = 20;
module.exports = { a, b };

// writing two:
module.exports.a = 10;
module.exports.b = 20;

// writing three:
exports.a = 10;
exports.b = 20;

// 三种写法结果是一样，对外暴露的接口的结果是一致的
```

```javascript
// B.js
const A = require("./A");
A  // {a:10, b:20}
```

[^ tips]:require()是 Node.js 中才支持的方法，目前前端浏览器都还没有原生支持。



##### commonJS例子

- 首先，我们创建需要依赖的模块代码m1、m2、m3

```javascript
// m1.js
module.exports = {
    msg: 'm1',
    foo: function () {
        return this.msg;
    }
}

// m2.js
module.exports = function () {
    return 'm2';
}

// m3.js
 // var exports = module.exports;在第博客二部分：commonjs规范中第六小点由说明
 exports.foo = function () {
     return 'ms';
 }
```

- 创建主入口文件index.js

```javascript
// index.js
var m1 = require('./m1');
var m2 = require('./m2');
var m3 = require('./m3');

console.log(m1.foo());
console.log(m2());
console.log(m3.foo());
```

- 前面规范中说过在浏览器并不支持commonjs规范，主入口文件index.js肯定不能直接被demo.html结构文本使用，直接使用的话会报错；所以在要前端开发中使用commonjs规范的话就必须使用插件将commonjs模块化规范转换成浏览器识别的代码结构，在这之前系统上必须安装nodejs环境，这时候我们可以先在控制台中测试看看index.js的依赖是否成功：

```shell
1 node -v //测试node环境是否安装成功，如果成功的话会在控制台打印出node的版本号
2 node index.js
```

- 这是在node环境下能够编译执行index.js,前面说过要想将commonjs模块化结构代码在浏览器中编译执行，得需要使用工具转换成浏览器能够编译执行的代码结构，这个工具就是Browserify，工具的官网：http://browserify.org/

```shell
npm install browserify
```

- 安装成功后，我们查看官网可以发现有一句话

```shell
 browserify main.js -o bundle.js
```

- 意思是将commonjs规范的main.js文件通过browserify转换成浏览器能够执行的bundle.js文件；那么我们就可以使用这个命令来将示例中的index.js转换成一个可以在浏览器使用的js文件（下面这句命令是要在命令窗口执行）：

```shell
// 我们自己的文件
 browserify index.js -o bundle.js
```

- 每次都需要输入 固定的 输入和输出文件名，我们可以使用命令，在package.json中修改

```javascript
"browserify": "./node_modules/.bin/browserify ./src/js/index.js -o ./src/js/bundle.js"
// 每次运行只需要输入npm run browserify
```

- 最后引入js，在我们的html文件中引入bundle.js

```javascript
<script src="./js/bundle.js"></script>
```

- 完整目录结构如下

![](./assets/commonJS.png)









具体详情参考：https://www.cnblogs.com/ZheOneAndOnly/p/11071280.html



注意：

1. exports 与module.exports 的区别：exports 是对 module.exports 的引用，不能直接给exports 赋值，直接赋值无效，结果是一个空对象，module.exports 可以直接赋值。

2. 一个文件不能写多个module.exports ，如果写多个，对外暴露的接口是最后一个module.exports。

3. 模块如果没有指定使用module.exports 或者exports 对外暴露接口时，在其他文件就引用该模块，得到的是一个空对象。



#### d. ES6

ES6 在语言标准的层面上，实现了模块功能，而且非常简单，ES6到来，完全可以取代 CommonJS 和AMD规范，成为浏览器和服务器通用的模块解决方案。

> a. 特点：

- ES6 模块之中，顶层的`this`指向`undefined`，即不应该在顶层代码使用`this`。
- 自动采用严格模式"use strict"。须遵循严格模式的要求
- ES6 模块export、import命令可以出现在模块的任何位置，但是必须处于模块顶层。如果处于块级作用域内，就会报错
- ES6 模块输出的是值的引用

> b. API

[import](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import)：导入模块

[export](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/export)：导出模块

> 示例

```js
// 1. 导出单个属性
export const a = 10;
export const b = 20;
import {a, b} from "./path"
// 2. 导出属性列表
const a = 10, b = 20;
export {a, b}
import {a, b} from "./path"
// 3. 重命名导出
export {a as x, b as y }
import {x, y} from "./path"
// 4. 默认导出
export default { min:10, max:20 }
import 变量 from "./path"
```



## 二、初探

### 2.1 准备

创建项目并在项目根目录中通过 `npm` 或 `yarn` 生成 *package.json* 配置文件。

```shell
# NPM
$ npm init --yes
# YARN
$ yarn init --yes
```



### 2.2 安装

安装 `webpack` 及其命令行工具 `cli`：

```shell
# NPM
$ npm install webpack webpack-cli --save-dev 
# YARN
$ yarn add webpack webpack-cli --save-dev  
# 查看版本
$ ./node_modules/.bin/webpack --version
4.41.2
```

> 提示：官方建议使用局部安装，局部安装可以使用最新的技术栈，JavaScript 是弱语言,有局部变量和全局变量,其实全局安装与局部安装的性质与函数的全局变量与局部变量有点类似。



### 2.3 创建项目结构

接下来创建如下项目结构：

```
.
├── dist
    └── index.html
├── node_modules
├── package.json
└── src
    └── js
    	├── main.js
   		├── util.js
```

>  提示：
>
>  1. src是源码文件，dist是我们编译打包好的文件；一个用于**开发环境**，一个用于**生产环境**。
>  2. node_modules 为 yarn 或 npm 安装依赖时自动生成的文件。



### 2.4 编辑项目文件

-> index.html

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>hello-webpack</title>
</head>
<body>
		<h1 id="title"></h1>
    <!-- 引入webpack打包后生成的文件 -->
    <script src="../dist/static/js/main-bundle.js"></script>
</body>
</html>
```

-> util.js

```javascript
class Util {
    constructor() {}
    static setTitle(id, title) {
        let el = document.getElementById(id);
        el.textContent = title;
    }
}

module.exports  = {
    Util
}
```

-> main.js

```javascript
import {
    Util
} from "./until.js";
Util.setTitle("title", "Hello, webpack!");
```



### 2.5 创建配置文件 

在 <ins>项目根目录下</ins> 创建 “webpack.config.js ” 文件，配置如下：

```javascript
// 1. 引入模块
const path = require('path');
// 2. 导出配置
module.exports = {
    // 配置基础路径为当前目录（默认为配置文件所在的当前目录）
    context: path.resolve(__dirname, './'),
    // 打包模式 development | production
    mode: 'development',
    // 入口 string | array | object
    entry: {
        main: './src/js/main.js'
    },
    // 出口
    output: {
        // 输出目录/绝对路径
        path: path.resolve(__dirname, './dist/'),
        // 输出文件名
        filename: 'static/js/[name]-bundle.js'
    },
    // 加载器
    module: {
        rules: []
    },
    // 开发服务
    devServer: {}
};
```



### 2.6 编译打包

## 

```shell
$ ./node_modules/.bin/webpack 
[webpack-cli] Compilation finished
asset static/js/main-bundle.js 4.53 KiB [emitted] (name: main)
runtime modules 931 bytes 4 modules
cacheable modules 277 bytes
  ./src/js/main.js 84 bytes [built] [code generated]
  ./src/js/until.js 193 bytes [built] [code generated]
webpack 5.10.1 compiled successfully in 104 ms
```

执行打包任务之后，会在 “./dist/static/js” 目录下生产一个  ”main-bundle.js“ 文件，运行 ”index.html“ 可以看到 ”Hello, webpack!“ 说明打包成功。

> 提示：你可以尝试不同的打包模式观察二者的区别。



### 2.7 开发环境


## 一、概述

Babel 是一个 JavaScript 编译器。

Babel 是一个工具链，主要用于将 ECMAScript 2015+ 版本的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。下面列出的是 Babel 能为你做的事情：

- 语法转换
- 通过 Polyfill 方式在目标环境中添加缺失的特性 (通过 [@babel/polyfill](https://www.babeljs.cn/docs/babel-polyfill) 模块)
- 源码转换 (codemods)

Babel 中文网：<https://www.babeljs.cn/>



## 二、Babel使用

### 2.1 起步

创建一个基本的项目文件结构，并新建必要文件，如下所示：

```
babel-demo
|- /dist
	|- index.html
	|- /static
	   |- /js
|- /src
	|- /js
		 |- index.js
|- package.json *
```

> 注意：dist为编译生成文件，src为源文件。



### 2.2 安装

```shell
$ npm i -D @babel/core @babel/cli @babel/preset-env
OR
$ yarn add -D @babel/core @babel/cli @babel/preset-env
```



### 2.3 配置文件 

在根目录中创建 “.babelrc” 配置文件，并设置转换规则

```json
{
    "presets": [
        "@babel/preset-env"
    ]
}
```



### 2.4 基本使用

通过上面的准备工作，我们现在就可以使用Babel编译ES6了。在 “./src/js/index.js” 写一个es6的箭头函数

```javascript
let sayHello = () => {
    console.log("Hello, babel!");
}
```

现在使用Babel命令，将目录 “./src/js/index.js” 下的文件编译输出到 “./dist/static/js/” 目录下：

```shell
$ ./node_modules/.bin/babel ./src/js/index.js -o ./dist/static/js/index.js -w -s
OR
$ ./node_modules/.bin/babel ./src/js/ -d ./dist/static/js/ -w -s
```

解读：

- `-o`：将某个js文件编译成指定js文件

- `-d`：将某个目录下的js文件编译至指定目录

- `-w`：实时监听文件/自动编译

- `-s`：生成资源映射文件便于调试，它可以帮助你在浏览器开发者工具（目前只有google chrome浏览器支持该功能）的“Source”选项卡中找到编译前的源文件，方便开发者进行调试。

  但首先得确保你开发者工具的设置里的这一项是处于勾选状态：

  右键检查 -> 工具栏中选择更多（右上角三个竖着的小圆点） -> Setting -> Sources -> Enable JavaScript source maps.

经过编译后生成的 “index.js” 是这样的，我们在index.html中引用的就是这个文件：

```javascript
var sayHello = function sayHello() {
    console.log("Hello, world!");
};
```



### 2.5 简化使用

在 “package.json” 文件的 “scripts” 属性下，设置如下代码：

```json
"scripts": {
  "build":"./node_modules/.bin/babel ./src/js/ -d ./dist/static/js/ -w -s"
}
```

> 提示：“build” 这个属性名是自定义的，其属性值则是要执行的指令。

内容配置完成之后，切换到命令行窗口输入：

 ```shell
$ npm run build
OR
$ yarn run build
 ```

这样即可执行指令进行编译。

当然，*”package.json“*  肯定不是专门为babel服务的，所有的node项目，也就是所有可以用npm或yarn安装的程序包都可以使用这个配置文件，也就是说这些项目任何复杂的命令行操作几乎都可以通过设置这个配置文件里的“scripts”属性来进行简化，并通过输入 *”npm|yran run scripts[name] “* 来触发。
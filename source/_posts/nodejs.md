---
title: nodejs 学习笔记
date: 2017-10-13 16:32:45
tags:
  - nodejs
categories: "nodejs"
---
> 参考
> module.exports,exports,export和export default,import与require区别与联系 [http://www.cnblogs.com/Nutrient-rich/p/7047877.html](http://www.cnblogs.com/Nutrient-rich/p/7047877.html)
> 阮一峰 CommonJS规范 [http://javascript.ruanyifeng.com/nodejs/module.html](http://javascript.ruanyifeng.com/nodejs/module.html)
> 7天学会NodeJs [http://nqdeng.github.io/7-days-nodejs/](http://nqdeng.github.io/7-days-nodejs/)

## 概述

JS是脚本语言，脚本语言都需要一个解析器才能运行。对于写在HTML页面里的JS，浏览器充当了解析器的角色。而对于需要独立运行的JS，NodeJS就是一个解析器。
每一种解析器都是一个运行环境，不但允许JS定义各种数据结构，进行各种计算，还允许JS使用运行环境提供的内置对象和方法做一些事情。例如运行在浏览器中的JS的用途是操作DOM，浏览器就提供了document之类的内置对象。而运行在NodeJS中的JS的用途是操作磁盘文件或搭建HTTP服务器，NodeJS就相应提供了fs、http等内置对象。

Node 应用由模块组成，采用 CommonJS 模块规范。
每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

## 模块

### 模块导出（ module.exports || exports ）

`module.exports` 属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取 `module.exports` 变量。
```js
// rocker.js
module.exports = { name: 'xlslucky' };

// index.js
var rocker = require('./rocker.js');
rocker // { name: 'xlslucky' }
```

Node为每个模块提供一个 `exports` 变量，指向 `module.exports` 。这等同在每个模块头部，有一行这样的命令。
```js
var exports = module.exports;
```

在对外输出模块接口时，可以向 `exports` 对象添加方法。
```js
// rocker.js
exports.name = 'xlslucky';

// index.js
var rocker = require('./rocker.js');
rocker // { name: 'xlslucky' }
```

切记不能直接将 `exports` 指向一个值，等于切断了 `exports` 和 `module.exports` 之间的联系。
```js
// exports 不再指向 module.exports
exports = 'hello world';
```

下面代码， `hello` 函数无法对外输出， 因为 `module.exports` 被重新赋值了。
```js
exports.hello = function () {
  console.log('say hello!')
}
module.exports = 'hello world!';
```

如果模块只对外输出一个单一的值，不能使用 `exports` , 只能使用 `module.exports` 输出。
```js
module.exports = function (x) {console.log(x)}
```

> 如果你觉得 `exports` 和 `module.exports` 很难分清，那就放弃使用`exports`, 只使用 `module.exports` 输出。

### 加载模块（ require ）

`require` 基本功能就是，读入并执行一个javascript文件，然后返回该模块的`exports`对象，如果没有发现指定模块，那就会报错。

```js
// rocker.js
exports.say = function () {
  console.log('hi!')
}

exports.message = '你好！';

// index.js
var rocker = require('rocker');
rocker // { say: [Function], message: '你好！' }
```

如果输出一个函数，那就不能定义在 `exports` 对象上，而要定义在 `module.exports` 变量上面。
下面代码，`require` 命令调用自身，等于执行 `module.exports` 。
```js
module.exports = function() {
  console.log('hello!')
}
require('test.js')() // hello!
```

`require` 命令加载文件后缀名默认为 `.js` 。
模块可使用相对路径 `./` 开头， 或者绝对路径 `/` 或 `C:` 开头。
```js
var foo1 = require('./foo');
var foo2 = require('./foo.js');
var foo3 = require('/home/user/foo');
var foo4 = require('/home/user/foo.js');
```

第一次加载某个模块，node会缓存该模块。以后再加载该模块，直接从缓存中取出该模块的 `module.exports` 属性。
```js
// rocker.js
exports.say = function () {
  console.log('hi!')
}

exports.message = '你好！';

// index.js
require('./rocker');
require('./rocker').name = "xuang";
console.log(require('./rocker')) // { say: [Function], message: '你好！', name: 'xuang' }
```
证明 `require` 命令并没有重新加载模块文件，而是输出了缓存。

## npm

> npm是随同nodejs一起安装的包管理工具

```shell
# 帮助
$ npm --help || npm -h
# 下载第三方包
$ npm install <pkg>[@<version>]
# 全局安装
$ npm install <pkg> -g
# 安装package.json里的依赖包
$ npm install
```

npm 版本号，语义版本号分为 `X.Y.Z` 三位，分别代表主版本号、次版本号和补丁版本号。

* 如果只是修复bug，需要更新z位。
* 如果是更新了功能，但向下兼容，需要更新y位。
* 如果有大变动，向下不兼容，需要更新x位。
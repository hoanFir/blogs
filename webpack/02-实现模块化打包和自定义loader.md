

## 一、模块化打包

Webpack 本质目标：**将零散的各个 JavaScript 模块文件打包到一个 js 文件中**。例如把 `/src/index.js`、`/src/modules.js` 两个个资源都打包到 `/dist/build.js` 中，再在 `/src/index.html` 里引入 build.js。

```javascript

const path = require('path');

module.exports = {

  entry: './src/index.js',
  
  //webpack 配置文件运行在 NodeJS 环境中，因此可以直接使用 path 等内置模块
  output:  {
    filename: 'bundle.js',
    path: path.join(__dirname, 'output')
  }
}

```

生成的 bundle.js

```javascript

//1
(function(modules) {})([...]);


//2
(function(modules) {
  ...
})([
  //每个函数对应一个模块
  (function(module, __webpack_exports__, __webpack_require__) {...}),
  (function(module, __webpack_exports__, __webpack_require__) {...}),
]);


//3
(function(modules) {

  //缓存加载过的模块
  var installedModules = {};
  
  //用于加载指定模块
  function __webpack_require__(moduleId) {...};

  //在__webpack_require__上挂载一些其他的数据和工具函数
  __webpack_require__.a = "";
  __webpack_require__.b = function(){};
  __webpack_require__.s = 0;
  
  ...
  
  //调用上面定义的 __webpack_require__ 函数
  //传入moduleId = 0的模块，即一开始会加载入口模块
  return __webpack_require__(__webpack_require__.s = 0);
  
})([
  //每个函数对应一个模块
  (function(module, __webpack_exports__, __webpack_require__) {...}), //入口模块
  (function(module, __webpack_exports__, __webpack_require__) {...}),
]);


//4
(function(modules) {

  //缓存加载过的模块
  var installedModules = {};
  
  //用于加载指定模块
  function __webpack_require__(moduleId) {
    if(installedModules[moduleId]) {
      return installedModules[moduleId].exports;
    }
    
    var module = installedModules[moduleId] = {
      i: moduleId, //id
      l: false, //loaded
      exports: {}
    }
    
    //(function(module, __webpack_exports__, __webpack_require__) {...})
    modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
    
    module.l = true;
    
    return module.exports;
  };

  //在__webpack_require__上挂载一些其他的数据和工具函数
  __webpack_require__.a = "";
  __webpack_require__.b = function(){};
  __webpack_require__.s = 0;
  
  ...
  
  //调用上面定义的 __webpack_require__ 函数
  //传入moduleId = 0的模块，即一开始会加载入口模块
  return __webpack_require__(__webpack_require__.s = 0);
  
})([
  //每个函数对应一个模块
  (function(module, __webpack_exports__, __webpack_require__) {...}), //入口模块
  (function(module, __webpack_exports__, __webpack_require__) {...}),
]);


//5
(function(modules) {

  //缓存加载过的模块
  var installedModules = {};
  
  //用于加载指定模块
  function __webpack_require__(moduleId) {
    if(installedModules[moduleId]) {
      return installedModules[moduleId].exports;
    }
    
    var module = installedModules[moduleId] = {
      i: moduleId, //id
      l: false, //loaded
      exports: {}
    }
    
    //执行模块函数
    //(function(module, __webpack_exports__, __webpack_require__) {...})
    modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
    
    module.l = true;
    
    return module.exports;
  };

  //在__webpack_require__上挂载一些其他的数据和工具函数
  __webpack_require__.a = "";
  __webpack_require__.b = function(){};
  __webpack_require__.s = 0;
  
  ...
  
  //调用上面定义的 __webpack_require__ 函数
  //传入moduleId = 0的模块，即一开始会加载入口模块
  return __webpack_require__(__webpack_require__.s = 0);
  
})([
  //每个函数对应一个模块
  (function(module, __webpack_exports__, __webpack_require__) { //入口模块
  
    //r函数用于给导出对象exports定义一个属性：__esModule: {value: true}
    __webpack_require__.r(__webpack_exports__);
    
    //调用其他模块
    var _module_js__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__(1);
    const module = Object(_module_js__WEBPACK_IMPORTED_MODULE_0__['default'])();
    
    //当前模块的业务代码
    ...
    
  }),
  (function(module, __webpack_exports__, __webpack_require__) {
    
    //r函数用于给导出对象exports定义一个属性：__esModule: {value: true}
    __webpack_require__.r(__webpack_exports__);
    
    __webpack__exports['default'] = (() => {
    
      //当前模块的业务代码
      ...
      
    })
    
  })
]);

```

## 二、通过Loader机制加载资源

Webpack 默认只能处理 JavaScript 代码。对于其他类型的资源，支持通过 Loader 机制在 js 文件中以模块化的方式载入。

```

you may need an appropriate loader to handle the file type.

```

- 为什么要在 js 中加载其他任意资源/为什么不分离业务逻辑和其他内容

假设页面上需要用到一个样式模块和一个图片文件，如果用传统的方式：将资源文件单独在html文件里引入，然后再到js文件中添加对应的逻辑代码。这样遗留的问题就是：如果要删除这个局部内容，就需要同时维护html和js文件。

因此，在 js 中加载其他任意资源是 webpack 的设计哲学，所有资源通过 js 来控制，就只需要维护 js 代码这一条线。


- css-loader

css-loader 只会把 css 模块加载到 js 代码中，还需要一个 style-loader 把转化后的代码通过 style 标签添加到页面上。


```javascript

const path = require('path');

module.exports = {

  entry: ...,
  output:  {
    filename: ...,
    path: path.join(__dirname, '...')
  },
  
  module: {
    rules: [
      test: /\.css$/,
      use: ['style-loader', 'css-loader'], //注意rules处理顺序是从数组由后往前，因此style-loader要放在第一个
    ]
  }
}

```

- 无法通过js代码表示的资源模块

如图片或字体文件。一般 loader 会将它们作为单独的资源拷贝到 dist 目录下。然后将其对应的访问路径作为导出成员暴露给外部。

- ...


## 三、自定义loader


实现 “Markdown -> markdown-loader -> HTML”。注意，最后loader返回的一定是一段js代码。

- 用一个loader实现

markdown-loader.js

```javascript

const marked = require('marked');

module.exports = source => {
  //marked 能将md解析为html字符串
  const html = marked(source);
  
  //把解析后的html字符串拼接为一段js代码
  const code = `module.exports=${JSON.stringify(html)}`; //小技巧：用JSON.stringify将字符串转化为标准的json格式字符串，再进行拼接，就不会有问题，因为原字符串内容的特殊字符，如换行符或引号，可能会造成语法上的错误，所以需要用JSON.stringify转义一下
  
  //也可以使用es module
  //const code = `export default ${JSON.stringify(html)}`
  
  return code;
}

```

webpack.config.js

```javascript
const path = require('path');

module.exports = {

  entry: ...,
  output:  {
    filename: ...,
    path: path.join(__dirname, '...')
  },
  
  module: {
    rules: [
      test: /\.md$/,
      use: './markdown-loader',
    ]
  }
}
```


- 用多个loader实现

markdown-loader.js

```javascript

const marked = require('marked');

module.exports = source => {
  const html = marked(source);
  
  return html;
}

```

webpack.config.js

```javascript
const path = require('path');

module.exports = {

  entry: ...,
  output:  {
    filename: ...,
    path: path.join(__dirname, '...')
  },
  
  module: {
    rules: [
      test: /\.md$/,
      use: ['html-loader', './markdown-loader'],
    ]
  }
}
```





## 一、模块化打包

Webpack 本质目标：将零散的各个 JavaScript 模块文件打包到一个 js 文件中。

对于其他类型的资源，支持通过 Loader 机制在 js 文件中以模块化的方式载入。例如把 `/src/index.js`、`/src/modules.js`、`/src/index.css` 三个资源都打包到 `/dist/build.js` 中，再在 `/src/index.html` 里引入 build.js。

- 打包

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
  (function(module, __webpack_exports__, __webpack_require__) {
    
  }),
]);

```



## 二、自定义loader


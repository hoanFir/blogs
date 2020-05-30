

## 一、模块化打包

Webpack 本质目标：将零散的各个 JavaScript 模块文件打包到一个 js 文件中。对于其他类型的资源，支持通过 Loader 机制在 js 文件中以模块化的方式载入。例如把 `/src/index.js`、`/src/modules.js`、`/src/index.css` 三个资源都打包到 `/dist/build.js` 中，再在 `/src/index.html` 里引入 build.js。

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



## 二、自定义loader


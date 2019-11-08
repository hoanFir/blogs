🐾 webpack-dev-server

🕘 2019.11.08 由 hoanfirst 编辑


基于`webpack-dev-server`+`webpack`。

```json
  
  "scripts": {
    "dev": "cross-env NODE_ENV=development webpack-dev-server --config ./build/webpack.config.dev.js",
    "build": "cross-env NODE_ENV=production webpack --env production --config ./build/webpack.config.pro.js -p",
  },

```

1. cross-env

cross-env: run scripts that set and use environment variables across platforms. 

```json

# OS X, Linux
PORT=3000 webpack-dev-server ...

# Windows(cmd.exe)
set PORT=3000 && webpack-dev-server ...

# use cross-env for all platforms
cross PORT=3000 webpack-dev-server ...

```

2.webpack -p

compress scripts...


---


在没有使用`webpack-dev-server`之前，我们需要在本地手动起一个服务，如使用`node+express`，再配置`webpack`，具体[参照](https://github.com/hoanFir/blogs/blob/master/webpack/webpack%E5%B8%B8%E7%94%A8%E9%85%8D%E7%BD%AE.md)。

webpack-dev-server can be used to quickly develop an application, privides a simple web server and the abilitiy to use live reloading.

官方示例：

```javascript

const path = require('path');

const HtmlWebpackPlugin = require('html-webpack-plugin');

const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {

  mode: 'development',
   
  entry: {
    app: './src/index.js',
    print: './src/print.js',
  },
  
  plugins: [
    new CleanWebpackPlugin();
    new HtmlWebpackPlugin({
      title: 'development',
    }),
  ],
  
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  }
}

```

---

1. webpack.config.base.js

2. webpack.config.dev.js 开发环境

3. webpacl.config.js 生产环境


### webpack.config.base.js

### webpack.config.dev.js

### webpack.config.pro.js


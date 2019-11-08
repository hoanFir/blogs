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

配置环境变量。在webpack赋给应用的全局变量，在js业务逻辑中使用。

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

```javascript

const webpack = require("webpack");
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const ExtractTextPlugin = require("extract-text-webpack-plugin");
const autoprefixer = require("autoprefixer");

const basePath = path.resolve(__dirname, '../');

module.exports = {
  target: 'web',
  entry: {
    index: path.resolve(basePath, 'src', 'index.js'),
    commons: ['antd', 'react', 'react-dom', 'react-router-dom', 'monent', 'redux']
  },
  output: {
    filename: 'app.min.js',
    chunkFilename: 'chunk/[name].min.js'
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        loader: 'babel-loader'
      },
      {
        test: /\.(css|less)$/,
        use: [
          {
            loader: 'style-loader',
          },
          {
            loader: 'css-loader',
          },
          {
            loader: 'less-loader',
            options: {
               plugins: ()=>[autoprefixer({
                browsers: ['last 5 versions']
               })]
            }
          }
        ]
      },
      {
        test: /\.(ico|png|gif|jpg|jpeg)$/,
        loader: 'url-loader'
      },
      {
        test: /\.(eot|svg|ttf|woff)\.??.*$/,
        loader: 'url-loader?name=fonts/[name].[md5:hash:hex:7].[ext]'
      }, 
    ] 
  },
  plugins: [
    new HtmlWebpackPlugin({
      finename: '../index.html',
      template: 'src/index.ejs'
    }),
    new webpack.optimize.CommonsChunkPlugin({
      name: 'commons',
      filename: 'commons.js
    }),
    new ExtractTextPlugin({
      filename: 'css/app.min.css'
    }),
    new webpack.DefinePlugin({
      'process.env': {
          NODE_ENV: JSON.stringify(process.env.NODE_ENV)
      }
    })
  ]
}

```

### webpack.config.dev.js

```javascript

const webpack = require('webpack');
const merge = require('webpack-merge');
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

const baseConfig = require('./webpack.config.base');

const basePath = path.resolve(__dirname, '../');

const devDomain = 'local.jd.com';
const devPort = 8006;

```

### webpack.config.pro.js

```javascript

const path = require('path');
const baseConfig = require('./webpack.config.base');
const merge = require('webpack-merge');

const basePath = path.resolve(__dirname, '../');

module.exports = merge({}, baseConfig, {

  output: {
    path: path.resolve(basepath, '../src/main/webapp/WEB-INF/resources/'),
    // publicPath: '/resource/',
    publicPath: '/static-product.enterprise.com/resource/',
  },
  module: {
    rules: [{
      test: /\.ejs$/,
      loader: 'html-loader'
    }]
  },
})

```

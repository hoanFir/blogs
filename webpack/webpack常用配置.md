🐾 webpack常用配置

🕘 2019.10.28 由 hoanfirst 编辑



- entry

```javascript
const path = require('path');

module.exports = {
  entry: {
    app: path.resolve(__dirname, '../'),
  }
}
```

- output

```javascript
const path = require('path');

module.exports = {
  output: {
      path: path.resolve(__dirname, '../dist/'),
      filename: '[name].js',
  },
}
```

- HtmlWebpackPlugin

The plugin will generate an HTML5 file for you that includes all your webpack bundles in the body using script tags.

tips：all javascript resources will be placed at the bottom of the body element by default。


```javascript

const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      // Default: 'index.html'
      filename: '../index.html',
      template: 'src/index.ejs',
    })
  ]
}

```

- webpack.optimize.CommonsChunkPlugin(options);

tips：webpack v4 开始使用`SplitChunksPlugin`。

```javascript
const path = require('path');
const webpack = require('webpack');

module.exports = {
  entry: {
    app: path.resolve(__dirname, '../'),
    commons: ['babel-polyfill', 'antd', 'react', 'react-dom', 'react-router-dom'],
  },
  output: {
      path: path.resolve(__dirname, '../dist/'),
      filename: '[name].js',
      chunkFilename: 'chunk/[name].js'
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({
      name: 'commons',
      fiename: 'common.js',
    })
  ]
}
```

- ExtractTextWebpackPlugin

This plugin extracts CSS into separate files. It creates a CSS file per JS file which contains CSS. 当输出的app.js过大，可以将里面的css内容打包成一个单独的文件。

tips: webpack v4 开始使用`mini-css-extract-plugin`。

```javascript

const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  module: {
    rules: [{
      test: /\.(css|less)$/,
      use: ExtractTextPlugin.extract({
        fallback: 'style-loader',
        use: [{
          loader: 'css-loader'
        }, {
          loader: 'less-loader'
        }]
      })
    }]
  },
  plugins: [
    new ExtractTextPlugin({
      filename: 'css/app.min.css'
    })
  ]
}
```

- happypack

HappyPack makes webpack builds faster by allowing you to transform multiple files `in parallel`.

```javascript

const HappyPack = require('happypack');
const happyThreadPool = HappyPack.ThreadPool({ size: 2 });

modules.exports = {
  // replace your current loaders with HappyPack's loader
  module: {
    rules: [{
      test: /\.(js/jsx)$/,
      exclude: /node_modules/,
      loader: 'happpypack/loader?id=happyBabel',
    }]
  }
  plugins: [
    new HappyPack({
      id: 'happyBabel',
      // loaders is the only required parameter
      loaders: [{
        loader: 'babel-loader?cacheDirectory=true'
      }],
      threadPool: happyThreadPool || 2,
    })
  ]
}

```




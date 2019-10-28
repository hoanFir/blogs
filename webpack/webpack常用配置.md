🐾 webpack常用配置

🕘 2019.10.28 由 hoanfirst 编辑


### webpack.config.base.js

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

- module

```javascript

module.exports = {
  module: {
    rules: [{
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        loader: 'happypack/loader?id=happyBabel',
    }, {
        test: /\.(css|less)$/,
        use: ExtractTextPlugin.extract({
            fallback: 'style-loader',
            use: [{
                loader: 'css-loader'
            }, {
                loader: 'less-loader'
            }]

        })
    }, {
      test: /\.(ico|png|gif|jpg|jpeg)$/,
      loader: 'url-loader'
    }, {
      test: /\.(eot|svg|ttf|woff|woff2)\??.*$/,
      loader: 'url-loader?name=fonts/[name].[md5:hash:hex:7].[ext]'
    }]
  }
}

```

- plugins:html-webpack-plugin

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

- plugins:webpack.optimize.CommonsChunkPlugin(options);

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

- plugins:extract-text-webpack-plugin

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

- plugins:happypack

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


### webpack.config.dev.js

- module

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');

module.exports = merge(webpackBaseConfig, {
  module: {
    rules: [
      {
        test: /\.md$/,
        loader: 'html-loader'
      },
      {
        test: /\.ejs$/,
        use: [
          'ejs-loader',
          {
            loader: 'ejs-html-loader',
            options: {
              webTitle: 'xx平台首页',
              language: 'zh_CN',
              resourcePath: '',
              resourceVersion: '',
              userPin: '',
              requestURI: ''
            }
          }
        ]
      }
    ]
  }
})

```

tips：webpack处理模板文件（如.ejs）有两种方式，一种是将模板文件当作一个字符串（html-loader），另一种是将模板文件当作一个已经编译好的模板函数（ejs-loader）。

- devtool

```javascript

module.exports = merge(webpackBaseConfig, {
    devtool: '#cheap-module-eval-source-map',
})

```
- plugins:html-webpack-plugin

```javascript

const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.ejs',
    })
  ]
}

```

- plugins:webpack.HotModuleReplacementPlugin

Enabling HMR is easy and in most cases no options are necessary.

```javascript

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
  ]
}

```

- plugins:webpack.NoErrorsPlugin

跳过编译时出错的代码并记录，使编译后运行时的包不会发生错误。

```javascript

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new webpack.NoErrorPlugin(),
  ]
}

```

- plugins:friendly-errors-webpack-plugin

recognizes certain classes（识别特定类别） of webpack errors and cleans（清理）, aggregates（聚合） and prioritizes（按优先顺序） them to provide a better Developer Experience.
 
```javascript

const FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new FriendlyErrorsPlugin(),
  ]
}

```

- plugins:copy-webpack-plugin

将已经存在的单个文件或整个目录复制到build directory。一般应用于静态资源文件。

tips：webpack-copy-plugin is not designed to copy files generated from the build process; rather, it is to copy files that already exist in the source tree, as part of the build process.

```javascript

const CopyWebpackPlugin = require('copy-webpack-plugin');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new CopyWebpackPlugin([{
      from: path.resolve(__dirname, '../src/editor'),
      to: path.resolve(path.resolve(__dirname, '../'), 'dist')
    }]),
    new CopyWebpackPlugin([{
      from: path.resolve(__dirname, '../src/iconfonts'),
      to: path.resolve(path.resolve(__dirname, '../'), 'dist')
    }]),
  ]
}

```

- plugins:Webpack.DefinePlugin

The DefinePlugin allows you to create global constants which can be configured at compile time. This can be useful for allowing different behavior between development builds and production builds. Such as determine whether `logging` takes place(development build or production build).

```javascript

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new webpack.DefinePlugin({
      "process.env.NODE_ENV": JSON.stringify("development")
    }),
  ]
}

```

### webpack.config.pro.js

- output

```javascript

const version_current = moment(new Date()).format('YYYYMMDD');

module.exports = merge(webpackBaseconfig, {
  output: {
      path: path.resolve(path.resolve(__dirname, '../'), '../src/main/webapp/WEB-INF/resource/'+version_current+'/'),
      publicPath: '//static-product.enterprise.com/resource/' + version_current+'/'
  },
})
```

- module

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');

module.exports = merge(webpackBaseConfig, {
  module: {
    rules: [
      {
        test: /\.md$/,
        loader: 'html-loader'
      },
      {
        test: /\.ejs$/,
        loader: 'html-loader'
      }
    ]
  }
})

```

- devtool

```javascript

module.exports = merge(webpackBaseConfig, {
    devtool: false,
})

```

- plugins:HtmlWebpackPlugin

```javascript

const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new HtmlWebpackPlugin({
      filename: '../../index.html',
      template: 'src/index.ejs',
    })
  ]
}

```

- plugins:Webpack.DefinePlugin

The DefinePlugin allows you to create global constants which can be configured at compile time. This can be useful for allowing different behavior between development builds and production builds. Such as determine whether `logging` takes place(development build or production build).

```javascript

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new webpack.DefinePlugin({
      "process.env.NODE_ENV": JSON.stringify("production")
    }),
  ]
}

```

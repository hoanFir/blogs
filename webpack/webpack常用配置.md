🐾 webpack常用配置

🕘 2019.10.28 由 hoanfirst 编辑

### 概述

基于`node+express`+`webpack-dev-middleware`+`webpack`。

package.json

```json
  
  "scripts": {
    "start": "node build/dev-server.js",
    "build-prod": "node build/build-pro.js"
  },
  
```

node build/dev-server.js

tips：基于express构建本地服务器。

```javascript

const opn = require('opn');
const path = require('path');
const webpack = require('webpack');
const express = require('express');
const proxyMiddleware = require('http-proxy-middleware');
const webpackDevMiddleware = require('webpack-dev-middleware');
const webpackHotMiddleware = require('webpack-hot-middleware');

const webpackConfig = require('./webpack.config.dev');

const port = 8005;
const proxys = {
            '/api': {
                target: 'http://uat.product.company.com',
                changeOrigin: true
            },
            '/auth': {
                target: 'http://uat.product.company.com/',
                changeOrigin: true
            },
            '/other/api': {
                target: 'http://localhost:3000/',
                changeOrigin: true
            },
        };

const uri = 'http://local.company.com:' + port;

const app = express();

const compiler = webpack(webpackConfig);

const devMiddleware = webpackDevMiddleware(compiler, {
    publicPath: '/',
    stats: {
        colors: true
    }
});

Object.keys(proxys).forEach(function (context) {
    let options = proxys[context];
    if (typeof options === 'string') {
        options = {target: options}
    }
    app.use(proxyMiddleware(options.filter || context, options))
});

const hotMiddleware = webpackHotMiddleware(compiler);

app.use(devMiddleware);
app.use(hotMiddleware);

// serve pure static assets
let staticPath = path.posix.join('/', 'src');
app.use(staticPath, express.static('./src'));

devMiddleware.waitUntilValid(() => {
    opn(uri);
});

app.listen(port);

```

node build/build-pro.js

```javascript

const webpack = require('webpack');
const webpackProductionConfig = require('./webpack.config.pro');

webpack(webpackProductionConfig, (error) => {
  console.log(error);
})

```

---

1. webpack.config.base.js
2. webpack.config.dev.js 开发环境
3. webpack.config.pro.js 生产环境

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
      chunkFilename: 'chunk/[name].js',
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

tips：webpack can process template files(such as .ejs) in two ways. one is to use `html-loader` and treat the ejs file as a string; the other is to use `ejs-loader` and treat the ejs file as a template.

- devtool

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');

module.exports = merge(webpackBaseConfig, {
    devtool: '#cheap-module-eval-source-map',
})

```

When webpack bundles source code, for example, bundle some files into one bundle.js, if one of the source files contains an error, the stack trace will simply point to bundle.js, this isn't helpful as we probably want to know exactly which source file the error came from.

**In order to track down errors and warnings**, javascript offers `source maps`.

Source maps: map complied code back to original source code. 

devtool: false - build fastest, rebuild fastest, with bundled code

devtool: cheap-module-eval-source-map - build slow, rebuild faster, with original source(lines only)

devtool: inline-cheap-module-source-map - buld slow, rebuild slower, with original source(lines only)


- plugins:html-webpack-plugin

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
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

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const webpack = require('webpack');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
  ]
}

```

- plugins:webpack.NoErrorsPlugin

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const webpack = require('webpack');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new webpack.NoErrorPlugin(),
  ]
}

```

webpack.NoErrorsPlugin(), an optional plugin that tells the reloader to not reload if there is an error. The error is simply printed in the console, and the page does not reload. 

If you do not have this plugin enabled and you have an error, a red screen of death is thrown. 即先处理完报错才会重载页面，不至于在频繁处理报错的过程中频繁渲染页面。


- plugins:friendly-errors-webpack-plugin

recognizes certain classes（识别特定类别） of webpack errors and cleans（清理）, aggregates（聚合） and prioritizes（按优先顺序） them to provide a better Developer Experience.
 
```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
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

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const path = require('path');
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

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const webpack = require('webpack');

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

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const webpack = require('webpack');
const path = require("path");

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

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');

module.exports = merge(webpackBaseConfig, {
    devtool: false,
})

```

- plugins:html-webpack-plugin

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
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

- plugins:copy-webpack-plugin

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const path = require('path');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new CopyWebpackPlugin([{
      from: path.resolve(__dirname, '../src/editor'),
      //发布时editor不随版本升级，放到resource目录下
      to: path.resolve(path.resolve(path.resolve(__dirname ,'../'), '../src/main/webapp/WEB-INF/resources/'), '/editor')
    }]),
  ]
}

```

- plugins:Webpack.DefinePlugin

The DefinePlugin allows you to create global constants which can be configured at compile time. This can be useful for allowing different behavior between development builds and production builds. Such as determine whether `logging` takes place(development build or production build).

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const webpack = require('webpack');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new webpack.DefinePlugin({
      "process.env.NODE_ENV": JSON.stringify("production")
    }),
  ]
}

```

- plugins:webpack-parallel-uglify-plugin

并行压缩/丑化js代码。

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const ParallelUglifyPlugin = require('webpack-parallel-uglify-plugin');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new ParallelUglifyPlugin({
      uglifyJS: {
        output: {
          //是否保留空格和制表符。设置false达到更好的压缩效果。
          beautify: false,
          //是否保留代码中的注释。设置false达到更好的压缩效果。
          comments: false,
        }
        compress: {
          warning: false,
          //是否删除代码中的console语句。设置true达到更好的压缩效果
          drop_console: true,
          //是否转换虽然已经定义但只用到一次的变量。如var x=1; y=x;会转换成y=5.
          collapse_vars: true,
          //是否提取出出现多次但没有定义成变量去引用的静态值。如x = 'x'; y = 'x';会转换成var a = 'x';x=a, y=a;
          reduce_vars: true,
        }
      }
    })
  ]
}

```

- plugins:optimize-css-assets-webpack-plugin

优化/压缩css代码。

tips：要考虑和`extract-text-webpack-plugin`一起使用时产生的重复问题。

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const ExtractTextPlugin = require('extract-text-webpack-plugin');
const OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin');

module.exports = merge(webpackBaseconfig, {
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
  }
  plugins: [
    new ExtractTextPlugin({
      filename: "css/app.min.css"
    })
    new OptimizeCSSPlugin({
      //cssProcessorOptions: The options passed to the cssProcessor, defaults to {}
      //cssProcessor: The css processor used to optimize/minimize the css, defaults to cssnano.
      cssProcessorOptions: {
        safe: true
      }
    }),
  ]
}

```

- plugins:compression-webpack-plugin

```javascript

const merge = require('webpack-merge');
const webpackBaseConfig = require('./webpack.config.base');
const CompressionPlugin = require('compression-webpack-plugin');

module.exports = merge(webpackBaseconfig, {
  plugins: [
    new CompressionPlugin({
      assert: '[path].gz[query]', //app.js.gz or commons.js.gz
      algorithm: 'gzip',
      test: /\.js$|\.css$/,
      threshold: 10240,
      minRatio: 0.8,
    })
  ]
}

```

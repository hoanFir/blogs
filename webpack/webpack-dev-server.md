🐾 webpack-dev-server

🕘 2019.11.08 由 hoanfirst 编辑

在没有使用`webpack-dev-server`之前，我们需要在本地手动起一个服务，如使用`node+express`，再配置`webpack`，具体[参照](https://github.com/hoanFir/blogs/blob/master/webpack/webpack%E5%B8%B8%E7%94%A8%E9%85%8D%E7%BD%AE.md)。

webpack-dev-server can be used to quickly develop an applicatioin.

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


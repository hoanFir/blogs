🐾 webpack常用配置

🕘 2019.10.28 由 hoanfirst 编辑



- entry

const path = require('path');

module.exports = {
  entry: {
    app: path.resolve(__dirname, '../'),
  }
}

- output

const path = require('path');

module.exports = {
  output: {
      path: path.resolve(__dirname, '../dist/'),
      filename: '[name].js',
      chunkFilename: 'chunk/[name].js'
  },
}

- webpack.optimize.CommonsChunkPlugin(options);

const path = require('path');
const webpack = require('webpack');

module.exports = {
  entry: {
    app: path.resolve(__dirname, '../'),
    commons: ['babel-polyfill', 'antd', 'react', 'react-dom', 'react-router-dom'],
  }
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({
      name: 'commons',
      fiename: 'common.js',
    })
  ]
}


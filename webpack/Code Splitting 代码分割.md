
Code splitting is one of the most compelling features of webpack.


### 一、打包和代码分割

目前现在大多数应用都会使用 `Webpack`, `Rollup` or `Browserify` 这类的构建工具来**打包**文件。

- 打包

将文件引入合并到一个单独文件，形成一个“bundle”，接着在页面上引入该 bundle，整个应用即可一次性加载。

- 代码分割

随着应用规模增长，代码也随之增长，尤其是在整合了体积巨大的第三方库的情况下，导致包体积过大加载时间过长。为了解决这个问题，代码分割技术能够创建多个包并在运行时动态加载。


### 二、代码分割的三种方案

Code splitting allows you to split code into various bundles which can then be loaded on demand or in parllel.

There are three approaches to code splitting:

- Entry Points: Manually split code using webpack `entry` configuration

- Prevent Duplication: Use the webpack `SplitChunkPlugin`

- Dynamic Imports: Split code via inline function calls within modules



### 三、方案一 / Entry Points

project

```

webpack-demo
|- package.json
|- webpack.config.js
|- /dist
|- /src
  |- index.js
  |- another-module.js
|- /node_modules

```

webpack.config.js

```javascript

const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
    another: './src/another-module.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};

```

npm run build

```

...
            Asset     Size   Chunks             Chunk Names
another.bundle.js  550 KiB  another  [emitted]  another
  index.bundle.js  550 KiB    index  [emitted]  index
Entrypoint index = index.bundle.js
Entrypoint another = another.bundle.js
...

```

注意，该方案有如下两种不足：

1. If there are any duplicated modules between entry chunks they will be included in both bundles

如果在 `./src/index.js` 和 `./src/another-module.js` 中 import 相同的依赖，如 `lodash`，会重复引入，导致两个模块文件都很大

2. It isn't as flexible and can't be used to dynamically split code with the core application logic


### 四、方案二 / Prevent Duplication

该方案主要是为了解决引入相同依赖的问题。

webpack.config.js

tips: the `dependOn` option allows to share the modules(i.e. loadsh) between the chunks

```javascript

 const path = require('path');

  module.exports = {
    mode: 'development',
    entry: {
      index: { import: './src/index.js', dependOn: 'shared' },
      another: { import: './src/another-module.js', dependOn: 'shared' },
      shared: 'lodash',
    },
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
  };

```

**Using the `SplitChunksPlugin`**

The `SplitChunksPlugin` allows us to extract common dependencies into an existing entry chunk or an entirely new chunk.

tips: The `CommonsChunkPlugin` has been removed in webpack v4 legato. In the latest version, using the `SplitChunksPlugin`.

webpack.config.js

```javascript

  const path = require('path');

  module.exports = {
    mode: 'development',
    entry: {
      index: './src/index.js',
      another: './src/another-module.js',
    },
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
    optimization: {
      splitChunks: {
        chunks: 'all',
      },
    },

```

With the `optimization.splitChunks` configuration option, we should now see the duplicate dependency removed from our `index.bundle.js` and `another.bundle.js`, and run

npm run build

```

...
                          Asset      Size                 Chunks             Chunk Names
              another.bundle.js  5.95 KiB                another  [emitted]  another
                index.bundle.js  5.89 KiB                  index  [emitted]  index
vendors~another~index.bundle.js   547 KiB  vendors~another~index  [emitted]  vendors~another~index
Entrypoint index = vendors~another~index.bundle.js index.bundle.js
Entrypoint another = vendors~another~index.bundle.js another.bundle.js

...

```


### 五、方案三 / Dynamic Imports

using the `import() syntax` that the ECMAScript proposal for dynamic imports.

tips: `import()` calls use promises internally.

project

```

webpack-demo
|- package.json
|- webpack.config.js
|- /dist
|- /src
  |- index.js
|- /node_modules

```

webpack.config.js

```javascript


const path = require('path');

  module.exports = {
    mode: 'development',
    entry: {
      index: './src/index.js',
    },
    output: {
      filename: '[name].bundle.js',
      chunkFilename: '[name].bundle.js',
      publicPath: 'dist/',
      path: path.resolve(__dirname, 'dist'),
    },
  };
  
```

`chunkFileName` option, which determines the name of non-entry chunk files.


Now, instead of **statically** importing `loadsh`, we'll use dynamic importing to separate a chunk:

index.js

```javascript

function getComponent() {

  return import(/* webpackChunkName: "lodash" */ 'lodash').then(({ default: _ }) => {
    const element = document.createElement('div');

    element.innerHTML = _.join(['Hello', 'webpack'], ' ');

    return element;

  }).catch(error => 'An error occurred while loading the component');

}

getComponent().then(component => {
  document.body.appendChild(component);
})

```

`webpackChunkName` in the comment will cause our separate bundle to be named `lodash.bundle.js` instead of just `[id].bundle.js`.

npm run build

```

...
                   Asset      Size          Chunks             Chunk Names
         index.bundle.js  7.88 KiB           index  [emitted]  index
vendors~lodash.bundle.js   547 KiB  vendors~lodash  [emitted]  vendors~lodash
Entrypoint index = index.bundle.js
...

```







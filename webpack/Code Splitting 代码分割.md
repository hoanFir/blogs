
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



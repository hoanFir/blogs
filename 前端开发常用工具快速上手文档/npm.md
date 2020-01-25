npm安装  

```
npm install --save ...
npm i -s ...

npm install --save-de ...
npm i -D ...

npm install -g ...
npm i -g ...
```


更新到最新版

```
npm install -g npm
```


淘宝镜像

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```


npm list --depth 0 获取当前项目的依赖结构

```
+-- css-loader@3.0.0
+-- vue@2.6.10
+-- vue-loader@15.7.0
`-- webpack@4.35.3
```


npm list -g --depth 0 获取全局的依赖结构

```
+-- cnpm@6.0.0
+-- eslint@5.15.1
+-- express-generator@4.16.0
+-- gulp-cli@2.1.0
+-- http-server@0.11.1
+-- less@3.9.0
+-- npm@6.9.0
+-- uglify-js@3.5.4
`-- webpack@4.29.6
`-- webpack-cli@3.3.5
```



强制清除缓存

```
npm cache clean -f / --force
```







插件机制的目标：提高webpack在项目自动化构建方面的能力。


## 一、常见应用场景

- 实现在每次打包之前自动清楚上一次打包的 dist 目录

- 自动生成 html

- 根据不同环境为代码注入类似 API 地址这种可能变化的部分

- 拷贝不需要参与打包的资源文件到 dist 目录

- 压缩打包后的文件

- 自动发布打包结果到服务器

- ...


## 二、实现原理：钩子机制

在 webpack 整个工作过程中，有很多个环节，webpack 本身已经为每个环节都加上了一个钩子，因此在开发插件的时候，可以往钩子上挂载不同的任务实现扩展 webpack 的各种能力。



## 三、开发一个插件

目标：清除 bundle.js 文件里的注释。

tips：webpack 要求插件是一个函数或者一个包含 `apply` 方法的对象


remove-comments-plugin.js

- 1. 首先，以class形式定义

```javascript

class RemoveCommentsPlugin {

  //complier对象包含了构建的所有配置信息
  apply(complier) {
    ...
  }
}

```

- 2. 然后，确定钩子


`emit`，在 webpack 即将向输出目录输出文件时执行。


- 3. 访问钩子，挂载任务函数

通


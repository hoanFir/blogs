
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

在 webpack 整个工作过程中，有很[多个环节](https://github.com/hoanFir/blogs/blob/master/webpack/05-%E5%90%84%E4%B8%AA%E7%8E%AF%E8%8A%82%E7%9A%84%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.md)，webpack 本身已经为每个环节都加上了一个钩子，因此在开发插件的时候，可以往钩子上挂载不同的任务实现扩展 webpack 的各种能力。



## 三、开发一个插件

目标：清除 bundle.js 文件里的注释。

tips：webpack 要求插件是一个函数或者一个包含 `apply` 方法的对象


remove-comments-plugin.js

**1. 首先，以class形式定义**

```javascript

class RemoveCommentsPlugin {

  //complier对象包含了构建的所有配置信息
  apply(complier) {
    ...
  }
}

```

**2. 然后，确定钩子**


`emit`，在 webpack 即将向输出目录输出文件时执行。


**3. 访问钩子，挂载任务函数**

通过 `complier` 对象的 `hooks` 属性访问到 `emit` 钩子，再通过 `tap` 方法注册一个钩子函数。

```javascript

class RemoveCommentsPlugin {

  //complier对象包含了构建的所有配置信息
  apply(complier) {
  
    complier.hooks.emit.tap('RemoveCommentsPlugin', compliation => {
      
      //compliation可以理解为此次打包的context。如compilation.assets = 打包文件列表，如index.html bundle.js...
      
      compilation.assets.keys().map(name =>{
      
        if(name.endWith('.js')) {
        
           //compilation.assets[name].source()获取文件内容
           const contents = compilation.assets[name].source();
           const noCommentsContents = contents.replace(/\/\*{2,}\/\s?/g, "");
           
           compilation.assets[name] = {
            source: () => noCommentsContents,
            size: () => noCommentsContents.length
           }
        }
      })
    })
  }
}

```

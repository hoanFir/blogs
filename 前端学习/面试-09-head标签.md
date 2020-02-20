### 一、head 标签

所有浏览器都支持 `<head>` 头部标签。

The HTML <head> element contains machine-readable information (metadata) about the document, like its title, scripts, and style sheets.

该标签用于定义文档的头部，是所有`头部元素`的容器。`头部元素`，可以提供元信息、引用脚本以及指示浏览器在哪里找到样式表等。


### 二、可以用在`<head>`里的头部元素标签

#### 2.1 meta

可选。

该标签提供页面的元信息（meta-information），比如针对搜索引擎或更新频度的描述，description，和关键词，keywords。

该标签的属性通过`名称/值`对来定义，即元信息总是以`名称/值`的形式被成对传递的。

**必须的属性**

|属性|值  |描述  |
|--|--|--|
|content  |some_text  |定义和 http-equiv 属性 或 name 属性相关的元信息，即和它们搭配使用  |

**可选的属性**

|属性|值  |描述  |
|--|--|--|
|http-equiv  |content-type / expires / refresh / set-cookie / Cache-Control / X-UA-Compatible  |把 content属性 关联到 HTTP 某个头部属性  |
|name  |author / description / keywords / generator / revised / others  |把 content属性 关联到一个名称  |
|scheme  |some_text  |定义用于翻译 content属性值 的格式  |

1. http-equiv属性

把 content属性 关联到 HTTP 某个头部属性。即指示服务器在发送实际文档之前，先在要传给浏览器的 MIME 文档头部，包含`名称/值`对。
在正常情况下，当服务器向浏览器发送文档时，会先发送许多`名称/值`对。并且所有服务器都至少要发送一个：`content-type: text/html`。这将告诉浏览器准备接受一个 HTML 文档。
当然，只有浏览器可以接受这些附加的头部字段，并能以适当的方式使用它们时，这些字段才有意义。
```html
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta http-equiv="Cache-Control" content="no-transform">
	<meta http-equiv="Cache-Control" content="no-siteapp">
```

2. name属性

把 content 属性关联到一个名称。即开发者可以自定义或者说自由地使用对获取文档的用户来说富有意义的名称。
`keywords`是一个经常被使用的名称，它为文档定义了一组关键字。当某些搜索引擎在遇到这些关键字时，会根据这些关键字对文档进行分类。

```html
	<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
	<meta name="keywords" content="DIYgod,blog">
	<meta name="generator" content="Hexo 3.8.0">
	<meta name="theme-color" content="#222">
	<meta name="description" content="人气网红 | 前端萌新 | 有猫 | 开源">
	<meta name="twitter:card" content="summary">
	<meta name="twitter:title" content="Hi, DIYgod">
	<meta name="twitter:description" content="人气网红 | 前端萌新 | 有猫 | 开源">	
```

#### 2.2 link

可选。

#### 2.3 style

必须。

#### 2.4 title

可选。

#### 2.5 script

可选。

#### 2.6 base

可选。

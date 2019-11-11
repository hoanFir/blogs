🐾 graphQL基本使用

🕘 2019.11.11 由 hoanfirst 编辑

在日常前后端数据交互中，存在哪些会造成`字段冗余`的场景？

- 为了兼容多平台，使用同一个套接口

- 多个页面模块调用同一接口，获取各自所需的部分数据（后台将相关数据统一放在该接口返回的数据体里面）

- ...


### graphQL

一种用于API的查询语言。使用 GraphQL 的应用可以工作得又快又稳，因为控制数据的是应用，而不是服务器。

- 请求客户端当前所要的数据，不多不少

```
向API发出一个graphQL请求，就能准确获得想要的数据，不多不少。

graphQL查询总是返回可预测的结果。

graphQL基于类型和字段的方式进行组织。当通过一个单一入口端点得到所有数据，graphQL使用类型来保证应用只请求可能的数据。

{
  hero {
    name
    friends {
      name
      homeWorld {
        name
        climate
      }
      species {
        name
        lifespan
        origin {
          name
        }
      }
    }
  }
}

type Query {
  hero: Character
}
type Character {
  name: String
  friends: [Character]
  homeWorld: Planed
  species: Species
}
type Planet {
  name: String
  climate: String
}
type Species {
  name: String
  lifespan: Int
  origin: Planet
}
```

- 获取多个资源，只用一个请求

```
graphQL查询不仅能获得资源的属性，还能沿着资源之间引用进一步查询。

典型的REST API请求多个资源时得载入多个URL，而graphQL可以通过一次起高球就获取应用所需的所有数据。这样一来，即使是比较慢的移动网络连接下，使用 GraphQL 的应用也能表现得足够迅速。
```



graphQL既是一种用于API的查询语言，也是一个满足数据查询的运行时。 graphQL对API中的数据提供了一套易于理解的完整描述，使得客户端能准确获取当前需要的部分数据，不会有任何冗余，也让API更容易随着时间（需求）的推移而演进。

1. 描述你的数据

```javascript

type Project {
  name: String,
  tagline: String,
  contributors: [User]
}

```

2. 请求你所要的数据

```javascript

{
  project(name: 'GraphQL') {
    tagline
  }
}

```

1. 得到可预测的结果

```javascript

{
  "project": {
    "tagline": "a query language for APIs."
  }
}

```

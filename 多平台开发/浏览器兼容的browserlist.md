
在开发前端项目时，经常能看到脚手架默认配置了一个 `browserlist` 字段。


[browserlist - npm](https://www.npmjs.com/package/browserslist)

### 配置方式

1. `browserlist` key in `package.json` file(recommended)

2. `.browserlist` config file

3. `browserlist` config file

4. `BROWSERLIST` environment variable

...

### 默认

`> 0.5%, last 2 versions, Firefox ESR, not dead`. tips: `,`  is equal to `or`.

- `>0.5%`：全球使用率统计大于 0.5% 浏览器版本。如果是 `> 0.5% in US`，就是根据 USA 的统计数据

- `last 2 version`：指每个浏览器的最后两个版本。

- `dead`：指没有官方支持或更新超过24个月的浏览器，如 IE 10, IE_Mob 10, BlackBerry 10, BlackBerry 7, Samsung 4 and OperaMobile 12.1。

使用 `npx browserlist` 查看支持的浏览器：

```
and_chr 80
and_ff 68
and_qq 1.2
and_uc 12.12
android 80

baidu 7.12

chrome 80
chrome 79

edge 80
edge 79
edge 18

firefox 74
firefox 73
firefox 72
firefox 68

ie 11

ios_saf 13.3
ios_saf 13.2
ios_saf 13.0-13.1
ios_saf 12.2-12.4

kaios 2.5

op_mini all
op_mob 46
opera 66
opera 65

safari 13
safari 12.1

samsung 11.1
samsung 10.1
```

### browsers

```
Android for Android WebView.

Baidu for Baidu Browser.

BlackBerry or bb for Blackberry browser.

Chrome for Google Chrome.
ChromeAndroid or and_chr for Chrome for Android

Edge for Microsoft Edge.

Electron for Electron framework. It will be converted to Chrome version.

Explorer or ie for Internet Explorer.
ExplorerMobile or ie_mob for Internet Explorer Mobile.

Firefox or ff for Mozilla Firefox.
FirefoxAndroid or and_ff for Firefox for Android.

iOS or ios_saf for iOS Safari.

Node for Node.js.

Opera for Opera.
OperaMini or op_mini for Opera Mini.
OperaMobile or op_mob for Opera Mobile.

QQAndroid or and_qq for QQ Browser for Android.

Safari for desktop Safari.

Samsung for Samsung Internet.

UCAndroid or and_uc for UC Browser for Android.

kaios for KaiOS Browser.
```


### Combiner types

1. union

`or`, `,`

取并集

`> .5% or last 2 versions`

`> .5%, last 2 versions`

2. intersection

`and`

取交集

`> .5% and last 2 versions`

3. relative complement

`not`

差集

`> .5% not last 2 versions` 取`> .5%`的差集

`> .5% and not last 2 versions`

`> .5% or not last 2 versions`

`> .5%, not last 2 versions`


### 常用配置


```
  "browserslist": {
    "production": [
      "defaults"
    ],
    "modern": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ],
    "ssr": [
      "node 12"
    ]
  }

```

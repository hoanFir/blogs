🐾 Chrome DevTools

🕘 2019.11.22 由 hoanfirst 编辑


1. [极客学院 - Chrome 开发工具指南](http://wiki.jikexueyuan.com/project/chrome-devtools/)

谷歌浏览器开发者工具是前端开发中常用的调试工具，如追踪布局问题，或设置JavaScript断点深入追踪代码。


2. 快捷键打开

Windows|Mac|功能|
-|-|-|
ctrl+shift+i|cmd+opt+i|打开"Network"面板|
ctrl+shift+j|cmd+opt+j|打开"Console"面板|
ctrl+shift+c|cmd+opt+c|打开"Element"面板|
ctrl+\[ ctrl+\]|cmd+\[或cmd+\]|切换面板|


3. 使用console在控制台面板输出调试信息

命令|功能|
-|-|
console.log()|输出普通信息|
console.info()|输出提示信息:information_source:|
console.error()|输出错误信息:x:|
console.warn()|输出警告信息:warning:|
console.table()|输出表格化后的数据信息|
console.time() console.timeEnd()|统计代码执行时间|
console.group() console.groupEnd()|分组输出多条信息|
console.count()|统计代码执行次数|

4. 提高性能的几个面板

面板|说明|
-|-|
'Network'面板|提供资源下载和加载的详细分析，用于提高网络性能|
'Audits'面板|提供加载页面过程的详细分析，用于减少页面加载时间，提高响应和用户体验|
'Performance'面板|允许为页面配置执行时间和内存使用量，有助于分析资源消耗问题，进一步优化代码|

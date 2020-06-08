
跨站点执行脚本，cross site scriping，又叫xss。

产生：

```

利用Internet某些Web站点的受信任级别高，来自非受信任环境的攻击者在这个受信任Web站点上注入数据，使其作为脚本执行。

跨站执行脚本可以用来访问数据，如用户的cookies，有了cookie就可以在任意能接进互联网的浏览器登陆目标网站，并以其他人的身份登录做破坏。

示例：

<input type="text" name="address" value="value">

value是来自用户的输入。

攻击者输入："/><script>alert(document.cookie)</script><!-

<input type="text" name="address" value=""/><script>alert(document.cookie)</script><!-">

后果：嵌入的JavaScript代码被执行

```

预防：

```

对输入内容进行转义。

```

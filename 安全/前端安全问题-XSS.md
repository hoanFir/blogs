
cross site scriping，跨站脚本攻击，又叫xss攻击。


## 产生：

XSS攻击通常指利用网页开发留下的漏洞，将恶意代码注入网页，使用户加载并执行攻击者的程序。攻击成功后，攻击者可能得到包括但不限于更高的权限、私密的网页内容、会话和cookie等各种内容。


栗子：盗用cookie

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

## 预防：

```

对输入内容进行转义。

```

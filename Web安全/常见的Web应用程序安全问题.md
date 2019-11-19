🐾 常见的Web应用程序安全问题

🕘 2019.10.19 由 hoanfirst 编辑

引起的原因：

1. 应用程序的某个组件的恶意数据在另一个组件中被当作了合法代码，这就造成了安全问题。

2. 对涉密信息的不当处理

### SQL注入

产生：

```

攻击者通过操纵程序的某种输入，在连接到SQL数据的应用程序上执行自己所构造的SQL语句。

比如，攻击者把SQL命令插入到Web表单的输入域或页面请求的query中，欺骗服务器执行恶意的SQL命令。因为在某些表单中，会直接把用户输入的内容用来构造动态SQL命令，或作为存储过程的输入参数。

示例：

Select * from sys_user where username='{username}';

攻击者输入：username='or'1=1

Select * from sys_user where username=''or'1=1';

后果：攻击者跳过了用户名的验证，实现了入侵。

```

预防：

```

过滤所有输入，确保输入字段只包含所需字符；尽量避免使用动态生成的SQL。

```


### 跨站点执行脚本

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


### 跨站点伪装请求

跨站点伪装请求，cross site request forgery，csrf。

产生：

```

攻击者在用户不知情的情况下在用户登录过的Web站点里发起攻击。

如邮箱的钓鱼邮件或bbs站点的钓鱼链接。由于用户当前已经登陆了某个邮箱站点或bbs站点，用户点击里钓鱼链接之后进入钓鱼网站，当他在浏览这个钓鱼网站时，可能因为某个图片或某个弹窗吸引你点击一下，此时可能就会触发一个攻击者构造的JavaScript点击事件。一般攻击者会构造一个bbs发帖的请求，去往你的bbs发帖，由于当前你的浏览器状态已经是登陆状态，就可以发帖成功。

```

预防：

```

在客户端发起的请求添加随机值等额外内容，让钓鱼网站无法正常伪造请求。


```

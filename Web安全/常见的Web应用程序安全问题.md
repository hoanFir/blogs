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

跨站点执行脚本又叫xss。

产生：

```

利用Internet某些Web站点的受信任级别高，来自非受信任环境的攻击者在这个受信任Web站点上注入数据，使其作为脚本执行。跨站执行脚本可以用来访问数据，如用户的cookies。

示例：

<input type="text" name="address" value="value">

value是来自用户的输入。

攻击者输入："/><script>alert(document.cookie)</script><!-

<input type="text" name="address" value=""/><script>alert(document.cookie)</script><!-">

后果：嵌入的JavaScript代码被执行

```

预防：

```

```

title: MailTo的用法
date: 2015-08-20 13:47:36
tags: [html, mailto]
---

`a`标签的`href`属性允许我们方便地调用本地默认邮件客户端发送邮件

## 如何使用mailto?

基本用法
```html
<a href="mailto:sample@163.com">send email</a>
```
或者
```
<form action="mailto:sample@163.com"></form>
```

`mailto`后边跟的是收信人，当然还有一些其它的参数可用，如下：

参数名称 | 意义
:---: | ---
to | 收信人
subject | 主题
cc | 抄送
bcc | 暗抄送
body | 内容

参数的传递方法同URL传值一样，针对上边2中用法分别是
### QueryString 方式
```html
<a href="mailto:sample@163.com?subject=test&cc=sample@hotmail.com&body=use mailto sample">send email</a>
```
### Form 方式
```html
<form name="sendmail" action="mailto:sample@163.com">
    <input name="cc" type="text" value="sample@hotmail.com">
    <input name="subject" type="text" value="test">
    <input name="body" type="text" value="use mailto sample">
</form>
```
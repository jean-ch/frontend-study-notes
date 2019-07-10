#### CSRF (Cross-Site Request Forgery)
跨站点请求伪造：攻击者通过跨站请求，以合法的用户的身份进行非法操作。   
可以这么理解CSRF攻击：攻击者盗用你的身份，以你的名义向第三方网站发送恶意请求。   
CRSF能做的事情包括利用你的身份发邮件，发短信，进行交易转账，甚至盗取账号信息

#### 防范
- SSL
- 验证码
- referer识别。在HTTP Header中有一个字段Referer，它记录了HTTP请求的来源地址。如果Referer是其他网站，就有可能是CSRF攻击，则拒绝该请求。但是，服务器并非都能取到Referer。很多用户出于隐私保护的考虑，限制了Referer的发送。在某些情况下，浏览器也不会发送Referer，例如HTTPS跳转到HTTP
- token机制。在HTTP请求中进行token验证，如果请求中没有token或者token内容不正确，则认为CSRF攻击而拒绝该请求
- 安全框架，例如Spring Security


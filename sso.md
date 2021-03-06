---
title: 如何实现单点登录(sso)
date: 2019-12-15 18:38:45
mp3: http://xiaoshiyi.top:9000/statics/1-04 Fairy.mp3
cover: http://xiaoshiyi.top:9000/statics/wapper1.jpeg
categories: 
    - 方案
---
### 简介
单点登录(Single Sign On)是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统的保护资源，若用户在某个应用系统中进行注销登录，
所有的应用系统都不能再直接访问保护资源，像一些知名的大型网站，如：淘宝与天猫、新浪微博与新浪博客等都用到了这个技术。
### 技术实现
在说单点登录的技术实现之前，我们先说一说普通的登录认证机制。
我们在浏览器中访问一个应用，这个应用需要登录，我们填写完用户名和密码后，完成登录认证。这时，我们在这个用户的session中标记登录状态为yes（已登录），
同时在浏览器（Browser）中写入cookie，这个cookie是这个用户的唯一标识。下次我们再访问这个应用的时候，请求中会带上这个cookie，服务端会根据这个
cookie找到对应的session，通过session来判断这个用户是否登录。如果不做特殊配置，这个cookie的名字叫做jsessionid，值在服务端是唯一的。

### 同域下的单点登录
一个企业一般情况下只有一个域名，通过二级域名区分不同的系统。比如我们有个域名叫做：a.com，同时有两个业务系统分别为：app1.a.com和app2.a.com。
我们要做单点登录，需要一个登录系统，叫做：sso.a.com。

我们只要在sso.a.com登录，app1.a.com和app2.a.com就也登录了。通过上面的登陆认证机制，我们可以知道，在sso.a.com中登录了，其实是在sso.a.com的
服务端的session中记录了登录状态，同时在浏览器端的sso.a.com下写入了cookie。那么我们怎么才能让app1.a.com和app2.a.com登录呢？
这里有两个问题：
- cookie是不能跨域的，我们cookie的domain属性是sso.a.com，在给app1.a.com和app2.a.com发送请求是带不上的。
- sso、app1和app2是不同的应用，它们的session存在自己的应用内，是不共享的。
![同域下的单点登录](http://xiaoshiyi.top:9000/statics/sso-cookie.jpg)

那么我们如何解决这两个问题呢？针对第一个问题，sso登录以后，可以将cookie的域设置为顶域，即.a.com，这样所有子域的系统都可以访问到顶域的cookie。
我们在设置cookie时，只能设置顶域和自己的域，不能设置其他的域。比如：我们不能在自己的系统中给baidu.com的域设置cookie。

cookie的问题解决了，我们再来看看session的问题。我们在sso系统登录了，这时再访问app1，cookie也带到了app1的服务端，
app1的服务端怎么找到这个cookie对应的session呢？这里就要把3个系统的session共享，如图所示。共享session的解决方案有很多，
例如：Spring-Session。这样第2个问题也解决了。

同域下的单点登录就实现了，但这还**不是真正的单点登录**。

### 不同域下的单点登录
这里我们就要说一说CAS流程了，这个流程是单点登录的标准流程。  
具体流程如下：  
1. 用户访问app系统，app系统是需要登录的，但用户现在没有登录。  
2. 跳转到CAS server，即SSO登录系统，以后图中的CAS Server我们统一叫做SSO系统。 SSO系统也没有登录，弹出用户登录页。  
3. 用户填写用户名、密码，SSO系统进行认证后，将登录状态写入SSO的session，浏览器（Browser）中写入SSO域下的cookie。  
4. SSO系统登录完成后会生成一个ST（Service Ticket），然后跳转到app系统，同时将ST作为参数传递给app系统。
5. app系统拿到ST后，从后台向SSO发送请求，验证ST是否有效。
6. 验证通过后，app系统将登录状态写入session并设置app域下的cookie。

至此，跨域单点登录就完成了。以后我们再访问app系统时，app就是登录的。接下来，我们再看看访问app2系统时的流程。

1. 用户访问app2系统，app2系统没有登录，跳转到SSO。
2. 由于SSO已经登录了，不需要重新登录认证。
3. SSO生成ST，浏览器跳转到app2系统，并将ST作为参数传递给app2。
4. app2拿到ST，后台访问SSO，验证ST是否有效。
5. 验证成功后，app2将登录状态写入session，并在app2域下写入cookie。
这样，app2系统不需要走登录流程，就已经是登录了。SSO，app和app2在不同的域，它们之间的session不共享也是没问题的。

### 总结
单点登录的所有流程都介绍完了，原理大家都清楚了。总结一下单点登录要做的事情：

- 单点登录（SSO系统）是保障各业务系统的用户资源的安全。
- 各个业务系统获得的信息是，这个用户能不能访问我的资源。
- 单点登录，资源都在各个业务系统这边，不在SSO那一方。用户在给SSO服务器提供了用户名密码后，作为业务系统并不知道这件事。 
- SSO随便给业务系统一个ST，那么业务系统是不能确定这个ST是用户伪造的，还是真的有效，所以要拿着这个ST去SSO服务器再问一下，这个用户给我的ST是否有效，
是有效的我们才能让这个用户访问。

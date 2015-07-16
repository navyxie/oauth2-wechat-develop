# 以接入微信开放平台为例讲解OAuth2原理

## 什么是OAuth？

OAuth是一个开放授权标准。允许用户让第三方应用访问该用户在某应用（服务）的资源。

OAuth有1.0和2.0版本，今天我们主要讲Oauth2.0,下面看一下Oauth2.0的运行流程

![OAuth2.0 运行原理图](https://mmbiz.qlogo.cn/mmbiz/E7ia3F4UicMxibpaLWjHJaOJNXq3Yuxv5WjuR9yCpvTHLlTUM4icwxHMv6elzXvtNicSyaSOic95sJbEb3K1cRpuG79Q/0?wx_fmt=png)

> （A）用户打开客户端以后，客户端要求用户给予授权。

> （B）用户同意给予客户端授权。

> （C）客户端使用上一步获得的授权，向认证服务器申请令牌。

> （D）认证服务器对客户端进行认证以后，确认无误，同意发放令牌。

> （E）客户端使用令牌，向资源服务器申请获取资源。

> （F）资源服务器确认令牌无误，同意向客户端开放资源。


**从上面可以看出，Oauth2.0的运行原理就是：第三方应用请求某个服务应用的用户给予授权，用户统一授权后，第三方应用就可以和服务应用进行后续的获取资源的令牌申请。整个流程的关键点就是用户是否同意授权。**

Oauth2.0授权模式总共有四种：

1. 授权码模式（authorization code）
  + 这是功能最完整、流程最严密的授权模式。它的特点就是通过客户端的后台服务器，与"服务提供商"的认证服务器进行互动。
2. 简化模式（implicit）
  + 不通过第三方应用程序的服务器，直接在浏览器中向认证服务器申请令牌。所有步骤在浏览器中完成，令牌对访问者是可见的，且客户端不需要认证。
3. 密码模式（resource owner password credentials）
  + 用户向客户端提供自己的用户名和密码。客户端使用这些信息，向"服务商提供商"索要授权。该模式存在安全隐患，一般不使用该模式。
4. 客户端模式（client credentials）
  + 指客户端以自己的名义，而不是以用户的名义，向"服务提供商"进行认证。该模式完全不经过用户授权这一块，是第三方应用与提供服务应用之间的一种信赖授权。
      

## 微信开放平台开发者接入使用了上面四种模式中的第一种，下面我们以微信授权过程来讲解Outh2.0的运营原理（授权码模式-authorization code）

授权码模式（authorization code）

![Oauth2.0 authorization code 授权码模式](https://mmbiz.qlogo.cn/mmbiz/E7ia3F4UicMxibpaLWjHJaOJNXq3Yuxv5WjFFuluSicz8FaTsH7ZDxKlHejsEHxibjddvBIxGrnG2ic8H51iaChPXCDYw/0?wx_fmt=png)


以获取微信网页授权获取用户基本信息为例解析上面授权模式[附微信文档](http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html)

1.  第一步：第三方应用引导用户到微信授权页面
  + 对应步骤(A)
2.  第二步：用户同意授权，获取code
  + 对应步骤(B)
  + 授权链接(引导用户点击授权的页面)![用户同意授权，获取code](https://mmbiz.qlogo.cn/mmbiz/E7ia3F4UicMxibpaLWjHJaOJNXq3Yuxv5WjokM9vth5S7okNm8hKk9g5ZppzYgvzLNfNZmjlYDWpyEI8wAs4LKtuw/0?wx_fmt=png)![返回参数](https://mmbiz.qlogo.cn/mmbiz/E7ia3F4UicMxibpaLWjHJaOJNXq3Yuxv5WjxAjYlTlVWQ7rW6txbNDaoVzm13jrqGwScv4tBaI0Phat63zKkm3Nnw/0?wx_fmt=png)
  + 对应步骤(C)
  + 返回参数,具体参数说明看截图 ![返回参数](https://mmbiz.qlogo.cn/mmbiz/E7ia3F4UicMxibpaLWjHJaOJNXq3Yuxv5WjTtnJcVB35fPrf8ZTKLuqibuqpMfTHsmqA3ZnOp980YeMhgib6Du82rww/0?wx_fmt=png)
3. 第三步：第三方应用通过code换取网页授权access_token
  + 对应步骤（D）
  + 获取access_token的请求参数![请求参数](https://mmbiz.qlogo.cn/mmbiz/E7ia3F4UicMxibpaLWjHJaOJNXq3Yuxv5WjyWWeKRkoSUM2slUnIn9DZtFEG4nQ18zic64TYVJ7J7PCb6Q4VfntFCA/0?wx_fmt=png)
  + 对应步骤（E）
  + 请求成功的响应参数![响应参数](https://mmbiz.qlogo.cn/mmbiz/E7ia3F4UicMxibpaLWjHJaOJNXq3Yuxv5Wjyn53Y80PtRibatNrDxbjsF6Oe3YmHb1A21ZxlBhOASFgWyyDsZbZsaw/0?wx_fmt=png)
4. 第四步：通过access_token拉取用户信息(需scope为 snsapi_userinfo)
  +获取用户信息的请求参数![响应参数](https://mmbiz.qlogo.cn/mmbiz/E7ia3F4UicMxibpaLWjHJaOJNXq3Yuxv5Wj7uaYuwDSUOefia2rNbePPHIicrSyrPnj9w7zjGyuEKZLYeOS84GMmxlw/0?wx_fmt=png)
  +成功响应的参数，格式为json![响应参数](https://mmbiz.qlogo.cn/mmbiz/E7ia3F4UicMxibpaLWjHJaOJNXq3Yuxv5Wj5dRQjL6FE3MfzibUBALCq6CLoVLmLicLDKzuVlhabqY3ibRakyASzDKog/0?wx_fmt=png)

以微信授权获取用户信息为例子讲解Oauth2.0 授权码模式到此结束，接下来会有一篇文章是讲解如何接入微信开放者平台。

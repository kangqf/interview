### OAuth 2.0

#### OAuth2.0 简介
也就是我们第三方登录方式的一种，步骤主要分为以下几步


#### OAuth2.0 示例

下面将以自己的网站获取微博的资源来介绍 OAuth2.0 

1. 引导用户前往微博的验证授权页面，一般这种引导会通过弹出一个新的窗口来完成

``` 
GET 

https://api.weibo.com/oauth2/authorize?client_id=YOUR_CLIENT_ID&response_type=code&redirect_uri=YOUR_REGISTERED_REDIRECT_URI
https://api.weibo.com/oauth2/authorize?client_id=41xxxx36&response_type=code&redirect_uri=http://kangqingfei.cn
```

其中`YOUR_CLIENT_ID`是在 http://open.weibo.com/webmaster/info/basic?siteid=41xxxxx36 中的`App Key`，而后面的`YOUR_CLIENT_SECRET`就是该页面上的`App Secret`。

这一步的`CLIENT_ID`是与`redirect_uri`对应的，也就是说不能拿别人的ID来redirect自己的uri，但是在已知别人的ID和`redirect_uri`的情况下还是可以伪造的，毕竟这些内容都会反映在引导用户去授权的那个网址上面。

这也是为什么不直接一步把授权码给你的原因了，怕的就是被别人伪造你的网站的请求，然后拿你的授权码，去获取微博的资源。


2. 在上一步中，如果用户确认授权，微博的认证服务器就会给用户浏览器发送一个3xx的重定向的请求，而重定向的地址就是一开始用户填的`redirect_uri`，并同时会在后面加上 `authorized_code`。 这里自己的网站收到这个code后应该关闭刚刚弹出的窗口，然后进入下一步。

```
3xx
http://kangqingfei.cn/?code=72033xxxx645633ea7
```

3. 自己网站拿到`authorized_code`后便会通过该code去请求有实际意义的`access_token`

```
POST 

https://api.weibo.com/oauth2/access_token?client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&grant_type=authorization_code&redirect_uri=YOUR_REGISTERED_REDIRECT_URI&code=CODE

https://api.weibo.com/oauth2/access_token?client_id=416xxxx36&client_secret=8ca6be3axxxx721e543&grant_type=authorization_code&redirect_uri=http://kangqingfei.cn&code=72xxxx33ea7
```
这里使用的POST方式，不容易被日志记录，而且使用https不容易被监听，然后还附加了`client_secret`用来保证确实是自己的网站，该`client_secret`可以被开发者重置


4. 微博认证通过后会以json的形式返回`access_token`，开发者应该把这个token保存好，并且该token是有 保质期 的一旦过期开发者应该重新认证，还有方法是每次用户通过OAuth2.0 认证通过之后开发者就更新其token，直到token过期

返回的json: 

```
{
    "access_token": "2.008_S6WD7vIaXxxxxGWa3ZC",
    "remind_in": "15xxxx99",
    "expires_in": 15xxxx99,
    "uid": "322xxxx45",
    "isRealName": "true"
}
```

5. 拿到`access_token`后我们就可以对用户的信息__为所欲为__了，我们可以拿着这个密钥去找微博的资源中心要该用户的信息，具体能要到的东西根据你的权限不一样而有差别，微博的可使用的接口在这里查询`http://open.weibo.com/webmaster/privilege?siteid=416xxx36`

例如我们用刚刚获取的token来请求用户的最新微博就可以这个样子：

```
GET
https://api.weibo.com/2/statuses/user_timeline.json?access_token=2.008xxxxxxxWa3ZC
```

然后微博就会以json 的形式返回用户的最新的微博了。
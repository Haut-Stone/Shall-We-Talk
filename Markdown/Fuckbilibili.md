## 爬取会员的世界

之前我写的python爬虫没有办法爬取会员的世界，这是因为会员的世界是需要用户登录才可以访问的，一般的游客是看不见的。这次的代码更新，解决了用户登录的问题，这里就来讲一下我们是如何解决这个问题的。

## 告诉服务器自己已登陆

其实解决爬取`会员的的世界`的问题的方法很简单，只需要让bilibili的服务器知道你已经登陆了就可以了。关键就在于怎么让他知道你登陆了。我刚开始想了这么几个方案：

- 向登陆页面提交登陆表单
- 引用第三方的API
- 手动登陆，然后直接在header里面加入cookies

`向登陆页面提交登陆表单`这个方法其实看起来是很可行的，实际上对于大部分的网站也都是标准操作，可是bilibili比较无奈的一点就是，他的登陆部分是一块拼图，如果是验证码的话，直接交给第三方库解决就好了，拼图的话不靠人的话真的是没有办法去解决了（目前来说，估计以后我会知道方法的），所以就放弃了这个方法。


`引用第三方的API`这个方法主要是依靠这个网站[传送门](https://api.kaaass.net/biliapi/docs/?file=02-视频相关/001-解析B站视频)这里集成了一些bilibili的API，其实就是这个网站的主人有bilibili的appkey，所以可以很方便的做一些事情，我也不懂他的appkey是怎么搞到手的。反正有appkey的事情就简单太多了，相当于bilibili数据的大门就向你敞开着，毕竟bilibili本身也会通过appkey获取数据。其实这个网站有的功能还是蛮好用的，可惜的是我把我自己的access_key输进去测试的时候，这个网站就出问题了，一直是400错误。换用github上的API也都有些陈旧了。所以没办法，这个方法也只好作罢。

`手动登陆，然后直接在header里面加入cookies`这个方法就是最后我实现的方法了。这个方法最大的特点就是，先手动登陆。不用再在程序里进行登陆了。大家都知道浏览器使用cookies保存登陆信息的。于是我就认真的分析了登陆前和登陆后，浏览器发给bilibili的请求头中cookies的不同之处。在经过了比对之后，锁定了这三个参数。

>'DedeUserID': 'XXX',
'DedeUserID__ckMd5': 'XXX',
'SESSDATA': 'XXX'

看名字就可以知道是用户的ID, 用户ID的加密，还有会话的编号。据我观察一次登陆后，这个`SESSDATA`是不会改变的。然后咱们只要把这三个属性打包到cookies里让requests库带着他访问bilibili。bilibili就会认为我们已经登陆，并且返回我们想要的信息给我们了。

## 那为什么浪费了一个下午

因为一个很愚蠢的错误，刚开始我把这些数据直接写在header里了，而且还把'cookie'写成了'Cookie',然后就理所应当的一直访问失败。然后我就开始怀疑我找错重点了，就有开始检查别的文件，然后，一下午就这个么过去了……不过这其中也不是没有收获，在这个过程中，我发现了bilibili大量使用js来加载数据，并且发现了许多获取数据的接口。这对以后的开发肯定是有利无害的。这么一想，一个下午也还值得。

## 代码参考
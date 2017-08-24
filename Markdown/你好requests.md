## 爬虫可是很实用的啊

这年头，不学点爬虫你都不好意思说自己学过python，可以说学python爬虫改变了我之间对网络的认识。requests库是一个简单，优秀的库。里面方便的方法让编写一个爬虫变的如此的简单。好好掌握这个库就可以做到一些很有意思的内容呢2333

## requests库的基本方法

### requests库的7种方法
requests库有以下这7种方法，这7种方法的后六种都是以第一种为基础的，鉴于网站的安全问题，有的方法不是很常用。所以先知道作用就行了，等到用到了再仔细研究也可以。其中get()和head()两种方法比较常用，`get()`方法用来获取网页响应时返回的文件，可以是html也可以是别的文件格式，`head()`仅仅返回一个响应网页的文件头，并不返回网页本身。

<div align="center">
  <img src="https://raw.githubusercontent.com/Haut-Stone/Shall-We-Talk/master/Photos/requests库的7个主要方法.png">
</div>

### 最常用的get()
下面就以最常用的get()为例子，讲解一下这7种方法所共有的性质，其他六种方法和get()除了在参数上有些不同外，其实都还是差不多的。。

`requests.get()`方法返回的是一个`response对象`,我们可以建立一个实例保存这个对象，例如下边这个样子。

```python
response_object = requests.get(url)
```

response对象里不仅包含了服务器返回的所有信息，也包含了你刚才发送给服务器的requests请求。这对于有时候需要查看请求内容的情况下是十分的方便的。get一共有以下3个不同的属性：

- url
- params 
- **kwargs (12个可选的参数 例如headers)

这里的参数有很多项。下面会一一讲到，至于params是什么，这个东西其实就是你在调用网站的API时填入的东西，比如你在bilibili搜一个up主名字时，url上的`keyword`就是这个参数，当然你手工构造一个这样的url也是可以的，只不过用这样的参数形式更容易让人理解你想要干什么罢了。

<div align="center">
  <img src="对象属性">
</div>

上面的这几个属性都是平时比较常用的属性，多打几次记住名字自然就熟悉了，这里指出一些属性的作用，`r.content` 属性因为是文件的二进制形式，一般像保存文件的时候要用到这个属性。`encoding`是当网站的html文件里写有<charset>标签时，python得到的编码格式，表示程序员在写网站时就规定了网站的编码格式，而如果程序员没有规定编码格式的话，默认的值是ISO‐8859‐1。`apparent_encoding`是python根据网页内容分析出来的可能的编码格式，一般如果网站的内容无法正常显示的时候，可以用这里面推荐的编码格式，代替网站本身的编码格式。

网页连接的错误情况很容易出现，在代码层面写出可以预防问题发生的健壮的代码是一个良好的习惯。这里列出了几种可能出现的异常种类

<div align="center">
  <img src="http异常">
</div>

为了预防以上异常的出现导致程序崩溃，我们要有一个通用的错误处理框架。在程序出现异常时处理异常，并抛出一个合理的提示信息。基本格式就像下面这个样子。

```python
import requests

def get_http_text(url):
	try:
		r = requests.get(url, timeout = 3)
		r.raise_for_status() # 返回不是200的话，制造一个异常
		r.encoding = r.apparent_encoding
		return r.text[:1000] # 一般网页返回的html都很长，所以切片其中的一部分返回
	except:
		return 'Something wrong!'

```

>本篇是学习中国大学mooc上的公开课后写下的总结
[toc]
# 简介
Requests是一个python HTTP库。  
示例：
``` python 
>>> r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
>>> import requests
>>> r = requests.get("https://bj.lianjia.com/xiaoqu/")
>>> r.status_code
200
>>> r.headers
{'Server': 'Lianjia', 'Date': 'Tue, 05 Jun 2018 08:26:53 GMT', 'Content-Type': 'text/html; charset=UTF-8', 'Transfer-Encoding': 'chunked', 'Connection': 'close', 'Vary': 'Accept-Encoding', 'Set-Cookie': 'select_city=110000; expires=Wed, 06-Jun-2018 08:26:53 GMT; Max-Age=86400; path=/; domain=.lianjia.com, all-lj=ba32fa4540e52c45d4c94b9a16e82078; Path=/, lianjia_ssid=8c7e3a95-d2e1-4b59-a244-ec465254e69e; expires=Tue, 05-Jun-18 08:56:53 GMT; Max-Age=1800; domain=.lianjia.com; path=/, lianjia_uuid=27e5995d-ae1a-42af-96af-ca638229704c; expires=Fri, 02-Jun-28 08:26:53 GMT; Max-Age=315360000; domain=.lianjia.com; path=/', 'via': 'web05-online.zeus.ljnode.com', 'Content-Encoding': 'gzip'}
```
Requests具有以下功能：  
- Keep-Alive和连接池
- 国际化域名和URL
- 带持久Cookie的会话
- 浏览器式SSL认证
- 自动内容解码
- BASIC/DIGEST认证
- 优雅的基于key/value的Cookie
- 自动解压
- Unicode响应体
- HTTP(S)代理支持
- 文件分块上传
- 流下载
- 连接超时
- 分块请求
- 支持.netrc

# 快速上手

## 发送请求  
> r = requests.get("http://www.baidu.com")
> r是Response对象，我们可以从中得到想要的响应信息  

解释:requests.get即通过GET方法来获取响应。实际上是对requests.request(mothod,url,**kwargs)函数的一个封装。  
上面等价于:  
> r = requests.request(mothod="GET","http://www.baidu.com")  

实际上，requests模块中分别封装了:get head post patch put delete和options。最常用的是get和post。  

## 传递URL参数
对于get请求，参数是以?key=value的形式置于URL中的。  
例如一个URL = "www.example.com/get?key1=value1&key2=value2"这样的网址。  
> r = requests.get("http://www.example.com/get?key1=value1&key2=value2")
> r = requests.get("http://www.example.com/get",params={'key1':'value1','key2':'value2'})
> 以上两种方式都是可行的，第二种方式将参数以字典的形式赋值给params参数，requests会自动为我们生成需要的URL，如果键值对中包含非英文字符比如汉字，requests会自动为其转码。  

对于post请求，参数是以表单的形式发送给服务器的。其在URL中并不显示。  
```python
message = {'key1':'value1','key2':'value2'}
r = requests.post('http://www.baidu.com/',data=message)
#只需要简单地传递一个字典对象给data参数即可。在发送请求时会自动编码为表单形式
payload = (('key1', 'value1'), ('key1', 'value2'))
r = requests.post('http://httpbin.org/post', data=payload)
#也可以为data参数传入一个元组列表，在表单中多个元素使用同一个key的时候，适用于这种方式传递
#如果你想要的数据并非编码为表单形式的，如果你传递一个string而不是一个dict,那么数据会被直接发布出去，而不进行转码
```

## 响应内容
Requests会自动解码来自服务器的内容，大多数unicode字符集都能被无缝地解码。  
请求发出后，Requests会基于HTTP头部对响应的编码做出有根据的推测。当你访问r.text之后，requests会使用其推测的文本编码。 
``` python 
r = requests.get('http://api.github.com/events')
#r是一个Response对象
r.text
#获得网页的内容，是一个unicode字符串
r.encoding
#得到Requests基于HTTP头部推测的网页文本编码
r.encoding = "ISO-8859-1"
#你也可以直接修改这个值来改变requests的推测
r.content
#以二进制即字节的方式访问响应内容。对于非文本(图片、视频等)尤其有用。
r.json()
#Requests中内置了JSON解码器，有助于你处理JSON数据。
```

## 定制请求头
如果你想为请求头添加HTTP头部，只要简单的传递一个dict给headers参数就可以了，注意：定制header的优先级低于某些特定的信息源：  
- 如果在.netrc中设置了用户认证信息，使用headers=设置的授权就不会生效。二如果设置了auth=参数，.netrc的设置就无效了
- 如果被重定向到别的主机，授权header就会被删除
- 代理授权header会被URL中提供的代理身份覆盖掉
- 在我们能判断内容长度的情况下，header的Content-Length会被改写  

``` python 
url = "http://www.baidu.com"
header = {'user-agent':'my-app/0.0.1'}
r = requests.get(url,headers=header)
```

# 高级用法

## 会话对象 
理解会话中的Cookie和Session对象：  
会话可以简单的理解为：用户打开一个浏览器，点击多个超链接，访问服务器多个web资源，然后关闭浏览器，整个过程称之为一个会话。  
Cookie技术:Cookie是客户端技术，服务器把每个用户的数据以cookie的形式写给用户各自的浏览器，并保存在客户端浏览器的缓冲中。当用户使用浏览器再访问服务器中的Web资源时，就会带着各自的数据去。这样，web资源处理的就是用户各自的数据了。  
Session技术:Session是服务器技术，利用这个技术，服务器在运行时可以为每一个用户的浏览器创建一个其独享的session对象，由于session为用户浏览器独享，所以用户在访问服务器的web资源时，可以把各自的数据放在各自的session中，当用户再去访问服务器中的其他web资源时，其他web资源再从用户各自的session中取出数据为用户服务。  

会话对象让你能够跨请求保持某些参数，它也会在同一个Session实例发出的所有请求之间保持cookie。期间使用了urllib3的连接功能，所以如果你先同一个主机发送多个请求，底层的TCP连接将会被重用，从而带来显著的性能提升。  
会话对象具有主要的Requests API的所有方法。可以通过会话对象来替代requests方法来操作。
``` python 
s = requests.Session()
s.get('http://www.baidu.com')  
s.auth = ('user','pass')
s.headers.update({'x-test':'true'})
s.get('http://www.baidu.com',headers={'x-test2':'true'})
#x-test和x-test2都会被发送

# 任何你传递给请求方法的字典都会与已设置会话层数据合并，方法层的参数覆盖会话的参数。
# 不过需要注意，就算使用了会话，方法级别的参数也不会被跨请求支持。
s = requests.Session()
r = s.get('http://httpbin.org/cookies',cookies={'from-my':'browser'})
print(r.text)   #{"cookies":{"from-my":"browser"}}
r = s.get("http://httpbin.org/cookies")
print(r.text)   #{"cookies":{}}
# 默认情况下，s.get会自动将得到的cookies传递给Session对象，如果你要手动添加cookie，可以使用Cookie utility函数来操纵Session.cookies
```
所以我们实际在实际使用中使用Session来管理请求，而不是通过requests.get方法来管理。
1. 编写header
2. 创建Session对象
3. Session.get()来返回Response
4. 处理返回的Response,还可以通过Session对象来继续请求页面

requests.Session().get()与requests.get()的区别：
如果我们要制定header和cookies，对于requests.get()我们需要在每次调用该方法的时候都指定requests.get(headers,cookies)参数。而对于requests.Session().get()在方法返回的时候，自动将得到的headers和cookies都赋值给Session对象中的headers和cookies参数，这样只要我们持有这个Session对象就可以直接调用get方法，而不用每次都为get方法制定headers和cookies参数，即使你要自定义headers和cookies参数也可以直接在Session对象上更改即可。

### 实例
通过chrome浏览器得到header和cookies来保持一个Session进而爬取知乎(www.zhihu.com)

``` python 
import requests
s = requests.Session()      #首先构建一个Session对象
s.get("http://www.zhihu.com")
#<Response [400]> 
#可以看到返回的结果是400“Bad Requests”即错误请求
#这是因为我们的请求头有问题，触发了知乎的反爬虫机制
s.headers
#{'User-Agent': 'python-requests/2.18.4', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'}
#其中的User-Agent表示用户客户端 requests默认为python-requests我们要更改他
#打开Chrome浏览器，打开开发者工具，随便打开一个网址，然后找到Network中的Headers下的Request Headers中的user-agent参数
header = {
    "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36",
}
s.headers.update(header)
#通过update来向Session对象的headers中添加参数
s.headers
#{'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'}
m = s.get("http://www.zhihu.com")
# <Response [200]>
m.text
#...<div class="SignFlowHeader-slogen">注册<!-- -->知乎，发现更大的世界</div></div><div class="SignContainer-inner">...
#得到的是一个注册界面，提示我们要注册
#此时有两种处理方法：
#1.通过Post来将需要的用户名密码以及验证码等数据发送给知乎服务器，显然这种方法需要更多的知识来支持(需要抓包、分析抓包结果等等)
#2.通过在Chrome中登陆得到获得的Cookies，直接将该Cookies绑定到Session对象中实现
#使用第二种方式来实现：
#在Chrome中登陆知乎，在开发者工具的Request Headers中找到cookie参数，复制。
cookie = {
    "cookie":'_zap=6941c590-1d69-48d5-80ae-cb91ced83ccd; q_c1=1a96f1dadb464943ad2ac016f5c6237a|1526200133000|1526200133000; d_c0="ACAhxTZcmA2PTgJ3myRHiZ_5Tf9eMBZiCmA=|1526343020"; __utmv=51854390.100-1|2=registration_date=20140711=1^3=entry_date=20140711=1; _xsrf=a1e368de-0adf-4f3e-913c-962664bd181e; __utmc=51854390; __utmz=51854390.1528240771.4.4.utmcsr=zhihu.com|utmccn=(referral)|utmcmd=referral|utmcct=/question/29372574/answer/346927368; __utma=51854390.407799188.1527575103.1528240771.1528240777.5; capsion_ticket="2|1:0|10:1528248082|14:capsion_ticket|44:Y2RlYzFmYTU1MjAyNDNlODhkYmNjN2I2ZmJjOWJjMDI=|2196210f0e3cf77e6a62dc730a543bef20faa6c36214205cfcfd5578c4843f52"; z_c0="2|1:0|10:1528248090|4:z_c0|92:Mi4xaHU5cEFBQUFBQUFBSUNIRk5seVlEU1lBQUFCZ0FsVk5Hb1VFWEFDenF6dVhvakMzVzBPWmpSMWUtY2NzWF9Cdkx3|bee2742195e1975e16ce979685ea3b92ae9ad4a25bf0ee9d2b42025bfa164d7e"; tgw_l7_route=bc9380c810e0cf40598c1a7b1459f027'
}
#注意，cookie的值中有使用到双引号，所以需要使用单引号来包括
s.headers.update(cookie)
s.get("https://www.zhihu.com")
#OK 这样我们就可以通过Session对象来不断获取知乎上的数据了。
```
简单总结下： 
1. 创建一个Session对象，reqeusts.Session()
2. 构造一个headers，通常需要User-Agent。
3. 将你构造的headers添加到Session对象的headers中去通过Session.headers.update(myHeaders)
4. 如果一个网站需要登陆，通过在Chrome登陆后查看cookies，并构造该cookies将其添加到Session对象中去来实现
5. 通过这个Session对象的get post等方法来获取网页

## 请求与响应对象
任何时候你进行类似requests.get()这样的调用，都是在做两件事情：  
1. 构建一个Request对象(可能还需要添加headers cookies等属性)  
2. 一旦requests得到一个从服务器返回的响应就会产生一个Response对象，该响应对象包含服务器返回的所有信息，也包含你原来创建的Request对象
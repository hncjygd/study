# 介绍
当我们从网站上抓取一个网页的时候，返回的通常是一个字符串，这个字符串包含了网页上所有的信息。类似:
``` html
"<html lang="zh" data-hairline="true" data-theme="light"><head><meta charSet="utf-8"/><title data-react-helmet="true">(4 封私信 / 38 条消息)首页 - 知乎</title>..."
```
而解析HTML就是将这一段字符中有用的信息提取出来。  
这里我们需要使用到的一个库是bs4.  

# bs4库介绍 

## 安装 
> pip install bs4

bs4依赖HTML解析器来完成工作，其支持3中解析器:  
|解析器|使用方法|优势|劣势|
|:------|:------|:------|:------|
|python标准库|Beautiful(markup,'html.parser')|python的内置标准库/执行速度适中/文档容错能力强|python3.2前的版本文档容错能力差|
|lxml解析器|Beautiful(markup,'lxml')|执行速度快/文档容错能力强|需要安装C语言库|
|html5lib|Beautiful(markup,'html5lib')|最好的容错性/以浏览器的方式解析文档/python编写不依赖外部扩展|速度慢|  

推荐使用lxml作为解析器。  

## 如何使用  
将一段文档传入BeautifulSoup的构造方法，就得到了一个文档的对象。传入的参数可以是字符串或一个文档句柄。  
``` python
from bs4 import BeautifulSoup
soup1 = BeautifulSoup(open("index.html"))
soup2 = BeautifulSoup("<html>data<html>")
```  
其内部首先将文档转换成Unicode，然后，BeautifulSoup选择最合适的解析器解析这段文档。也可以手动指定解析器。  

## 对象的种类  
BeautifulSoup将复杂的HTML文档转换成一个复杂的树形结构，每个节点都是pyhton对象，所有对象可以归纳为4中：  
- Tag   
- NavigableString
- BeeautifulSoup
- Comment

### Tag对象
Tag对象相当于HTML的标签。表示一个具有属性和内容的HTML标签。  
tag的属性：name - 表明tag的名字  attributes - 标签具有的属性  
> <p class="title"><b>The Dormouse's story</b></p>  

对于上面的一段文档，其中包含p b两个tag，其中p标签中有一个class属性。

### NavigableString对象
该对象表示包装在tag中的字符串。  
> <p class="title"><b>The Dormouse's story</b></p>  

对于p标签其中没有包含字符串，而对于b标签其中包含了"The Dormouse's story"

### BeautifulSoup对象
该对象表示一个文档的全部内容，大部分时候，可以把它当作tag对象(实际上BeautifulSoup继承自Tag，而Tag继承自PageElement)。  

### 注释及特殊字符串  
Tag，NavigableString BeautifulSoup基本上覆盖了html的所有内容，但是还有一些特殊对象，最重要的就是注释部分。而该部分使用Comment对象来表示。Comment对象是一个特殊类型的NavigableString对象。  

### 这四个对象之间的关系  
他们之间具有继承的关系：
- Tag和NavigableString都继承自PageElement类  
- BeautifulSoup继承自Tag类  
- Comment继承自NavigableString类  

可以看到他们都有共同的祖先：PageElement类：该类包含了页面的导航信息，包括一个标签或者一段文字。  
通常我们创建一个BeautifulSoup对象，然后通过他得到Tag对象(通常使用find_all()方法),然后通过Tag对象的string属性来获取NavigableString对象。

### 实例
``` html  
<!-- index.html -->
<html>
    <head><title>The Dormouse's story</title></head>
    <body>
        <p class="title">
            <b>The Dormouse's story</b>    
        </p>
        <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>
            <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a>
             and
            <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p> 
        <p class="story">爱丽丝</p>
    <body>
</html>
```
> 其中每个<b>The Dormouse's story</b>都是一个标签即tag对象。
``` python
from bs4 import BeautifulSoup
soup = BeautifulSoup(open("index.html"),'lxml')
tag = soup.b #得到第一个b标签
#<b>The Dormouse's story</b>
type(tag)
#bs4.element.Tag  可见他是一个Tag对象
tag.name
#b
tag.attrs   #attrs以字典的形式列出所有该标签具有的属性
#{} 由于b标签没有属性所以为空
soup.p.attrs
#{'class': ['title']}   class是一个 多值属性，其值是一个列表  
soup.p['class']     #可以以字典的形式来获得属性         
# ['title']     
tag.string      #通过string属性来获取标签中包含的字符串
#  "The Dormouse's story"
type(tag.string)
# bs4.element.NavigableString
```

## 遍历文档树  
前面介绍了，bs4中包含4中节点，其中BeautifulSoup指代整个HTML文档，Tag对应各个标签，而NavigableString则对应由标签包括的字符串，最后Comment表示注释。
### 子节点  
子节点一般是针对于Tag来说的，一个Tag可能包含多个其他节点(其他Tag和NavigableString).  
而NavigableString节点并没有子节点，所以以下属性仅适用于Tag节点(当然也适用于BeautifulSoup因为它继承自Tag)  

### 通过tag的名字取得Tag对象
通过.操作符加上标签名可以直接得到一个tag对象。soup.b soup.p等等甚至可以soup.body.b来得到嵌套在其中的tag对象。  
注意这种取得对象的方式只能获得当前名字的对一个Tag对象，如果想要获得所有的该Tag对象，比如获得所有a标签则需要使用find_all()方法来获取。  

### .contents和.children  
Tag的.contents属性会将tag的直接子节点以列表的方式输出：  
> soup.head.contents  #[<title>The Dormouse's story</title>]   

其.children一样，也是列出tag的直接子节点，但是返回的是一个迭代器。  
> soup.head.children  #<list_iterator at 0x5c690d0>  

### .descendants属性
上面的.contents和.children属性仅仅包含tag的直接子节点，而子节点自己还有子节点。而.descendants属性可以对所有tag的子孙节点进行递归循环。  
``` python
for child in soup.head.descendants:
    print(child)
# <title>The Dormouse's story</title>
# The Dormouse's story
```  

### .string属性
如果tag只有一个NavigableString类型子节点，那么这个tag可以使用string来得到这个子节点。  
``` python
soup.head.string     #虽然他还有一个title Tag节点，但是只有一个BavigableString子节点。
#'The Dormouse's story'  
soup.title.string      #同样
#'The Dormouse's story' 
soup.title.contents    #得到的结果一样，因为title下只有一个子节点，就是这个NavigableString对象
#'The Dormouse's story' 
soup.html.string    #如果一个tag下包含多个NavigableString类型的子节点，.string方法无法确定应该输出哪个，就直接输出None
#None
```

### .strings和.stripped_strings属性
对于Tag下有多个NavigableString类型的子节点的情况，可以使用.strings属性来获取。该属性是一个生成器，需要使用list()转换或者for语句来调用。  
``` python
for string in soup.strings:
    print(string)
    # u"The Dormouse's story"
    # u'\n\n'
    # u"The Dormouse's story"
    # u'\n\n'
    # u'Once upon a time there were three little sisters; and their names were\n'
    # u'Elsie'
    # u',\n'
    # u'Lacie'
    # u' and\n'
    # u'Tillie'
    # u';\nand they lived at the bottom of a well.'
    # u'\n\n'
    # u'...'
    # u'\n'
```
可以看到输出的字符串中包含了很多空格或空行，使用.stripped_strings可以去除多余空白内容：  
``` python
for string in soup.stripped_strings:
    print(string)
    # u"The Dormouse's story"
    # u"The Dormouse's story"
    # u'Once upon a time there were three little sisters; and their names were'
    # u'Elsie'
    # u','
    # u'Lacie'
    # u'and'
    # u'Tillie'
    # u';\nand they lived at the bottom of a well.'
    # u'...'
```
其会除去全部的空格的行，以及段首和段尾的空白会被删除。  

### .parent和.parents属性表示父节点  
通过.parent属性可以获得某个元素的父节点，例如title标签的父节点为head。  
通过.parent得到的父节点是直接父节点，而通过.parents属性是通过递归得到元素的所有父类节点。  

### .next_sibling/.next_siblings和.previous_sibling/.previous_siblings属性来表示兄弟节点  
个人感觉没啥用，只要知道有这个属性就好了，真用到了再查文档。  

## 搜索文档树  
bs4中虽然定义了很多搜索方法， 但是最有用的有2个find()和find_all().其参数基本上相同，只是find()返回最先匹配的一个节点。而find_all()以列表形式返回匹配的所有节点。所以说find_all()的适用性更强一些。也是我们最常用的方法。所以我们只介绍find_all()方法的使用，其他的搜索方法基本上与之类似。  

### find_all()方法  
>find_all(name,attrs,recurisive,text,**kwargs)  
1.name参数：可以查找所有名字为name的tag，字符串对象会被自动忽略掉。其可以是一个字符串、正则表达式、列表、方法或者True(统称为过滤器)。  
    -字符串：最简单的过滤器
    soup.find_all("b")  #搜索文档中所有的b标签。  
    -正则表达式：如果传入正则表达式，会按照match()来匹配内容
    -列表:如果传入列表参数，会将与列表中任一元素匹配的内容返回
    soup.find_all(['a','b'])    #返回所有a标签和b标签
    -True：可以匹配任何值，即返回所有的标签tag对象(字符串节点是不会返回的)
    -方法：如果没有合适的过滤器，可以自定义过滤器，即可以接受一个方法， 方法只接受一个tag标签， 如果这个方法返回True，表示当前元素匹配并且返回，如果返回False，则不匹配  
    def has_class_but_no_id(tag):
        return tag.has_attr("class") and not tag.has_attr("id")
    soup.find_all(has_class_but_no_id)      #返回所有定义了class但是没有定义id的标签  
2.attrs参数：通过定义一个字典来搜索包含特殊属性的tag对象。
    例如一个标签包括id class data-foo属性就可以构建一个字典:data = {'class':'value1','id':'value2','data-foo':'value3'}
    soup.find_all(attrs=data)   #会找到所有tag中包含data字典中定义的属性和值的标签对象
3.recurisive参数：默认情况下，搜索会检索当前tag的所有子孙节点，如果只想搜索tag的直接子节点，可以使用recurisive=False
4.text参数：通过text参数可以搜索文档中的字符串内容
5.**kwargs参数：虽然我们可以通过attrs来创建一个dict来匹配标签的属性，但是写法比较繁琐。而对于像id class这些常用的属性，我们直接可以通过id='value2',href='url'这样的方式指定。甚至可以用id=True来说明具有id该属性即可。而attrs通常用在类似data-foo这样不常用或者自定义的属性上。
    soup.find_all('p',id=True,class_='big') #搜索所有具有id属性，并且class属性为big的p标签。(注意因为class是python的关键字，所以匹配时使用class_)

## 总结
我们在爬取到网站的数据是，需要通过bs4来解析获得的HTML数据。具体的步骤如下：  
1. 通过requests等爬取网页，通过response.text得到网页的字符串  
2. 通过返回响应的字符串，来构建BeautifulSoup对象。soup = BeautifulSoup(response.text,'lxml')
3. 通过find_all()方法来取得需要的节点。通常是使用find_all('div',class_="value")来得到一个data列表  
4. 循环该类表中的数据，通过.string属性来得到列表中标签的内容字符串 
5. 将内容字符串存入数据库或excel等文件中


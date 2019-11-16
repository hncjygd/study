# html介绍

## 什么是html

 - html是Hyper Text Markup Language的缩写，即超文本标记语言
 - html不是一种编程语言，而是一种标记语言
 - 标记语言有自己的一套标记标签
 - html使用标记标签来描述网页
 - 标签在网页中是不会被显示的所以标签被叫做超文本，标签是用来描述网页中文字寓意的

> 重点：
> html是标签语言，每个标签是用来描述每段文字的意义的。  
> 例如：\<h1>html基础\</h1> 表明html基础是一个标题，而具体标题在浏览器中的样式(比如将标题加粗下空一行)这些都是浏览器的默认行为，与html无关。  
> **一定要记住html是给文本添加语义的，其表现样式与html无关**  

## html编写网页格式

### html页面基本结构

``` html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 标题 </title>
    </head>
    <body>
        ...
    </body>
<html>
```

#### html基本结构解释

> \<!DOCTYPE html>  
> 该声明并不是html标签，它是指示web浏览器关于页面使用哪个html版本进行编写的指令。  
> 在html5中使用这个声明。  

- html元素：此元素告诉浏览器其自身是一个html文档
  - 该标签限定了文档的开始点和结束点，他们之间是文档的头部和主体
  - 头部使用head标签定义，主体使用body标签定义
- head元素：定义文档的头部，是所有头部元素的容器
  - 其中可以包含 base link meta script style title 元素
  - 其中title元素是必须有的
  - 一般情况下写在head的内容都不是给用户看的，而是给浏览器看的 
- body元素：定义文档的主体，其中包含文档的所有内容

> 字符集问题  
> html本质上是一个文本文件，所以肯定会有字符集一说。  
> \<meta charset="UTF-8"> 可以用来告诉浏览器使用那一个字符集来解析文档  
> 通常比较常用的中文字符集有GBK(GB2312)  UTF-8. 在实际开发中使用哪个都可以  
> 但是一定要记着，并不是meta中定义了UTF-8就可以了，还要必须保证文件本身是使用utf-8字符集编写的，在meta中的定义仅仅告诉浏览器使用utf-8来解析，如果你文本本身是GBK的，那么解析出来依然是乱码。

### HTML元素
HTML元素指的是从开始标签，到结束标签的所有代码。  
HTML元素语法：
- html元素以开始标签起始
- html元素以结束标签终止
- 元素的内容是开始标签与结束标签之间的内容
- 某些html元素具有空内容
- 空元素以开始标签的结束而结束
具体实例:  

| 开始标签                     | 元素内容                | 结束标签  |
|--------------------------|---------------------|-------|
| \<p>                     | This is a paragraph | \</p> |
| \<a href="default.html"> | this is a link      | \</a> |
| \<br>                    |                     |       |


### HTML简单标签

##### HTML标题

html中标题使用h1~h6等标签进行定义，其中h1定义最大标题，h6定义最小标题。  

> 注意：  
> 浏览器会自动在标题的前后添加空行，而且标题独占一行。   
> 默认情况下，HTML会自动对块级元素前后添加一个额外的空行，段落、标题都属于块级元素  

> 特别注意：  
> 标题非常重要，搜索引擎使用标题为你的网页的结构和内容编制索引。  
> 所以在正常开发中，不要乱用标题，并且一个页面仅仅有一个h1来强调最重要的词语，以便搜索引擎索引  

#### HTML段落

- \<p> 定义一个段落
- \<hr> html使用hr来定义水平线，它是一个空元素
- \<!-- --> 注释，在编辑器中使用 ctrl+/可以快速注释(多种语言中都有)
- \<br> 用于在你希望不产生一个新的段落的情况下进行换行
  - 因为html是用于描述语义的，br的语义就是希望换行但是并不希望另起一个段落。所以在使用中需要想清楚是想要重新开始一个段落还是仅仅希望在段落中换行

``` html
<!-- 不要乱用h1标签 -->
<h1>This is a heading</h1>
<hr>
<p>这是一个段落中的第一行<br>这是一个段落中的第二行<p>
```

#### HTML文本格式化标签

文本格式化：  

- b bold 定义粗体，已经被废弃， strong 代替他定义为着重语气
- i italic 定义斜体，已经被废弃， ins 代替他定义为插入字
- u underline 定义下划线 已经被废弃，em 代替他定义为加重语气，不如strong强烈
- s strikethrough 定义删除线 已经废弃 del 代替他定义为删除字
- code 定义计算机代码
- address 定义地址

根据被废弃的标签我们发现，html正在还原其本质标签用来控制文字的语义，而定义样式的操作交给css来专门干。类似于此像br hr等标签也应该在实际开发中尽量少用。

### 图像标签

img标签用于向网页中嵌入一副图像，从技术上讲，img标签并不是在网页中插入图像，而是从网页上链接图像。img标签创建的是被引用图像的占位空间。  
img标签有两个必须的属性：src 和 alt 属性，可选属性有height width  

#### 标签属性

- html标签可以拥有属性，属性提供了有关html元素的更多的信息
- 属性总是以名称/值对的形式出现，比如 name="value"
- 属性总是在html元素的开始标签中规定 
- 属性值应该始终被包括在引号内，单引号/双引号都可以，但是如果属性中包含了双引号就必须用单引号，反之亦然。
- img标签是行内元素，不换行
- html中定义了一些全局的属性，意味着在每个html标签中都可用
  - class 规定元素的一个或多个类名
  - id 规定元素的唯一id
  - style 规定元素的行内css样式
  - 其他属性可以参考手册


#### 图像标签属性介绍

- src 用于指示图片资源的位置
- alt 用于在图片无法加载时使用的替代文本
- height width 指定图片的宽高
  - 如果我们手动指定了宽高，要考虑图像的比例否则图像会变形。也可以只制定height或者只指定width，这样系统会自动匹配相应的宽高

``` html
<img src="https://www.baidu.com/img/xinshouyedong_4f93b2577f07c164ae8efa0412dd6808.gif" alt="百度图片">
```

#### 路径问题
在使用src引用很多时候并不是引用一个URL外链来引用一个图片的，多数情况下是在本地或则服务器某个位置中存在的图片，这个时候就需要知道引用路径的问题。路径分为两种情况：  

- 相对路径
  - html文件与路径同级别时，直接通过文件名引用即可
    - \<img src="example.png">
  - html文件在图片文件的上一级，假设图片位于一个images文件夹中
    - \<img src="images/example.png">
  - html文件在图片的下一级时，可以使用../表示上一级文件，与linux的习惯相同
    - \<img src="../example.png"> 
- 绝对路径

### 链接标签

html使用a标签来在页面中创建一个链接，可以从一张页面链接到另一个页面。  
超链接可以是一个字、词也可以是一副图像，你可以点击这些内容来跳转到新的文档  
默认情况下，当你把鼠标指针移动到网页中的某个链接上时，箭头会变成小手。  
如果要链接的网页是本机或者同服务器下的文件，也可以使用相对/绝对路径来引用  

链接标签的属性：  
- href 规定链接指向的页面的url
  - 假链接，在你还没有编写完网页的时候，可以先使用一个占位符
    - \# javascript:
    - 以上两种占位符都可以，区别在于#点了以后会回到顶部，而javascript:不变
    - 实际上有些页面中的返回顶部按钮就是通过href="#"实现的
- target 规定在何处打开链接文档
  - _blank 新窗口打开
  - _self 默认，在原窗口打开


``` html
<!-- 点击百度图片来打开百度 -->
<a href="http://www.baidu.com" target="_blank">
    <img src="https://www.baidu.com/img/xinshouyedong_4f93b2577f07c164ae8efa0412dd6808.gif" alt="百度图片">
</a>
```

#### base标签

base标签为页面上的所有链接规定默认地址或默认目标。  
通常情况下，浏览器从当前文档的url中提取相应的元素来填写相对url中的空白。使用base标签可以改变这一点，浏览器将不再使用当前文档中的url，而是使用指定的url来解析所有的相对url，这其中包括a img link form标签中的url。  
在实际开发中，例如img通常会存放在一个单独的图片服务器上，与我们的html文件并不在一个路径下，此时这个base标签就非常有用了。  
注意：base标签是可以空元素标签必须位于head元素内部。其属性：  
- href 规定页面中所有相对链接的基准url
- target 规定页面中所有链接的打开位置

``` html
<head>
<base href="http://www.w3school.com.cn/i/" />
<base target="_blank" />
</head>
```

#### 锚点
使用锚点语法，可以通过a标签在文档内部跳转，这样使用者就无需不停的滚动页面来寻找他们需要的信息了。主要用于为整个页面添加目录。  
要想使用锚点需要几步：  
1. 在需要跳转到锚点的标签上添加id属性
2. 通过a标签中的href="#id_name"来实现跳转

``` html
<p>目录</p>
<a href="#title">html学习</a>
<a href="#head">head标签</a>
<h1 id="title">html学习</h1>
<h2 id="head">head标签</h2>
```

### 列表标签
html中定义了三种类表：  
- 无序列表(unordered list)
- 有序列表(ordered list)
- 定义列表(definition list)

#### 无序列表
给数据添加列表语义，并且这些数据没有先后之分。  
无序标签的定义：
- ul标签定义无序列表
- li标签定义无序列表中的元素
- ul与li标签是一个组合，正常情况下，ul标签内只能存在li标签，如果想要图片等其他元素，可以将这些元素包含在li标签中。

> 重点：  
> 在实际的网页开发中，无序列表是用的非常多的。任何同类数据/信息的集合都可以使用无序列表来定义：新闻、商品列表、导航条等等都是通过无序列表定义的。  

> 无序标签的快速创建  
> ul>li*4  
> 通过上述命令，可以生成一个ul标签其中包含四个li标签。  

```html
<!-- 快速生成代码 -->
<!-- ul>li*2>p+ul>li*3 -->
<ul>
    <li>
        <p></p>
        <ul>
            <li></li>
            <li></li>
            <li></li>
        </ul>
    </li>
    <li>
        <p></p>
        <ul>
            <li></li>
            <li></li>
            <li></li>
        </ul>
    </li>
</ul>
```

``` html
<p>中国的城市：</p>
<ul>
    <li>北京</li>
    <li>武汉</li>
    <li>上海</li>
</ul>
```

#### 有序列表
给数据添加列表语义，并且这些数据都有先后之分。  
其定义与ul差不多，但是将ul改为ol就是有序列表。  
其是用ol以及li来组合定义有序列表。


#### 定义列表
定义列表表示的语义为定义一个标题，并且为这个标题定义描述。  
定义列表用dl(definition list)标签定义，其中包含dt(definition title)标签来定义列表的标题，dd(definition description)标签来描述所定义标题。  

> 实际开发中的用处：  
> 1.做网站尾部的相关信息  
> 2.做图文混排  
> 实际开发中注意：  
> 1.dl下应当只包含dt dd
> 2.dt可以对应多个dd但是不建议这样做，应当一个dt对应一个dd
> 3.和li一样，如果想要丰富页面，可以在dt和dd标签中嵌套其他标签来实现

``` html
<dl>
    <dt>北京</dt>
    <dd>中国的首都</dd>
    <dt>香港</dt>
    <dd>中国的特别行政区</dd>
</dl>
```

### 表格
表格由table标签定义，每个标签有若干行(由tr标签定义，table row),每行被分割为若干单元格（由td标签定义，table data），数据单元格中可以包含文本、图片、列表、段落、表单、水平线以及其他表格等内容。   
- table 定义表格
- tr    table row定义表格行
- td    table data 定义表格单元格
- th    table heading 定义表格表头 通常使用他来代替td表示表头单元格(th也位于tr标签下)
- 表格的结构标签
  - caption 定义表格的标题
  - thead 表格的表头信息也可以看作页眉信息
  - tbody 定义表格的主体
  - tfoot 定义表格的页脚信息

``` html
<!--表格的完整结构 -->
<table>
    <caption>表格的标题</caption>
    <thead>
        <tr>
            <th>表格的表头信息</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>表格的数据</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>表格的页脚信息</td>
        </tr>
    </tfoot>
</table>
```

#### 表格的属性


控制样式的属性：这些属性仅仅作为了解，正常开发都是使用css来控制的：  

1. 宽度和高度属性 width height
   1. 可以给table标签和td标签使用
   2. 表格的宽度和高度默认是根据内容的尺寸来调整的
   3. 可以为table标签设置width height属性的方式来手动指定表格的宽度和高度
   4. 如果给td标签设置width height属性，会修改当前单元格的宽度和高度，不会影响整个表格的宽度和高度，这也意味着其他表格会改变
2. 水平对齐和垂直对齐的属性 align valign
   1. 水平对齐 align 可以给table tr td标签使用
   2. 垂直对齐 valign 只能给 tr td标签使用
   3. 给table标签设置align属性，可以控制整个表格在水平方向的对齐方式  
   4. 给tr标签设置 align valign 属性可以控制当前行的所有单元格内容的对齐方式
   5. 给td标签设置 align valign 属性可以控制该单元格的对齐
3. 外边距和内变距属性
   1. 外边距就是单元格与单元格之间的距离 cellspacing 默认为2px
   2. 内变距就是单元格的边框与单元格内容之间的距离 cellpadding 默认为1px


细线表格的制作方式：  
1. 给table标签设置bgcolor:设置整个表格的背景颜色
2. 给tr标签设置bgcolor：设置整行的背景颜色
3. 给table标签设置cellspacing='1px'

单元格合并：
1. colspan 来指定将一个单元格在水平方向上当作多个单元格来看待。
2. rowspan 来指定将一个单元格在垂直方向上当作多个单元格来看待。

### HTML表单
html表单用于搜集不同类型的用户输入。  
form标签定义html表单，其中可以包括 input元素 select元素 textarea元素 button元素 datalist元素 keygen元素 output元素，后三个是HTML5中新定义的。  

- form标签属性
  - action 规定向何处提交表单的地址
  - method 规定提交表单所用的http方法，默认GET
- input标签属性
  - type 用于定义输入类型，其有很多取值，这个属性取值的不同决定了input标签不同的功能和外观。
  - select option 定义下来菜单
  - textarea 定义文本输入框
  - datalist 绑定待选项，需要与input绑定


#### 输入类型

- text 单行文本域 可以通过value属性添加默认值，通过list属性与datalist绑定
- password 单行密码域 可通过value属性添加默认值
- radio 单选框，默认不互斥，可通过设置相同的name属性来保证互斥，通过设置checked来选默认值
- checkbox 复选框，也通过checked属性来选择默认值
- button 按钮，通过type属性设置按钮上标题，用于配合js完成相应内容
- submit 提交表单中数据，提交服务器由form标签中的action指定，需要提交的任何在form标签中定义的具有name属性的表单元素
  - 在表单元素中，除了按钮标签外，都可以通过value来制定需要提交给服务器的数据，类似与text textarea password email等标签其中输入就是赋值给value的，但是类似于radio checkbox等标签必须显示指定value标签才能向服务器提交准确的数据
- reset 重置 重置所有表单中填写的数据
- email number url data等HTML5中提供的type用于字段检查等

``` html
<form action="http://www.baidu.com">
    <!-- text输入类型定义供文本输入的单行输入字段
    可以通过value属性来添加默认值 -->
    <!-- label 如果直接写文字，该文字与输入框是没有任何内在联系的，虽然他们看着在一行，例如你点击文字并不会激活输入框。要想实现点击文字激活输入框就需要将其绑定到一个label标签中
    1.给需要绑定的input标签中定义一个id属性
    2.在lebel标签中通过for属性等于input的id属性值即可绑定 -->
    <label for="accout">帐号：</label><input type="text" id="accout"><br>
    <!-- password 密码输入框
    可以通过value属性来添加默认值 -->
    密码：<input type="password"><br>
    <!-- radio单选框，
    默认情况下单选框并不互相排斥，要想互斥必须设置name属性且属性值相同 
    可以使用checked属性来设置默认值，checked的值就是 "checked"所以可以不用写，如果写多个checked的情况下默认选择最后面的 -->
    性别：
    <input type="radio" name="gender" checked="checked" value="male">男
    <input type="radio" name="gender" value="female">女
    <input type="radio" name="gender" value="sec">保密 <br>
    <!-- checkbox 多选框 
    可以选择多个，也可以使用checked默认多个选择-->
    爱好：
    <input type="checkbox" name="favourite" value="basketball">篮球
    <input type="checkbox" name="favourite" value="football">足球
    <input type="checkbox" name="favourite" value="ball">排球
    <!-- button 定义一个按钮，通过value属性定义按钮上的文字，主要用于配合js完成一些操作 -->
    <input type="button" value="注册" onclick="alert('注册成功')"> <br>
    <!-- datalist 给输入框绑定待选项，类似与百度中的联想输入
    datalist需要与一个input绑定才可以使用，通过input中的list属性  
    datalist并不是强制的，你也可以在input中输入任何值 -->
    毕业院校：
    <input type="text" list="school"> <br>
    <datalist id="school">
        <option>北京大学</option>
        <option>清华大学</option>
        <option>复旦大学</option>
    </datalist>
    <!-- select标签，下拉列表 
    他不是一个input标签的属性而是一个新的标签，其不能输入，只能在你的option标签中的文字中选择-->
    选择你的地址：
    <select>
        <option>北京</option>
        <option>上海</option>
        <option selected="selected">武汉</option>
    </select> <br>
    <!-- textarea标签，定义一个多行输入框
    1.默认情况下输入框可以无限输入
    2.默认情况下有固定大小可以通过cols来设置宽rows来设置高度 -->
    自我介绍：
    <textarea cols="30" rows="10"></textarea> <br>
    <!-- submit 提交数据按钮，默认value值就是提交，
        其作用是将保单中的数据提交到远程服务器，其需要满足两个条件：
    1.告诉表单需要将数据提交到哪个服务器：通过在form标签中定义action属性来实现
    2.告诉表单那些数据需要提交：通过在需要提交的表单元素中添加name属性，任何有name属性的表单元素中输入的内容都将被提交
    3.提交的值是标签元素中的value属性，类似text password textarea等等待输入的表单元素会将输入赋值给value属性，但是类似radio checkbox这些无法输入的元素需要自行指定value，否则提交的数据为name_value=on这些并不可用 -->
    <!-- reset 重置按钮，不设置value值的情况下默认显示重置文字，其作用用于清空表单中的数据 -->
    <input type="submit">
    <input type="reset">
    <!-- hidden隐藏域 用于在用户不知觉的情况下上传一些数据，主要用来搜集用户数据，他在页面中没有表现形式是隐藏的 -->
    <!-- h5中提供了一些type类型， email url number color等用于检查输入相应的内容的，但是由于ie很多不支持所以兼容性并不强 -->
</form>
```


### 字符实体

在HTML中有的字符是被HTML保留的，有的在HTML中有特殊的含义，这些是不能再浏览器中显示出来的，那么这些东西要想显示出来就必须通过字符实体来显示。

- \&nbsp; 表示空格(在html中对空格/回车/tab不敏感，会把多个空格/回车/tab当作一个空格来处理，所以要想添加多个空格就要使用这个字符实体)
- \&lt; 小于号 <
- \&gt; 大于号 >
- \&copy; &copy;版权符号
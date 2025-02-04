
## 第一次运行vim

在命令行输入vim就可以进入vim，后面也可以跟文件名: vim file 来在打开vim的同时打开文件。如果没有接文件名，可以在vim中通过e命令打开文件：  

> :e file_path  

当我们刚打开一个vim的时候是无法输入文字的，甚至都不知道如何退出，你可能随意按下键盘的某些键发现可以输入文字了。其实这就牵扯到Vim的各种模式。首先我们想来说明如何退出vim:  

> :q  
> :q!   强制退出  
> :wq   保存后退出  
> 如果发现以上命令无法执行，多按几次Esc在执行命令

## vim的模式

我们在刚进入vim的时候是工作在vim的Normal模式下，可以在左下角发现一个Normal字样，就代表Normal模式，该模式下我们可以通过一些按键来执行相应的命令。如果我们想要在其中插入文本就需要在Normal模式下执行相应的命令来进入插入模式：  

> a 在字符后插入 append  
> i 在字符前插入 insert  
> o 在下一行插入  
> O 在上一行插入  
> A 在行尾插入  
> I 在行首插入  

当我们在Normal模式下按下上面任意一个按键(需要在英文模式下)都可以进入插入模式，他们的不同之处在于光标跳转到不同的位置插入。在插入模式下的操作与其他编辑器并没有什么不同，屏幕上会实时显示你打出的字符。如果我们需要退出Insert模式，按下Esc键就看可以了，Esc就是你键盘左上角的那个按键。通常情况下命令都是在Normal下执行的，如果按下了某些快捷键并没有出现什么效果可以按下Esc再试试。  
还有一个模式是CommandLine模式，通过在Normal模式下按下冒号(:)键来进入命令行模式，此时你会发现vim界面的最下端显示了一个:等待你输入命令，我们的保存、退出都是在命令行模式下执行的：  

> :w    保存  
> :q    退出  
> :wq   保存并退出  
> :q!   强制退出，不保存文件


## 光标移动

光标移动是编辑器中最常见的命令了。在vim下通常光标移动都是在Normal进行的，而且定义了特别奇葩的四个按键来移动光标即: h j k l 也就是你右手键盘定位位置附近的四个按键，其分别对应了左移、下移、上移、右移(非常好奇为啥不使用wasd或者ijkl,游戏爱好者的福音啊)，这里有一个记忆的方法就是j象征着下箭头代表向下移动，不过要想形成肌肉记忆，唯一的方法就是不断的练习。需要注意的是hjkl命令只能在Normal模式下进行移动，所以如果处于插入模式下需要按下Esc来退到Normal模式下在进行移动。也许你会问我要在插入模式下怎么移动光标呢，对于初学者来说直接使用方向键就完事了，方向键可以在任意模式下移动光标。但是还是作为初学者并不建议你使用方向键，毕竟vim最NB的地方就是双手不离开键盘进行编辑操作，而方向键确实太远了，还是需要多多使用hjkl来进行移动，其并不比方向键慢，即使需要按下Esc和i等进入插入模式的命令，但是作为初学者需要的是肌肉记忆。

### 词移动

上面介绍的hl作为左右移动仅仅能够移动一个字符，效率并不高，vim提供了词移动命令，可以让你在每个词之间进行跳转。  

> w 移动到下一个单词首部  
> e 移动到下一个单词末尾  
> b 移动到上一个单词首部  
> ge 移动到上一个单词的尾部  

需要了解的是，在Vim中定义了文本对象的概念，其中词、句子、段落等都是内置的文本对象，而以上操作说白了就是对文本对象的操作。我们这里只讲解对于词文本对象的基本移动操作，类似句子、段落等都有相应的移动操作。  
在Vim中对于词的解释是一个由字母数字组成的单词，通常用空格或标点符号隔开来区分每一个单词，而标点符号也被作为一个单词来看待。需要注意的是对于词的理解在中文环境下可能会让人疑惑，需要注意的是vim并不能区真正意义上中文的词，而是通过在中文文章中到找英文字符、标点符号、空格来区分一个词的。

## 文本改动

文本改动，说白了就是对文本的增删改查。对文本的增加可以通过在插入模式下输入来完成。我们这里主要讲解的就是对文本的删除、修改和查找。  
这里需要知道vim中命令操作的格式：  

> operation + motion  

我们通过灵活组合operation和motion来执行各种各样的动作。而operation就是操作的意思，vim中定义了一系列的operation(可以通过:help operator命令来查看)其中我们经常使用的是c(change)、d(delete)、y(yank)分别表示更改、删除、赋值操作。而我们上面讲解的光标移动都是一些motion。  
所以根据operation + motion这样的格式，d$命令就是删除到行尾。我们可以灵活运用这个写命令来达到删改的目的。  
还有一个vim下的特点就是motion下可以跟数字来表执行多少次motion，例如4j就是表示下移4行，使用数字来结合以上的操作，例如d4w就是表示删除4个单词(其实并不是严格意义上的删除了四个单词，而是从光标处删除到4w移动的位置)

### 删除操作

我们在上面已经讲解了vim中操作的基本格式，其实vim对一些常用的操作也定义了一些简单的快捷键来完成删除操作，不过这些快捷键多数都可以分解成operation + motion这样的格式。  

> x 删除光标所在位置的字符，实际上等同于dl  
> D 删除至行尾，实际上等同于d$  
> dd 删除光标所在行，实际上等同于0d$   

可以看出他们实际上就是一些操作+动作组合的简化操作形式，如果你实在不想记这么多快捷键的情况下，记住一些operation和常用的motion就可以了。  

### 更改操作

更改操作依然遵循了operation+motion这样的格式，只不过是其将d换成了c而已，更改操作会经你motion中字符删除并进入插入模式，所以说你非要使用d来删除文本后使用i进入插入模式，而不使用c也完全可以，vim的强大之处就在于你怎么玩都可以，并不限制你玩的方式，而操作效率也是因人而异的，没有那种是绝对正确的，适合自己的才是最好的，而高手操作流程无他唯手熟尔。废话不说来看看更改操作。  

> r{char} 以char替换光标下的字符，实际上等同于cl\<Esc>  
> R 进入替换模式，不断替换光标下的字符  
> cc 修改本行内容，实际上等同于 0c$ ddi等命令组合  

### 复制操作

我们就不详细讲复制操作了，如果想要知道最详细的情况:help yank来查看，如果想要知道最详细的文本改动操作通过:help change.txt来查看。  
虽然不再将复制了，其实也没啥可将的，无非就是将d换成y，被删除的内容没有被删除而是被复制到了寄存器中，可以使用p(put)来粘贴出来而已。我们这里需要了解的是vim中删除并没有真正的删除掉文本，而是也将其放入到寄存器里了，当我们使用d来删除一些字符的情况下，也可以使用p来将字符粘贴出来。

### 文本对象

我们前面在讲解移动命令的时候讲过文本对象的概念，其实就是vim中定义了词、句子、段落等文本对象，我们可以在其之上进行操作。而w e b这些移动命令都是基于文本对象来说的，可以称这些命令为文本对象动作。我们这里要将的是文本对象不同于文本对象动作，动作都是可以单独执行的，而文本对象本身不能单独执行需要与operation配合使用，所以可以看作一个不能独立运行的动作。  

> aw 包含一个单词(around word)  
> iw 内含一个单词(inner word)  
> aW 包含一个字串(around WORD)  
> iW 内含一个字串(inner word)  
> a" a{ a<等 包含在" { <等中  
> i" i{ i<等 内含在" { <等中  

我们已经讲过word和WORD的区别了，最大的区别就是WORD中仅仅使用空格/Tab来作为分割一个单词的依据，而word中空格、标点符号都可以作为分割单词的依据，并且标点本身也属于一个词。  
而其中还有一个概念就是around和inner，其中around表示包括周围的东西，对于一个词来说就是包括词之间的空格，而对于inner而言则不包括。一个词可能不好理解，如果是一个类似(prams)这样的函数参数列表形式，i(不包括空格本身，而a(则包括括号本身。  
上面我们已经讲解了文本对象的概念，可能你会疑惑这有什么用。可以这么说文本对象的概念极大的扩展了vim中operation的操作范围。举例说明，如果你想要修改一个词直接使用ciw就可以了，无论光标在词的什么位置，如果没有文本对象你可能需要先将光标定位到词首然后使用ce等命令来修改。而类似上面更改括号内的内容直接执行ci(就可以了。非常的方便。  
这还不是文本对象的终点，一些插件还定义了一些其他文本对象，例如块、函数体等等，我们都可以这样操作他们。

## 总结

本教程仅仅是vim的上手教程，学完本教程对于你修改个配置文件足够使用了。但是如果你想用vim来编辑其他文件比如写代码啥的，以上的内容也不是不可以实现，就是会非常痛苦低效。这就需要你继续探索vim的其他功能，你可以通过读vim的帮助来进一步学习他，也可以期待我后续的教程^-^

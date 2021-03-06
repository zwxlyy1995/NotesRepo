# MarkDown语法学习笔记
* * *

- [MarkDown语法学习笔记](#markdown语法学习笔记)
  - [<a id='chapter1' href="#">第一章</a>](#第一章)
    - [1.1 设计理念](#11-设计理念)
    - [1.2 内联HTML语法](#12-内联html语法)
    - [1.3 特殊字符转义](#13-特殊字符转义)
  - [<a id="chapter2" href="#">第二章</a>](#第二章)
    - [2.1 注释表达](#21-注释表达)
    - [2.2 分级标题 任务列表](#22-分级标题-任务列表)
    - [2.3 缩进 换行 空行 对齐方式](#23-缩进-换行-空行-对齐方式)
    - [2.4 斜体 粗体 删除线 下划线 背景高亮](#24-斜体-粗体-删除线-下划线-背景高亮)
    - [2.5 超链接 页内链接 自动链接 注脚](#25-超链接-页内链接-自动链接-注脚)
    - [2.6 无序列表 有序列表 定义型列表](#26-无序列表-有序列表-定义型列表)
    - [2.7 插入图像](#27-插入图像)
    - [2.8 多级引用](#28-多级引用)
    - [2.9 转义字符 字体 符号 颜色](#29-转义字符-字体-符号-颜色)
  - [第三章](#第三章)
    - [3.1 内容目录](#31-内容目录)
    - [3.2 代码块](#32-代码块)
    - [3.3 流程图](#33-流程图)
    - [3.4 表格](#34-表格)
    - [3.5 LaTeX公式](#35-latex公式)
    - [3.6 分割线](#36-分割线)
    - [3.7 HTML原始码](#37-html原始码)
    - [3.8 特殊字符](#38-特殊字符)

* * *
## <a id='chapter1' href="#">第一章</a>


### 1.1 设计理念

* MarkDown 纯文本 不包含标记标签和指令化语句
* ...
  

### 1.2 内联HTML语法

*  HTML是发布格式，MarkDown是创作格式
*  MarkDown可以直接使用HTML的标签
*  ...

### 1.3 特殊字符转义

&emsp;&emsp;在 HTML 中, 有两个字符需要特殊对待: < 和 &，左尖括号用于起始标签。如果你想将它们用作字面量, 你必须将它们转义为字符实体, 例如&lt; 和 &amp;。

* * *
## <a id="chapter2" href="#">第二章</a>

### 2.1 注释表达

* 代码法
```html
<div style="display:none">
注释内容
</div>
```
* html注释
```html
<!-- 注释内容 -->
```
* hack法
```
[//]: # (哈哈我是最强注释，不会在浏览器中显示。)
[^_^]: # (哈哈我是最萌注释，不会在浏览器中显示。)
[//]: <> (哈哈我是注释，不会在浏览器中显示。)
[comment]: <> (哈哈我是注释，不会在浏览器中显示。)

```

### 2.2 分级标题 任务列表

* 分级标题

写法：
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题  <!--最多6级标题-->
```

* 任务列表
  
写法：
```
- [ ] 任务一 未做任务 `- + 空格 + [ ]`
- [x] 任务二 已做任务 `- + 空格 + [x]`
```
效果：
- [ ] 任务一 未做任务 `- + 空格 + [ ]`
- [x] 任务二 已做任务 `- + 空格 + [x]`

### 2.3 缩进 换行 空行 对齐方式

* 首行缩进

写法：
```
【1】 &emsp;或&#8195; //全角
【2】 &ensp;或&#8194; //半角
【3】 &nbsp;或&#160;  //半角之半角
```
效果：
【1】 &emsp;或&#8195; //全角
【2】 &ensp;或&#8194; //半角
【3】 &nbsp;或&#160;  //半角之半角

* 换行

&emsp;&emsp;由于markdown编辑器的不同,可能在一行字后面，直接换行回车，也能实现换行，但是在Visual Studio Code上，想要换行必须得在一行字后面空两个格子才行。

* 空行

&emsp;&emsp;在编辑的时候有多少个空行(只要这一行只有回车或者space没有其他的字符就算空行)，在渲染之后，只隔着一行。

* 对齐方式

写法：
```
<center>行中心对齐</center>
<p align="left">行左对齐</p>
<p align="right">行右对齐</p>
```
效果：
<center>行中心对齐</center>
<p align="left">行左对齐</p>
<p align="right">行右对齐</p>

### 2.4 斜体 粗体 删除线 下划线 背景高亮

写法：
```
*斜体* _斜体_
**粗体** __粗体__
***加粗斜体*** ___加粗斜体___
~~删除线~~
<u>下划线</u>
==背景高亮==
```
效果：
*斜体* _斜体_
**粗体** __粗体__
***加粗斜体*** ___加粗斜体___
~~删除线~~
<u>下划线</u>
==背景高亮==

### 2.5 超链接 页内链接 自动链接 注脚

* 行内式 和 自动链接

语法：
```
[链接名称](链接地址 "悬停时显示的文字") 或者 [链接名称](链接地址) 或者 <链接地址>
```
例子：
```
这是一个行内式超链接[百度主页](https://www.baidu.com/ "百度主页")
这是一个自动超链接<https://www.baidu.com/>
```
效果：
这是一个行内式超链接[百度主页](https://www.baidu.com/ "百度主页")
这是一个自动超链接<https://www.baidu.com/>

* 参考式

语法：
```
[链接文字][链接标记]
在文本的任意位置添加[链接标记]:链接地址。
```
例子：
```
我经常去的几个网站[Google][1]、[Leanote][2]。

[1]:http://www.google.com 
[2]:http://www.leanote.com

```
效果：
我经常去的几个网站[Google][1]、[Leanote][2]。

[1]:http://www.google.com 
[2]:http://www.leanote.com

* 注脚

语法：在需要添加注脚的地方加上`[^脚注名字]`，成为加注。然后在文本任意位置添加脚注，**脚注前必须有对应的脚注名字；注脚和脚注必须空一行，否则会失效；MarkDown转换后自动将脚注归到文章最后。**
例子:
```
使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2]。

[^1]:Markdown是一种纯文本标记语言

[^2]:HyperText Markup Language 超文本标记语言
```
效果：
使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2]。

[^1]:Markdown是一种纯文本标记语言

[^2]:HyperText Markup Language 超文本标记语言

* 锚点(页内超链接)

&emsp;&emsp;网页中，锚点其实就是页内超链接，也就是链接本文档内部的某些元素，实现当前页面中的跳转。比如我这里写下一个锚点，点击回到目录，就能跳转到目录。 在目录中点击这一节，就能跳过来。还有下一节的注脚。这些根本上都是用锚点来实现的，只支持在标题后插入锚点，其它地方无效。
语法：
```
<a id='#jump' href="#">第一个题目</a>
带有锚点的题目 其中href值为你要跳跃的锚点的 #+name值(#指代这是一个锚点)

##<a name='jump'>跳转的锚点</a>
要跳到的锚点处，保持name值与题目href值一直即可(也可以加##修改成大标题哦)
```
例子:
<a href='#chapter1'>跳转至第一章标题处</a>
<a href='#chapter2'>跳转至第二章标题处</a>

### 2.6 无序列表 有序列表 定义型列表

* 无序列表

语法： *，+，- 表示无序列表。
写法：
```
* 无序列表项 一
+ 无序列表项 二
- 无序列表项 三
```
效果：
* 无序列表项 一
+ 无序列表项 二
- 无序列表项 三

* 有序列表

语法：数字接着一个英文的句点符号
写法：
```
1. 有序列表项 一
2. 有序列表项 二
3. 有序列表项 三
```
效果:
1. 有序列表项 一
2. 有序列表项 二
3. 有序列表项 三

* 定义型列表

语法：定义型列表由名词和解释组成。一行写上定义，紧跟一行写上解释。解释的写法:紧跟一个缩进(Tab)
写法：
```
MarkDown
:   轻量级文本标记语言（左侧有一个可见的冒号和四个不可见的空格）
```
效果:
MarkDown
:   轻量级文本标记语言（左侧有一个可见的冒号和四个不可见的空格）

### 2.7 插入图像

语法：
```
![图片alt](图片地址 "图片Title")
```
例子：
![Runoob](http://static.runoob.com/images/runoob-logo.png "Runoob")
Markdown 还没有办法指定图片的高度与宽度，如果需要的话，可以使用普通的 &lt;img&gt; 标签。
```
<img src="http://static.runoob.com/images/runoob-logo.png" width="50%">
```
<img src="http://static.runoob.com/images/runoob-logo.png" width="50%">

### 2.8 多级引用

语法：引用需要在被引用的文本前加上>符号和空格，允许多层嵌套
写法：
```
>>> 请问 Markdwon 怎么用？ - 小白

>> 自己看教程！ - 愤青

> 教程在哪？ - 小白
```
效果：
>>> 请问 Markdwon 怎么用？ - 小白

>> 自己看教程！ - 愤青

> 教程在哪？ - 小白

### 2.9 转义字符 字体 符号 颜色

* 转义字符

\ 反斜杠 ` 反引号 * 星号 _ 下划线 {} 大括号 [] 中括号 () 小括号  # 井号 + 加号 - 减号 . 英文句号 ! 感叹号

* 字体

写法：
```
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=12 face="黑体">黑体</font>
<font color=gray size=5>gray</font>
<font color=#00ffff size=3>null</font>
```
效果:
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=12 face="黑体">黑体</font>
<font color=gray size=5>gray</font>
<font color=#00ffff size=3>null</font>

* * *
## 第三章

### 3.1 内容目录

在段落中填写 [TOC] 以显示全文内容的目录结构。

### 3.2 代码块

* 行内式
写法：
```
C语言里的函数 `scanf()` 怎么使用？
```
效果：
C语言里的函数 `scanf()` 怎么使用？

* 缩进式多行代码

语法：缩进4个空格或者一个制表符
写法：
```
#include &lt;stdio.h&gt;
int main(void)
{
    printf(&#34;Hello world\n&#34;);
}

```
效果:
    #include &lt;stdio.h&gt;
    int main(void)
    {
        printf(&#34;Hello world\n&#34;);
    }

* 用6个 `  包裹，还可以指定语言类型

写法：
```
    ```cpp
    include <stdio.h>
    int main(void)
    {
    printf("Hello world\n");
    }
    ```
```
效果：
```cpp
include <stdio.h>
int main(void)
{
printf("Hello world\n");
}
```

### 3.3 流程图

写法:
```
    ```
    graph LR
    A-->B
    ```

    ```
    sequenceDiagram
    A->>B: How are you?
    B->>A: Great!
    ```
```
效果：
```
graph LR
A-->B
```

```
sequenceDiagram
A->>B: How are you?
B->>A: Great!
```

### 3.4 表格

语法:
* 不管是哪种方式，第一行为表头，第二行分隔表头和主体部分，第三行开始每一行为一个表格行。
* 列于列之间用管道符|隔开。原生方式的表格每一行的两边也要有管道符。
* 第二行还可以为不同的列指定对齐方向。默认为左对齐，在-右边加上:就右对齐。
\- 左对齐， :-: 中心对齐，-: 右对齐

写法：
```
|学号|姓名|序号|
|-|-|-|
|小明明|男|5|
|小红|女|79|
|小陆|男|192|

```
效果:
|学号|姓名|序号|
|-|-|-|
|小明明|男|5|
|小红|女|79|
|小陆|男|192|

### 3.5 LaTeX公式

* 行内公式
  
写法：
```
质能守恒方程可以用一个很简洁的方程式 `$E = m c^2 $`来表达。
```
效果:
质能守恒方程可以用一个很简洁的方程式 `$E = m c^2 $`来表达。

* 整行公式

大部分浏览器支持：
```
$$ 公式 $$
```
或者使用块级公式：
```math
x = \dfrac{-b \pm \sqrt{b^2 - 4ac}}{2a} 
```
```math
[\frac{1}{\Bigl(\sqrt{\phi \sqrt{5}}-\phi\Bigr) e^{\frac25 \pi}} =
1+\frac{e^{-2\pi}} {1+\frac{e^{-4\pi}} {1+\frac{e^{-6\pi}}
{1+\frac{e^{-8\pi}} {1+\ldots} } } }]

```

### 3.6 分割线

语法：在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。可以在星号或是减号中间插入空格。
写法：
```
* * *
***
*****
- - -
-----------
```

### 3.7 HTML原始码

&emsp;&emsp;在代码区块里面， & 、 < 和 > 会自动转成 HTML 实体，这样的方式让你非常容易使用 Markdown 插入范例用的 HTML 原始码，只需要复制贴上，剩下的 Markdown 都会自动处理。
例子：
```
第一个例子：
<div class="footer">
© 2004 Foo Corporation
</div>
第二个例子：
<center>

<table>
<tr>
<th rowspan="2">值班人员</th>
<th>星期一</th>
<th>星期二</th>
<th>星期三</th>
</tr>
<tr>
<td>李强</td>
<td>张明</td>
<td>王平</td>
</tr>
</table>

</center>
```
效果：
第一个例子：
<div class="footer">
© 2004 Foo Corporation
</div>
第二个例子：
<center>

<table>
<tr>
<th rowspan="2">值班人员</th>
<th>星期一</th>
<th>星期二</th>
<th>星期三</th>
</tr>
<tr>
<td>李强</td>
<td>张明</td>
<td>王平</td>
</tr>
</table>

</center>

### 3.8 特殊字符

|特殊字符|描述|字符代码|
|:-:|-|-|
| |空格|`&nbsp;`|
|&lt;|小于号|`&lt;`|
|&gt;|大于号|`&gt;`|
|&amp;|和号|`&amp;`|
|&yen;|人名币|`&yen;`|
|&copy;|版权|`&copy;`|
|&reg;|注册商标|`&reg;`|
|&deg;C|摄氏度|`&deg;C`|
|&plusmn;|正负号|`&plusmn;`|
|&times;|乘号|`&times;`|
|&divide;|除号|`&divide;`|
|&sup2;|平方上标|`&sup2;`|
|&sup3;|立方上标|`&sup3;`|

* * *
主要参考自：https://www.jianshu.com/p/ebe52d2d468f#fn1
部分参考自：https://www.runoob.com/markdown/md-tutorial.html
<p align="right">zwxupc@163.com 2020.10.09</p>
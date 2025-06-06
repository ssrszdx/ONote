# 关于 CSS 级联的优先级

‍

下一步基于选择符和它特殊的程序来给规则排序。特殊性如下计算：

1．内联样式属性（inline style attribute）是规则最特殊的规则类型。如果有特殊CSS属性的内联样式规则，该规则就“胜出”。

2．id选择符是第二特殊的规则。如果规则有多个id选择符，具有id选择符最大数的规则胜出。

3．如果没有id选择符，或者数值相同，那么就计算规则中类或伪类的数量。有最多类或伪类的规则有较高的优先权。

4．如果类（或没有类）的数量相同，比较元素的数量——元素数量越多，特殊性越高。

5．如果id值、类和元素的数量都相同，最近声明的规则有最高优先权。如果两条规则具有相同的选择符，并对特殊CSS属性的值具有不同意见，那么位于第二的规则更加特殊。

    如果用户遇到了不分胜负的情况，那么继续考察下一条规则。例如，如果用户具有一个复杂规则，它有三个id选择符、三个class选择符和两个元素选择符，而另外一个具有三个id选择符、三个class选择符和一个元素选择符，那么用户具有相同的id选择符和相同的class选择符，因此要解决级联，用户需要继续考察元素选择符的数量来进一步做出决定。

**《精通Web标准网页布局：XHTML+CSS+JavaScript 》

**    使用样式的优先级是指使用不同方法为元素定义样式时的优先级问题。在上节讲解的3种使用样式的方法中，元素中直接使用样式表的方法的优先级最高，然后是从头部调用的样式表，最后是从外部调用的样式表。

**《CSS手册简编》**

    Cascading Style Sheet中的Cascading是"层叠"的意思，也就是说在同一个Web文档中可以有多个样式单存在，这些样式单根据所在的位置，拥有不同的优先级，优先级越高，就会被最后在显示时采用。从样式单插入的形式来看可以分为三种：

1. 内联式样式单：它利于现有的HTML标记，把特殊的样式加入到那些由标记控制的信息中，比如刚才的例子。
2. 嵌入式样式单：它和Javascript一样可以嵌入到HTML文件的头部中去（<html>和<body>标记之间），使用<Style>和</Style>容器装载，例如："<style> p {color : red ; font-weight : bold} </style>"，这样会对页面中所有<p>标记都起作用。

3.外部式样式单：是一种保存在外部的样式单文件，外部文件以.CSS为扩展名，例如"<link rel=stylesheet href="main-sheet.css" type="text/css">"。

    在应用时可以根据需要随意运用以上三种方式，但在实际中内联式样式单和嵌入式样式单使用得更多一些。

**《CSS样式使用优先级判断》**

1. 多个选择器可能会选择同一个元素，有3个规则，从上到下重要性降低：

    ！important的用户样式  
    ！important的作者样式  
    作者样式  
    用户样式  
    浏览器定义的样式
2. CSS样式的特殊性权重--谁有分量，谁说了算。

CSS规范为不同类型的选择器定义了特殊性权重，特殊性权重越高，样式会被优先应用。  
权重设定如下：  
html选择器，权重为1；  
类选择器， 权重为10；  
id选择器， 权重为100；  
在html标签中直接使用style属性，则此处的style属性的权重为1000；

例：  
h1{color:blue}             权重为1  
p em{color:yellow}         权重为2  
.warning{color:red}        权重为10  
p.note em.dark{color:grag} 权重为22  
# main{color:black}         权重为100

这里还有一种情况：权重一样时如何处理？权重一样时就另说了。看看下面的就明白了。

3. CSS样式的层叠原则--谁离我近，谁说了算。

当权重一样时，会采用“层叠原则” 后定义的会被应用。

如：p{color:yellow}  
     p{color:red}

作用到这里   <p>我的什么颜色呢？</p>  
结果会是red的。

4. CSS样式的特殊标记--谁有特权，谁说了算。

如果有人看不顺眼，非得要自己说了算，那可以搞点特权，如下即可

p {color:blue !important;}

加上!important;可将自己权重设为最高。  
如果你要问两个!important;设定的样式，那个样式说了算，我说你为什么不自己试试看看呢！

‍

sass less   css 預處理器

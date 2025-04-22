# Css小结

# 

第14课：元素的定位

CSS定位令你可以将一个元素精确地放在页面上你所指定的地方。联合使用定位与浮动（参见[第13课](http://zh.html.net/tutorials/css/lesson13.php)），你将能够创建多种高级而精确的布局。

本课我们将讨论以下内容：

* [CSS定位的原理](http://zh.html.net/tutorials/css/lesson14.php#s1)
* [绝对定位](http://zh.html.net/tutorials/css/lesson14.php#s2)
* [相对定位](http://zh.html.net/tutorials/css/lesson14.php#s3)

## CSS定位的原理

把浏览器窗口想象成一个坐标系统：

![带有坐标的浏览器窗口](df8939a5034cfcf76fb12065adba9f85-20220205111530-dcmm2zu.gif)

CSS定位的原理是：你可以将任何盒子（box）放置在坐标系统的任何位置上。

假设我们要放置一个标题。通过使用盒状模型（参见[第9课](http://zh.html.net/tutorials/css/lesson9.php)），标题将显示如下:

![盒子中的标题](e6e43dd504bacdbacce6acadb770d6c9-20220205111530-s31dkhm.gif)

如果我们要把这个标题放置在距文档顶部100像素、左边200像素的地方，我们可以在CSS中输入以下代码：

```

	h1 {
		position:absolute;
		top: 100px;
		left: 200px;
	}


```

得到的显示效果如下：

![在浏览器窗口里定位标题](707dac72299ca240a7e42907073209f3-20220205111530-7l5d87b.gif)

正如你所看到的，采用CSS定位技术来放置元素是非常精确的。相对于使用表格、透明图像或其他方法而言，CSS定位要简单得多。

## 绝对定位

一个采用绝对定位的元素不获得任何空间。这意味着：该元素在被定位后不会留下空位。

要对元素进行绝对定位，应将`position`属性的值设为 **absolute** 。接着，你可以通过属性 **left** 、 **right** 、**top**和**bottom**来设定将盒子放置在哪里。

举个绝对定位的例子，假如我们要在文档的四个角落各放置一个盒子：

```

	#box1 {
		position:absolute;
		top: 50px;
		left: 50px;
	}

	#box2 {
		position:absolute;
		top: 50px;
		right: 50px;
	}

	#box3 {
		position:absolute;
		bottom: 50px;
		right: 50px;
	}

	#box4 {
		position:absolute;
		bottom: 50px;
		left: 50px;
	}


```

* [显示示例](http://zh.html.net/tutorials/css/lesson14_ex1.php "显示上述代码")

## 相对定位

要对元素进行相对定位，应将`position`属性的值设为 **relative** 。绝对定位与相对定位的区别在于计算位置的方式。

采用相对定位的元素，其位置是**相对于它在文档中的原始位置**计算而来的。这意味着，相对定位是通过将元素从原来的位置向右、向左、向上或向下移动来定位的。采用相对定位的元素会获得相应的空间。

举个相对定位的例子，我们可以相对于三张图片在页面上的原始位置来对它们进行相对定位。注意这些图片将在文档中各自的原始位置处留下空位。

```

	#dog1 {
		position:relative;
		left: 350px;
		bottom: 150px;
	}
	#dog2 {
		position:relative;
		left: 150px;
		bottom: 500px;
	}

	#dog3 {
		position:relative;
		left: 50px;
		bottom: 700px;
	}


```

* [显示范例](http://zh.html.net/tutorials/css/lesson14_ex2.php "显示上述代码")

## 小结

在以上两课中，你学会了如何浮动和定位元素。这两个方法可以令你在进行页面布局时，放弃使用HTML表格和透明图像这些过时的方法，而是取而代之以CSS。CSS更为精确、更具优势、并且更易于维护。

‍

# 

第13课：浮动元素（float）

我们可以通过CSS属性`float`令元素向左或向右浮动。也就是说，令盒子及其中的内容浮动到文档（或者是上层盒子）的右边或者左边（参见[第9课](http://zh.html.net/tutorials/css/lesson9.php)关于盒状模型的描述）。下图阐明了其原理：

![一个向左浮动的盒子](9332d4801d70b92719ae46287b363135-20220205111551-5ompfqb.gif)

举个例子，假如我们想让一张图片被一段文字围绕着，那么其显示效果将如下所示：

![一个包含被文字环绕的图片的向左浮动的盒子](05e114e809f90ddb53833f13bbac153b-20220205111551-n4tj908.gif)

## 如何实现这个效果？

上例的HTML代码如下所示：

```

	<div id="picture">
		<img src="bill.jpg" alt="Bill Gates">
	</div>

	<p>causas naturales et antecedentes, 
	idciro etiam nostrarum voluntatum...</p>


```

要实现图片向左浮动、而且被文字环绕的效果，你只需先设定图片所在盒子的宽度，然后再把CSS属性`float`设为left即可：

```

	#picture {
		float:left;
		width: 100px;
	}


```

* [显示示例](http://zh.html.net/tutorials/css/lesson13_ex1.php "显示上述代码")

## 另一个例子：列

浮动也可以用于实现在文档中分列。要创建多个列，你需要在HTML代码里用`div`来结构化想要的各个列：

```

	<div id="column1">
		<p>Haec disserens qua de re agatur
		et in quo causa consistat non videt...</p>
	</div>

	<div id="column2">
		<p>causas naturales et antecedentes, 
		idciro etiam nostrarum voluntatum...</p>
	</div>

	<div id="column3">
		<p>nam nihil esset in nostra 
		potestate si res ita se haberet...</p>
	</div>


```

下面，我们把各列的宽度设定为“33%”，并通过定义`float`属性令各列向左浮动：

```

	#column1 {
		float:left;
		width: 33%;
	}

	#column2 {
		float:left;
		width: 33%;
	}

	#column3 {
		float:left;
		width: 33%;
	}


```

* [显示示例](http://zh.html.net/tutorials/css/lesson13_ex2.php "显示上述代码")

`float`属性的值可以是 **left** 、**right**或者 **none** 。

## clear属性

CSS属性`clear`用于控制浮动元素的后继元素的行为。

缺省地，后继元素将向上移动，以填补由于前面元素的浮动而空出的可用空间。在前面的例子中，文本自动上移到了比尔盖茨的图片旁。

`clear`属性的值可以是 **left** 、 **right** 、**both**或 **none** 。原则是这样的：如果一个盒子的`clear`属性被设为“both”，那么该盒子的上边距将始终处于前面的浮动盒子（如果存在的话）的下边距之下。

```

	<div id="picture">
		<img src="bill.jpg" alt="Bill Gates">
	</div>

	<h1>Bill Gates</h1>

	<p class="floatstop">causas naturales et antecedentes, 
	idciro etiam nostrarum voluntatum...</p>


```

要避免文本上移到图片旁，我们可以在CSS中添加以下代码：

```

	#picture {
		float:left;
		width: 160px;
	}

	.floatstop {
		clear:both;
	}


```

* [显示示例](http://zh.html.net/tutorials/css/lesson13_ex3.php "显示上述代码")

## 小结

浮动在很多情况下都很有用，它经常与定位联合使用。在[下一课](http://zh.html.net/tutorials/css/lesson14.php)，我们将深入了解如何进行盒子的绝对或相对定位。

‍

# 

第15课：用z-index进行层次堆叠

CSS可以处理高度、宽度、深度三个维度。在前面的课程中，我们已经了解了前两个维度。在本课中，我们将学习如何令不同元素具有层次。简言之，就是关于元素堆叠的次序问题。

为此，你可以为每个元素指定一个数字（`z-index`）。其原理是：数字较大的元素将叠加在数字较小的元素之上。

比方说，我们正在打扑克，并且拿了一手同花大顺。我们可以通过为各张牌设定一个`z-index`的方式来表示这手牌：

![同花大顺](54e0e0b311f6187946b2f7b4746fbb6c-20220205111619-7lfgwfe.gif)

在这个例子中，我们采用了1-5五个连续的数字来表示堆叠次序，但是你也可以用五个不同的其他数字来取得同样的效果。这里的要点在于：用数字的大小次序反映希望的堆叠次序。

扑克牌这个例子的代码可以这样写：

```

	#ten_of_diamonds {
		position: absolute;
		left: 100px;
		top: 100px;
		z-index: 1;
	}

	#jack_of_diamonds {
		position: absolute;
		left: 115px;
		top: 115px;
		z-index: 2;
	}

	#queen_of_diamonds {
		position: absolute;
		left: 130px;
		top: 130px;
		z-index: 3;
	}

	#king_of_diamonds {
		position: absolute;
		left: 145px;
		top: 145px;
		z-index: 4;
	}

	#ace_of_diamonds {
		position: absolute;
		left: 160px;
		top: 160px;
		z-index: 5;
	}


```

* [显示示例](http://zh.html.net/tutorials/css/lesson15_ex1.php "显示上述代码")

该方法虽然简单，但却能应付许多情况。你可以将图片叠加到文本之上，也可以将文本叠加到文本之上等等。

## 小结

层次可以应用于许多情况之下。例如，可以用`z-index`实现为标题（headline）增添效果（避免了采用图片的方式）。这样，一方面，装载文本的速度比图片要快；另一方面，采用文本可能更有利于提高网站的搜索引擎排名。

‍

# 

第10课：外边距和内边距

上一课，你学习了盒状模型。在这一课，我们将了解如何通过设置`margin（外边距）`和`padding（内边距）`这两个CSS属性来改变元素的显示。

* [为元素设置外边距](http://zh.html.net/tutorials/css/lesson10.php#s1)
* [为元素设置内边距](http://zh.html.net/tutorials/css/lesson10.php#s2)

## 为元素设置外边距

一个元素有上（top）、下（bottom）、左（left）、右（right）四个边。`外边距（margin）`表示从一个元素的边到相邻元素(或者文档边界)之间的距离。可以参考[第9课](http://zh.html.net/tutorials/css/lesson9.php)的图示。

在下面这个例子中，我们将了解如何为文档本身（即`body`元素）定义外边距。下图显示了我们对外边距的要求。

![外边距的示例](4fb0ca82b502ec23ea8b33653720b0ca-20220205111640-uhxo8x2.gif)

满足上述要求的CSS代码如下：

```

	body {
		margin-top:100px;
		margin-right:40px;
		margin-bottom:10px;
		margin-left:70px;
	}


```

或者你也可以采用一种较优雅的缩写形式：

```

	body {
		margin: 100px 40px 10px 70px;
	}


```

* [显示示例](http://zh.html.net/tutorials/css/lesson10_ex1.php "显示上述代码")

几乎所有元素都可以采用跟上面一样的方法来设置外边距。例如，我们可以为所有用`<p>`标记的文本段落定义外边距：

```

	body {
		margin: 100px 40px 10px 70px;
	}

	p {
		margin: 5px 50px 5px 50px;
	}


```

* [显示示例](http://zh.html.net/tutorials/css/lesson10_ex2.php "显示上述代码")

## 为元素设置内边距

内边距（padding）也可以被理解成“填充物”。这样理解是合理的，因为内边距并不影响元素间的距离，它只定义元素的内容与元素边框之间的距离。

下面我们通过一个简单的例子来说明内边距的用法。在这个例子中，所有标题都具有背景色：

```

	h1 {
		background: yellow;
	}

	h2 {
		background: orange;
	}


```

* [显示示例](http://zh.html.net/tutorials/css/lesson10_ex3.php "显示上述代码")

通过为标题设置内边距，你可以控制在标题文本周围填充多少空白：

```

	h1 {
		background: yellow;
		padding: 20px 20px 20px 80px;
	}

	h2 {
		background: orange;
		padding-left:120px;
	}


```

* [显示示例](http://zh.html.net/tutorials/css/lesson10_ex4.php "显示上述代码")

## 小结

你正在逐步精通CSS盒状模型。在下一课，我们将进一步了解如何将边框设置为不同颜色、以及如何改变元素形状。

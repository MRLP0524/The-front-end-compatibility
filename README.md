# The-front-end-compatibility

##0.前言

感觉读者现在基本都已经习惯了我的套路，但是还是有几位读者让我“猝不及防”呀。

例如我在上期文章中，说各位都习惯了我的墨迹，结果...

![](http://upload-images.jianshu.io/upload_images/693359-603b91f651bba257.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一口老血就喷屏幕上了！~

![](http://upload-images.jianshu.io/upload_images/693359-6c9ab3571b7a73d8.gif?imageMogr2/auto-orient/strip)

如果你的留言足够有趣，在下一期文章中，也会有机会上文章哦！~  😝😝😝

***

在前两天，看了我男神（龙哥）专门去讲解兼容性的视频，觉得真心好，所以专门花时间来给大家一起分享一下。

废话不多说，直接上干货。

今天分节太多，就全用副标题了，大家根据自己喜好查看。

另外补充一点，今天代码的演示效果均使用的模拟器中的 IETester，请注意。

##1.什么是兼容性？

所谓前端的兼容性，是一个非常大的话题。

我们今天就只来看一看在页面布局方面有哪些兼容性的问题。

今天的兼容性问题主要发生在“前端三毒”身上，它们分别是：

1. IE 6
2. IE 7
3. IE 8

##2.问题一：宽度计算问题

 案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		#box{
			width: 400px;

		}
		.left{
			width: 200px;
			background: red;
			height: 300px;
			float: left;
		}
		.right{
			width: 200px;
			float: right;
		}
		.div{
			width: 180px;
			height: 180px;
			padding: 15px;
			background: blue;
		}
	</style>
</head>
<body>
	<div id="box">
		<div class="left"></div>
		<div class="right">
			<div class="div"></div>
		</div>
	</div>
</body>
</html>
```

 上面的这段代码在标准浏览器中的显示情况如下：

![](http://upload-images.jianshu.io/upload_images/693359-a0fad4ab50ffd063.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而在 IE 低版本浏览器中是一个什么样子呢？

![](http://upload-images.jianshu.io/upload_images/693359-fc8432b14ddaed1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我勒个去，什么情况？

这其实是因为，在 IE 6 下，子级的宽度会撑开父级设置好的宽度。

而解决方法也非常简单，将 **子级元素的宽度设置为小于等于父级元素的宽度 **就可以了。

除此之外，也推荐大家**在盒模型的计算一定要准确，否则在 IE 下回出现问题**。

##3. 浮动时的兼容性问题（一）

在前端兼容性中，浮动这里也是兼容性的一个重灾区，例如下面的示例代码。

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.box{
			width: 400px;
		}
		.left{
			background: red;
			float: left;
		}
		.right{
			background: blue;
			float: right;
		}
		h3{
			margin: 0;
			height: 40px;
		}
	</style>
</head>
<body>
	<div id="box">
		<div class="left">
			<h3>左侧</h3>
		</div>
		<div class="right">
			<h3>右侧</h3>
		</div>
	</div>
</body>
</html>
```

在标准浏览器中没有任何问题。

![](http://upload-images.jianshu.io/upload_images/693359-42d5c31b99313cb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而在低版本浏览器中是什么样子呢？

![](http://upload-images.jianshu.io/upload_images/693359-73490cfee64203b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这又是因为什么呢？

**在 IE 6 中，元素浮动中，如果元素需要宽度进行撑开，需要给里面的块元素都添加浮动才可以。**

那么解决方案也非常简单，我们只需要给他们的内容也进行浮动就可以。

![](http://upload-images.jianshu.io/upload_images/693359-c8c9d96f4ce754bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##4. 浮动时的兼容性问题（二）

除了上面的问题，我们再来看一个问题。

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.left{
			width: 100px;
			height: 100px;
			background: red;
			float: left;
		}
		.right{
			width: 200px;
			height: 100px;
			background: blue;
			margin-left: 100px;
		}
	</style>
</head>
<body>
	<div id="box">
		<div class="left"></div>
		<div class="right"></div>
	</div>
</body>
</html>
```

标准浏览器中：

![](http://upload-images.jianshu.io/upload_images/693359-03e22400b26bf0c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

妥妥的没任何问题，但是当在 IE 低版本中运行呢？

![](http://upload-images.jianshu.io/upload_images/693359-bf9b289755cfe267.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

发现了么？中间多出来了一个间隙（3px）。

这是因为，**在 IE 6/7 下，元素要通过浮动排在同一排，就需要给这行元素都加浮动**。

所以我们怎么办？

该怎么实现还是要怎么实现，别想着偷懒。

![](http://upload-images.jianshu.io/upload_images/693359-33de554316f2ddff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##5.IE 低版本中的最小高度问题

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		div{
			height: 2px;
			background: red;
		}
	</style>
</head>
<body>
	<div></div>
</body>
</html>
```

标准浏览器中显示正常：

![](http://upload-images.jianshu.io/upload_images/693359-dcda6a27dc46610f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而在 IE 6 下，你会发现一个问题。

![](http://upload-images.jianshu.io/upload_images/693359-77609d2d0a405bf6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

WTF? 我明明设置的是 2px ，为什么出来这么粗一根？

这其实是因为，在 IE 6 下元素的高度如果小于 19px 的时候，会被当做 19px 处理。

那么我们怎么解决？

其实非常简单，直接通过 `overflow: hidden;` 就可以解决。

##6. IE 中的边框

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		div{
			width: 100px;
			height: 100px;
			border: 1px dotted red;
		}
	</style>
</head>
<body>
	<div></div>
</body>
</html>
```

在标准浏览器中：

![](http://upload-images.jianshu.io/upload_images/693359-476b38bfd3b7a570.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而 IE 6 下是一个什么样子呢？？

![](http://upload-images.jianshu.io/upload_images/693359-b360f16ebbada77f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

惊了，这是怎么回事？

要知道，**在 IE 6 下不支持 1px 的 dotted 边框样式**。

那假如美工大爷非要用这个样式呢？

我们其实可以通过背景平铺去解决，这里就不去演示了，大家肯定都会，对吧。

##7. margin 的问题

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.box{
			background: red;
			border: 1px solid red;
			zoom:1;
		}
		.div{
			width: 100px;
			height: 100px;
			background: blue;
			margin: 100px;
		}
	</style>
</head>
<body>
	<div class="box">
		<div class="div"></div>
	</div>
</body>
</html>
```

大家都知道，子级的第一个元素的 margin-top 会直接传递给父级，对吧。

![](http://upload-images.jianshu.io/upload_images/693359-79d8360220925103.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而我们其实也可以通过 设置一个 border 去解决这个问题。

这时候在上面的 margin 就可以生效了。

![](http://upload-images.jianshu.io/upload_images/693359-7f550b31ee9d619b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但是，问题来了，你猜在 IE 低版本中是什么样子呢？

![](http://upload-images.jianshu.io/upload_images/693359-d95aa528e683b198.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

原因：**在 IE 下，父级有边框的时候，子级的 margin 会失效。**

为什么会出现这种情况呢？

这其实是在 IE 6/7/8 中才存在的一个属性，[hasLayout](http://baike.baidu.com/link?url=sHWElF5PBdxEd_TNPDDvwED5axx_qdU2lQ9Jme7UZVSEi4saZpti6eUInlek0IrZ4bgrP70R6BNj0sEks-VDrK)。

在 IE 下的大部分兼容性问题都是因为 hasLayout 属性触发的问题，尽量触发 hasLayout 可以减少 IE 的问题。

至于触发 hasLayout 的方法有非常多，但比较常用的就是 `zoom: 1;`，有兴趣的小伙伴可以自己尝试一下。

##8.IE 中双边距的 BUG

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		body{
			margin: 0;
		}
		.box{
			width: 200px;
			height: 200px;
			background: red;
			float: left;
			margin: 100px;
		}
	</style>
</head>
<body>
	<div class="box"></div>
</body>
</html>
```

 上述这段代码，在标准浏览器中，呈现的模样：
 

![](http://upload-images.jianshu.io/upload_images/693359-8f36565d68e16cf4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 
 而在低版本浏览器中，呈现的样式则完全不同。
 

![](http://upload-images.jianshu.io/upload_images/693359-518c8505a9ceadd4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 
这个也就是 IE 6 下非常著名的 双倍边距 BUG。

这个 BUG 的出现原理是，**在 IE 6 下，块元素有浮动并且横向的时候有 margin 的时候，横向的 margin 会扩大两倍**。

那知道原因之后，解决起来也就非常简单了。

既然块元素会出现这个问题，那我们就将其转换为内联元素。

这样就不会产生这个问题了。


![](http://upload-images.jianshu.io/upload_images/693359-2d8023691b73a09e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##9.margin 的 双边距效果

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.box{
			border: 10px solid red;
			float: left;
		}
		.box div{
			width: 100px;
			height: 100px;
			background: red;
			margin-left: 30px;
			border: 5px solid #000;
			float: left;
		}
	</style>
</head>
<body>
	<div class="box">
		<div>1</div>
		<div>2</div>
		<div>3</div>
		<div>4</div>
	</div>
</body>
</html>
```

这段代码如果放在标准浏览器中，显示的妥妥的没问题。


![](http://upload-images.jianshu.io/upload_images/693359-7157a5132b8c90fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但是如果放在低版本浏览器中是个什么样子呢？？


![](http://upload-images.jianshu.io/upload_images/693359-afbd1c82c5b185ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


你会发现，margin-left 一行中最左侧的第一个元素有双边距，而如果换成margin-right 的话，margin-right 一行中最右侧的第一个元素有双边距。

这个问题的解决方案和上面的那个案例相同，我们可以直接将其转换成 **内联元素**，这样的话，这个问题就可以直接解决了。


![](http://upload-images.jianshu.io/upload_images/693359-b64b8ea7be4364f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##10.li 的浮动间隙问题

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		ul{
			margin:0;
			padding: 0;
			list-style: none;
			width: 300px;
		}
		li{
			list-style: none;
			height: 30px;
			border: 1px solid #000;
		}
		a{
			width: 100px;
			height: 30px;
			float: left;
			background: red;
		}
		span{
			width: 100px;
			height: 30px;
			float: right;
			background: blue;
		}
	</style>
</head>
<body>
	<ul>
		<li>
			<a href="javascript:void(0)"></a>
			<span></span>
		</li>
		<li>
			<a href="javascript:void(0)"></a>
			<span></span>
		</li>
		<li>
			<a href="javascript:void(0)"></a>
			<span></span>
		</li>
	</ul>
</body>
</html>
```

上面的代码在标准浏览器中是怎么显示的呢？


![](http://upload-images.jianshu.io/upload_images/693359-54534caf3cce7a0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


和咱们预料的是相同的，对吧。

但是，你们猜猜在 IE 6 和 IE 7 中是怎么显示的呢？？


![](http://upload-images.jianshu.io/upload_images/693359-d3aae751c14c2029.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**在 IE 6/7 下，li 本身没有浮动，li 里面的内容有浮动，li 下会产生一个间隙。**

那么这个问题该如何解决呢?

这里给大家推荐两个解决方案。

1. 给 li 也添加浮动
2. 给 li 添加 vertical-align

先来看看给 li 添加浮动的做法。


![](http://upload-images.jianshu.io/upload_images/693359-9b0886f86aa13e60.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但是注意一个问题，这样在 IE 下确实能够解决这个问题，但是相应的，在标准浏览器中，中间的白色块会消失。


![](http://upload-images.jianshu.io/upload_images/693359-9a45bdfd249d5590.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


所以，我们推荐使用第二种做法，就是去添加 vertical-align。

大家还记得我在之前写的一篇文章，专门说 img 标签在下面有一条白色间隙的文章么？

[如何解决 img 标签上下出现的间隙](http://www.jianshu.com/p/796de6ac0943)

这个问题的解决方案同样也是需要修改 vertical-align 的 属性，将默认的 baseline 修改为其他内容。


![](http://upload-images.jianshu.io/upload_images/693359-b42d1f3c9151dd3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##11. 当最小高度和 li 问题一起发生的时候

基于上面的 li 问题，如果我们同时触发最小高度和 li 的问题的时候，该怎么办呢？

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		ul{
			margin:0;
			padding: 0;
			list-style: none;
			width: 300px;
		}
		li{
			list-style: none;
			height: 12px;
			border: 1px solid #000;
		}
		a{
			width: 100px;
			height: 12px;
			float: left;
			background: red;
		}
		span{
			width: 100px;
			height: 12px;
			float: right;
			background: blue;
		}
	</style>
</head>
<body>
	<ul>
		<li>
			<a href="javascript:void(0)"></a>
			<span></span>
		</li>
		<li>
			<a href="javascript:void(0)"></a>
			<span></span>
		</li>
		<li>
			<a href="javascript:void(0)"></a>
			<span></span>
		</li>
	</ul>
</body>
</html>
```

标准浏览器中的样式：


![](http://upload-images.jianshu.io/upload_images/693359-13469e9f958eb156.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


而在低版本浏览器中呢？

![](http://upload-images.jianshu.io/upload_images/693359-672d8dfb5660e321.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这时候肯定有很多小伙伴开始动脑筋了，既然两个问题在一起，那我就直接将两个问题的解决代码一起加上呗。


![](http://upload-images.jianshu.io/upload_images/693359-2c6e2feb4f87d25e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这时候，你会发现，最小高度的问题确实解决了，但是间隙还是存在呀？

这时候就是我要给大家提示的一个问题，在 IE 6 下，最小高度的 BUG 和 li 的间隙问题共存的时候，使用 vertical-align 是无效的。

我们必须要使用 `float : left;`。

##12.前端三毒中最恐怖的一毒

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.box{
			border: 10px solid red;
		}
		.box div{
			width: 100px;
			height: 100px;
			background: blue;
			border: 5px solid #000;
			margin: 20px;
			float: left;
			display: inline;
		}
	</style>
</head>
<body>
	<div class="box">
		<div>1</div>
		<div>2</div>
		<div>3</div>
		<div>4</div>
	</div>
</body>
</html>
```

鉴于这个问题非常“恐怖”，我们一点点的来分析。

首先咱们先来看看，初始状态现在是什么样子？

标准浏览器：


![](http://upload-images.jianshu.io/upload_images/693359-51d738261c0a5f60.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 
IE 低版本：
 

![](http://upload-images.jianshu.io/upload_images/693359-bc2cd722aef89233.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 
而且，我们发现不仅仅在下方的 margin 失效了，并且发现在前面还存在一个双边距的现象。

那我们怎么办呢？

我们可以去添加一个宽度。


![](http://upload-images.jianshu.io/upload_images/693359-6fd9ae2515974101.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 
这时候在 IE 浏览器下显示正常了。

![](http://upload-images.jianshu.io/upload_images/693359-b7a63a0c31551706.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但是相对的，在标准浏览器中会失效。


![](http://upload-images.jianshu.io/upload_images/693359-1bed9630493a4bab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这时候怎么办呢？

我们可以去添加一个 overflow。

![](http://upload-images.jianshu.io/upload_images/693359-660d21f20f17fd88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这时候，显示确实正常了。

注意：前方高能！！！

当我们将宽度一像素，一像素的增加的时候。


![](http://upload-images.jianshu.io/upload_images/693359-f519a9b94444107a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当你增加到 3px 的时候（作者这里增加到 5px 才触发这个现象，应该是模拟器不准确），会突然发现，底部的 margin 又失效了。


![](http://upload-images.jianshu.io/upload_images/693359-765115df4838b6b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


而且还存在另外一个非常诡异的现象。

我们首先将刚才的代码做一下修改。


![](http://upload-images.jianshu.io/upload_images/693359-8d55436e300d93ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这时候显示是正常的。


![](http://upload-images.jianshu.io/upload_images/693359-3b6b677303469e79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但是当我们注释某一个 div 的时候（删除也可以），刚才的现象又发生了。

（作者这里注释了两个，为大家演示一下，注释一个也是相同的。）


![](http://upload-images.jianshu.io/upload_images/693359-bdc913708424bff0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


实际上，这其实是因为当一行子级元素宽度之和和父级的宽度相差超过 3px，或者 子级元素不满行的情况时，最后一行的子级元素的 margin-bottom 会失效。

这个问题没法解决，只能够避免。

这点一定要注意。

##13.文字溢出 BUG

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.box{
			width: 400px;
		}
		.left{
			float: left;
		}
		.right{
			float: right;
			width: 400px;
		}
	</style>
</head>
<body>
	<div class="box">
		<div class="left"></div>
		<span></span>
		<div class="right">李先生真帅</div>
	</div>
</body>
</html>
```

这个 BUG 是怎么回事呢？

运行上面的代码，在正常浏览器内显示没有任何问题。


![](http://upload-images.jianshu.io/upload_images/693359-26e59824bc9320db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但是需要注意，如果在 IE 6里面，你会发现一个奇怪的现象。


![](http://upload-images.jianshu.io/upload_images/693359-667c9d968d686030.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们下面怎么多了一个“帅”字呀？

如果你在下面多加一个 `span` 标签呢？

两个 span：


![](http://upload-images.jianshu.io/upload_images/693359-e615e6a90cde96ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

三个 span：


![](http://upload-images.jianshu.io/upload_images/693359-1ffccbf243868188.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



四个 span：

![](http://upload-images.jianshu.io/upload_images/693359-1b47ebb6969d7189.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们会发现，甚至于我们的第一行文字直接全部消失了。

这个就是 IE 6 下的文字溢出 BUG.

**子元素的宽度和父级的宽度如果相差小于 3px 的时候，两个浮动元素中间有注释或者内联元素的时候，就会出现文字溢出，内联元素越多，溢出越多.**

而且，如果中间存在注释，也会触发 BUG.

![](http://upload-images.jianshu.io/upload_images/693359-43deebdfe8715b4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


既然我们将问题提了出来，那就肯定有解决的办法。

我们推荐，**用 块元素标签 把注释或者内联元素包裹起来。**

这样就可以有效的避免触发这个 BUG 了。

##14.绝对定位失效 BUG

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.box{
			width: 200px;
			height: 200px;
			border: 1px solid #000;
			position: relative;
		}
		a{
			position: absolute;
			width: 40px;
			height: 40px;
			background: red;
			right: 20px;
			top: 20px;
		}
		ul{
			width: 150px;
			height: 150px;
			background: yellow;
			margin: 0 0 0 50px;
			padding: 0;
			float: left;
			display: inline;
		}
	</style>
</head>
<body>
	<div class="box">
		<ul></ul>
		<a href="javascript:void(0)"></a>
	</div>
</body>
</html>
```

上面的案例代码，在标准浏览器中显示完全没有问题。


![](http://upload-images.jianshu.io/upload_images/693359-c097c845f585b259.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但是当我们在低版本浏览器中，显示就会出现问题了。


![](http://upload-images.jianshu.io/upload_images/693359-20dd394930ce860e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这个问题的触发原因：

**在 IE 6 下，当浮动元素和绝对定位元素是兄弟关系的时候，绝对定位会失效。**

那我们该怎么解决呢？

非常简单，**不让浮动元素和绝对定位是兄弟关系**就可以咯。

我们可以用 div 标签嵌套一下，这样分隔开来就不会触发刚才的问题了。

##15.滑动元素 BUG

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.box{
			width: 200px;
			height: 200px;
			border: 1px solid #000;
			overflow: auto;
		}
		.div{
			width: 150px;
			height: 300px;
			background: red;
			position: relative;
		}
	</style>
</head>
<body>
	<div class="box">
		<div class="div"></div>
	</div>
</body>
</html>
```

这时候我们在标准浏览器中查看一下。


![](http://upload-images.jianshu.io/upload_images/693359-8ebdc4452cdcfe52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


不管是滑动还是其他方式都没问题，但是，IE 下就没问题了么？


![](http://upload-images.jianshu.io/upload_images/693359-7ca0e8365eac1d0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


哼哼，前端三毒，名不虚传。
 
问题原因在这：**在 IE 6/7 下，子级元素有相对定位，父级 overflow 包不住子级元素。**
 
那我们该怎么处理这个问题？
 
既然子元素有相对定位，那我们就给父元素同样给出相对定位呗。
 

![](http://upload-images.jianshu.io/upload_images/693359-1a3b47c4f1dbb6b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 
这样就能轻松解决掉这个问题了。
 
##16.像素偏差 BUG
 
案例代码：
 
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.box{
			width: 201px;
			height: 201px;
			border: 1px solid #000;
			position: relative;
		}
		.box span{
			width: 20px;
			height: 20px;
			background: yellow;
			position: absolute;
			right: -1px;
			bottom: -1px;
		}
	</style>
</head>
<body>
	<div class="box">
		<span></span>
	</div>
</body>
</html>
```

上面的代码，在 标准浏览器中可以正常显示。


![屏幕快照 2016-09-28 11.40.07.png](http://upload-images.jianshu.io/upload_images/693359-4d9657b94e814b61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但是如果放在 IE 中显示呢？


![](http://upload-images.jianshu.io/upload_images/693359-1245701e47d6f462.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这个问题的原因：

**在 IE 6 下，如果绝对定位的父级宽和高是奇数的时候，子级元素的 right 和 bottom 会有1像素的偏差。**

至于解决方案，不好意思，没有。


![](http://upload-images.jianshu.io/upload_images/693359-a4692f5b6c0c4109.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个东西只能去尽量避免，如果没办法，只能这么做，那你就要和美工大爷提前商量好了，要么你改，要我我弄完你别找我的事。（我以前就这么干的，羞涩...）

##17.固定定位 BUG

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		body{
			height: 2000px;
			background: red;
		}
		.box{
			width: 200px;
			height: 200px;
			background: yellow;
			position: fixed;
			top: 30px;
			left: 100px;
		}
	</style>
</head>
<body>
	<div class="box"></div>
</body>
</html>
```

我们在标准浏览器中，显示是正常的。

![](http://upload-images.jianshu.io/upload_images/693359-00eda700748928a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


而在 IE 浏览器中，你会发现，咱们设置的 固定定位失效了！


![](http://upload-images.jianshu.io/upload_images/693359-17fcac2ce8f9b3a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这其实是因为，在 IE 6 中不存在固定定位，只存在绝对定位。

那我们该怎么做呢？

在 IE 低版本下，我们只能通过 JS 去模拟这个效果。

##18.透明度失效

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		body{
			height: 2000px;
			background: red;
		}
		.box{
			width: 200px;
			height: 200px;
			background: yellow;
			position: fixed;
			top: 30px;
			left: 100px;

			opacity: 0.5;
		}
	</style>
</head>
<body>
	<div class="box"></div>
</body>
</html>
```

我们在应用了定位之后，经常会用到 `opacity`这个属性，在标准浏览器中，显示是没有任何问题的。


![](http://upload-images.jianshu.io/upload_images/693359-2b7e6a1f3e48f966.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是如果在 IE 中去查看呢？

你会发现，我的透明度失效了！


![](http://upload-images.jianshu.io/upload_images/693359-c357cdf62ed483bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这其实是因为，在 IE 中其实是不认识 opacity 这个属性的。

那我们如果想要在 IE 中使用透明度，该怎么做呢？

我们可以这么用。

![](http://upload-images.jianshu.io/upload_images/693359-bc9fb0bea547ad91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这样我们的透明度就都可以正常使用了。

##19.input 元素的 BUG

案例代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		.box{
			width: 200px;
			height: 32px;
			border: 1px solid #000;
		}
		input{
			width: 100px;
			height: 30px;
			border: 1px solid red;
			margin: 0;
			padding: 0;
		}
	</style>
</head>
<body>
	<div class="box">
		<input type="text" name="">
	</div>
</body>
</html>
```

在标准浏览器中显示是没有什么异常的。


![](http://upload-images.jianshu.io/upload_images/693359-bb8661e8682528f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是，在 IE 6 和 IE 7 中，输入型表单控件上下会有 1px 的间隙。

![](http://upload-images.jianshu.io/upload_images/693359-87cff2b8af156a48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


那我们怎么去解决呢？

我们其实可以通过设置浮动就能解决。

![](http://upload-images.jianshu.io/upload_images/693359-6316689db89e49b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##20. IE 中不支持 png 图片的问题

我们在平时的开发中，难免会遇到各种各样的图片，很多时候都是我们的美工大爷给我们提前切好的，但是我们偶尔还是需要自己弄一些图片。

那在处理图片之后，大家一般使用的图片格式都是什么呢？

他们之间有哪些区别呢？我们今天首先来看一下。

这里我首先通过 PS 去制作了一张图片，具体的图片制作方式在此不做更多说明。

重点在图片的导出的时候，我们在导出时，不同的图片格式都有什么差别呢？

首先我们先来看看 GIF 的导出。

![](http://upload-images.jianshu.io/upload_images/693359-c2629cdbe6eb029d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们会发现，图片要么是直接透明的，要么是不透明的，不存在渐变这个效果。

而 JPEG 呢？


![](http://upload-images.jianshu.io/upload_images/693359-ff09907ab4a6d550.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


完全没有透明效果。

而 我们常用的 PNG 格式呢？

这里首先来看看 PNG-8

![](http://upload-images.jianshu.io/upload_images/693359-4db03a8d0abdeff6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


和 GIF 非常类似,但是大小要小很多。

最后我们再来看看 PNG-24.


![](http://upload-images.jianshu.io/upload_images/693359-c482f16f80c09e3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们会发现，我们图片的大小略大，但是透明效果全部存在。

明白了上面各种图片的特性之后，我们再来看看下面的案例。

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		body{
			background: #000;
		}
		.box{
			width: 400px;
			height: 400px;
			background: url("1.png");
		}
	</style>
</head>
<body>
	<div class="box"></div>
</body>
</html>
```

在标准浏览器中，显示完全没有问题。


![](http://upload-images.jianshu.io/upload_images/693359-21a3350725390b5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


那么接下来一起再来看看 IE 中的显示。

![](http://upload-images.jianshu.io/upload_images/693359-1ac4e8f661b3faec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


发现了么？

我们的透明效果完全失效了，我们使用的可是 PNG-24 呀。

这是为什么呢？

这其实是因为，在IE 6 下，不支持透明度的方式。

那我们怎么去处理这个问题呢？

我们其实可以通过一个 JS 文件来处理它。

处理工具：[DD_belatedPNG](http://www.dillerdesign.com/experiment/DD_belatedPNG/#download)

使用起来其实也非常简单。

![](http://upload-images.jianshu.io/upload_images/693359-c455e61ff9608cfe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这样在 IE 6下，我们的 PNG 图片也可以正常显示啦。

##21.条件注释语句

接下来给大家介绍一个小套路，就是条件注释语句，这个是我们平常处理兼容时很常用的一个小技巧。

使用方式也非常简单。


![](http://upload-images.jianshu.io/upload_images/693359-cfc298e1a7704c41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意，这里的书写方式是固定的，不能够更改。

那我们写这个有什么效果呢？

我们会发现，在标准浏览器中我们是看不见任何东西的。


![](http://upload-images.jianshu.io/upload_images/693359-f477af89d857601a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但是在特定的 IE 版本中，我们可以看到注释之间的文字。


![](http://upload-images.jianshu.io/upload_images/693359-7b66dd38a98ac5ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


甚至我们也可以在苹果官网中看到这样的处理方法。


![](http://upload-images.jianshu.io/upload_images/693359-fba5b637c02b024e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##22.CSS HACK

最后再给大家介绍一个差不多算是“走火入魔”的处理方法。

就是我们其实可以根据不同的 IE 浏览器版本去执行特定的代码。

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		/* CSS HACK */
		.box{
			width: 100px;
			height: 100px;
			background: red;
			background: yellow\9;
			+background: black;
			_background:pink;

			/*
			
			\9	 	：	IE 10 之前的浏览器解析的代码
			+ 或者 * ：	IE 7（包括 7）之前的 IE 浏览器
			_ 		：	IE 6（包括 6）之前的 IE 浏览器

			 */
		}
	</style>
</head>
<body>
	<div class="box"></div>
</body>
</html>
```

我们可以看到，如果我当前的版本是 IE 7，我可以在 background 前面跟上一个 “+”。

这样在 IE 7 以及之前的版本里，就能识别对应的这段代码了。


![](http://upload-images.jianshu.io/upload_images/693359-d746011492c4f6d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


IE 7 浏览器：

![](http://upload-images.jianshu.io/upload_images/693359-0ab761b14f1efff9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##23.后话

怎么样，看过了上面的文章，终于知道为什么我们经常管 IE 6 , IE 7 , IE 8 这三个叫做 “前端三毒” 了吧。

这篇文章前前后后写了三天了，如果你能够看到这段话，那我相信你一定是我的真爱粉了。

爱你么么哒！~ 😘

最后再厚颜无耻一次，求点赞，求分享，求抱抱！~

**转载请注明出处，请尊重作者的劳动成果，谢谢。**

**李先生（李鹏）**

**2016年09月28日**

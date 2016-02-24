之前就接触过伪类，我常常用于清除浮动，但是在实践中很少利用它实现其他效果，今天看公司的简单元件实现，很多用到了伪类，所以就具体的来学习一下：


## ：before ##
：before 选择器是表示在被选元素的内容前面插入内容。然后使用 content 属性来指定要插入的内容。

    <!DOCTYPE html>
	<html>
	<head>
	<style>
	p:before
	{
	content:"我可以做图标";
	background-color:pink;
	color:black;
	font-weight:bold;
	}
	</style>
	</head>
	<body>	
	<p>我是静静。</p>
    
	</body>
	</html>
运行如图：我们发现其实before可以帮助我们做很多事情，比如有特殊样式的标题，或者列表的图标

![](https://github.com/Anjing1993/mypassages/blob/master/images/brfore.png)


## ：after ##
：after 选择器在被选元素的内容后面插入内容。同样使用 content 属性来指定要插入的内容。

    
	<!DOCTYPE html>
	<html lang="en">
	<head>
    <meta charset="utf-8">
    <style>
        p:after
        {
            content:"- 台词";
            background-color:yellow;
            color:red;
            font-weight:bold;
        }
        ul .tt{
            list-style:none;
            position:relative;
            float:left;
            width:44px;
            height:44px;
            background:black;
        }
        .tt:after{
            content:".";
            position:absolute;
            width:10px;
            height:10px;
            left:50%;
            top:50%;
            -webkit-transform: translate(-50%,-50%) rotate(-45deg);
            border:2px solid white;
            border-bottom:0;
            border-right:0;
        }
        .box{
            min-height: 40px;
            float: right;
            position: relative;
        }
        .ipt{
            position:relative;
            height:40px;
        }
        .ipt input{
            width:220px;
            height:40px;
            line-height:40px;
            margin-left:100px;
            -webkit-appearance: none;
            border: 0;
            box-shadow: none;
            padding: 0;
            font-size: 15px;
            background: #d8d8d8;
            text-indent: 10px;

        }
        .ipt:after{
            content: '@vip.163.com';
            position: absolute;
            top: 50%;
            right: 0;
            line-height: 44px;
            margin-top: -22px;
            font-size: 15px;
            color: black;

        }
    </style>
	</head>
	
	<body>
	
	<p>我是静静。</p>
	
	<ul>
	    <li class="tt"></li>
	</ul>
	<div class="box">
	    <div class="ipt">
	        <input type="text"/>
	    </div>
	</div>
	
	
	</body>
	</html>

运行结果如下图：我们发现可以很巧妙地使用after去实现返回按钮，或者自带后缀的输入框，当然还有很多符号都可以利用它们两个画出来，这样我们不再需要定义太多的结构或者用图片啦，瞬间就变成一种优化方式呢！但是一定要注意是给哪个元素加上after喔

![](https://github.com/Anjing1993/mypassages/blob/master/images/after.png)

>注意1 ：对于 IE8 及更早版本中的 :before，：after都必须声明 <!DOCTYPE>。 

>注意2：当然开始也提到过，他们常用于清除浮动：


    .clearfix::before,
	.clearfix::after {
	    content: ".";
	    display: block;
	    height: 0;
	    visibility: hidden;
	}
	.clearfix:after {clear: both;}
	.clearfix {zoom: 1;}



### 额外小记1：画三角形 ##
之前用到类似于三角形的小图标，都会采用图片，然而页面中图片的数量会影响页面的加载速度，当时就了解到了[icon font](http://www.iconfont.cn/collections?personal=1)，要快速使用它：

- 第一步：使用font-face声明字体

		@font-face {font-family: 'iconfont';
		    src: url('iconfont.eot'); /* IE9*/
		    src: url('iconfont.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
		    url('iconfont.woff') format('woff'), /* chrome、firefox */
		    url('iconfont.ttf') format('truetype'), /* chrome、firefox、opera、Safari, Android, iOS 4.2+*/
		    url('iconfont.svg#iconfont') format('svg'); /* iOS 4.1- */
		}

- 第二步：定义使用iconfont的样式

	 .iconfont{
		    font-family:"iconfont" !important;
		    font-size:16px;font-style:normal;
		    -webkit-font-smoothing: antialiased;
		    -webkit-text-stroke-width: 0.2px;
		    -moz-osx-font-smoothing: grayscale;}

- 第三步：挑选相应图标并获取字体编码，应用于页面

        <i class="iconfont">&#x33;</i>
但是今天了解到一种很简便的方法，就是学会巧妙写代码：

	    .triangle{
	            width: 0;                       //占位
			    height: 0;
			    border: 5px solid transparent;  //画
			    border-top-color: #333;         //选择涂色
			    top: 50%;
			    right: 5px;
			    margin-top: -2px;
	        }
实际上就是用边框来实现的三角形效果。而且看完几个元件之后真的要哭了，这些手动实现的效果很赞啊！！！自己之前从来没这么干过，还是思考的太少啦。。。。。。。。。。赶紧赶紧积累下来！！！！！
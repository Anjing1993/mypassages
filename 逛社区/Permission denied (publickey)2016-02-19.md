之前就接触过伪类，我常常用于清除浮动，但是在实践中很少利用它实现其他效果，今天看公司的简单元件实现，很多用到了伪类，所以就具体的来学习一下：


## 额外小记：appearance ##
看源码时看到了这个属性，出于好奇就查了查：
appearance 属性允许您使元素看上去像标准的用户界面元素。所有主流浏览器都不支持 appearance 属性。Firefox 支持替代的 -moz-appearance 属性。Safari 和 Chrome 支持替代的 -webkit-appearance 属性。它用来改变按钮在iPhone下的默认风格，其实我们可以反过来思路，使用“appearance”属性，来改变任何元素的浏览器默认风格，简单的说，你可以使用“appearance”属性将“段落p”渲染成button的风格，也可以渲染成“输入框”、“选择框”等效果，所以还算是很棒的~~~然而它的行为是由它的值决定的：

- normal	将元素呈现为常规元素。
- icon	将元素呈现为图标（小图片）。
- window	将元素呈现为视口。
- button	将元素呈现为按钮。
- menu	将元素呈现为一套供用户选择的选项。
- field	将元素呈现为输入字段。

找到一篇相关的[文章](http://www.w3cplus.com/css3/changing-appearance-of-element-with-css3.html)感觉讲得不错~~~

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

运行结果如下图：我们发现可以很巧妙地使用after去实现返回按钮，或者自带后缀的输入框，这样我们不再需要定义太多的结构，但是一定要注意是给哪个元素加上after

![](https://github.com/Anjing1993/mypassages/blob/master/images/after.png)

>注意1 ：对于 IE8 及更早版本中的 :before，：after都必须声明 <!DOCTYPE>。 
>注意2：当然开始也提到过，他们常用语清楚浮动：

    .clearfix::before,
	.clearfix::after {
	    content: ".";
	    display: block;
	    height: 0;
	    visibility: hidden;
	}
	.clearfix:after {clear: both;}
	.clearfix {zoom: 1;}

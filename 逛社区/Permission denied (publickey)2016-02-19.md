之前就接触过伪类，但是在实践中很少使用，今天看公司的简单元件实现，很多用到了伪类，所以就具体的来学习一下：

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



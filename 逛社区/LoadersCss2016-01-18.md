一睁开眼已经九点多啦~~~罪过啊，那就开始刷刷社区吧，看能不能找到宝，啊哈！如愿以偿，找到了一款相当不错的东西：LoadersCss,一个用CSS 技术实现 loading 动画库。不知道为啥我就是对这种东西比较感兴趣，可能是因为我和它们一样有多动症吧...

那么，让我把链接双手捧上~[此处有宝贝](https://connoratherton.com/loaders)~~~

## LoadersCss ##

它是一个完全用css实现的lodaing动画集。为了避免费时的绘制和css属性布局的计算，每个动画都被限制成css属性的一小部分
## 安装 ##
它的使用还是很简单的，其实只要引入即可，当然也可以使用命令行：

    bower install loaders.css
    npm i --save-dev loaders.css
## 使用 ##

 - 标准   
  - 页面包含 loaders.min.css
  - 创建元素，并且给元素添加相应的类名(例 `<div class="loader-inner ball-pulse"></div>) `
  - 查看例子，插入适当数目的`<div>`
  
 - jQuery（可选）
   - 包含 loaders.min.css, jQuery, and loaders.css.js
   - 创建元素，并且给元素添加相应的类名
   - oaders.js是把每个动画的DIV元素正确数量的一个简单的辅助
   - 页面加载好后去初始化特定div上的动画
## 定制 ##
当然我们可以自己去定制他们，比如修改颜色

    .ball-grid-pulse > div {
      background: orange;
    }
## 浏览器的支持 ##
所有浏览器的最新版本，都是支持它的，并且也支持IE9

IE 11 ✔	Firefox 36 ✔	Chrome 41 ✔	Safari 8 ✔

## 进一步研究 ##



- [http://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/](http://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/)
- [http://aerotwist.com/blog/pixels-are-expensive/](http://aerotwist.com/blog/pixels-are-expensive/)
- [http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)
- [http://frontendbabel.info/articles/webpage-rendering-101/](http://frontendbabel.info/articles/webpage-rendering-101/)

## ps ##

个人觉得其实像这种加载动画，我们的页面一般都需要，但是需求的数量并不多，所以我们可能很多时候没有必要引入整个库，我们可以找到自己想要的那一个，然后抽取出来就OK！

此篇算是学习了解，也是翻译，有什么问题，欢迎随时拍砖哈~~~
			
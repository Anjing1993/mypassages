&ensp;&ensp;前几天一直在git上搭建自己的博客，用了两种方法，一种是gh-pages：这个特别简单，很快就写了两篇，但是又想用jekyll渲染自己的博客，就试了另一个方法：anjing1993.github.io。就倒腾了三天，还是没有弄好，首页的渲染是出来了，戳链接 我的博客，可是文章的同步内有没有弄好，安装相关的bundler总是出问题，心累，还是用gh-pages吧，另一种方遇到的问题过几天再尝试吧~
&ensp;&ensp;这几天一直忙，今天才又开始写自己的博客，但是在git push时遇到了问题：​如图
![](https://raw.githubusercontent.com/Anjing1993/blog/gh-pages/images/flat.png)

&ensp;&ensp;很明显这个与之前遇到的冲突问题是不同的，尝试先去pull代码，再去push，但是还是有问题，查了资料发现出现这个问题的原因是：远程版本里的readme文件和本地版本起了冲突，我突然想起来自己前几天在远程gh-pages的分支上试用了模板，然后readme文件被重写了。。。然后我自己也没有管，然后今天就吃苦了。。。。。最后呢，我总结出了两个解决办法，其实都是强制push，不过分为命令和图形界面:

----------

#####（1）​利用TortoiseGit强制从远程拉取一下code，然后在强制推送一下，就搞定了，如图，点击确定就ok!
![](https://raw.githubusercontent.com/Anjing1993/blog/gh-pages/images/pull.png)
![](https://raw.githubusercontent.com/Anjing1993/blog/gh-pages/images/push.png)

----------

#####（2）然后，就是利用命令行强制处理：
![](https://raw.githubusercontent.com/Anjing1993/blog/gh-pages/images/%E5%91%BD%E4%BB%A4%E8%A1%8C.png)

&ensp;&ensp;问题终于解决了，折腾了快一早上。。。。。肚子咕咕咕叫，我想我该吃饭了。。。​

鸣谢：[http://stackoverflow.com/questions/14609263/geting-an-error-pushing-to-github-updates-were-rejected-because-a-pushed-branc](http://stackoverflow.com/questions/14609263/geting-an-error-pushing-to-github-updates-were-rejected-because-a-pushed-branc)
                                                            
  12/30/2015 4:53:37 PM 
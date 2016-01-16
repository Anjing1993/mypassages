&ensp;&ensp;最近总是有些杂七杂八的事，所以没接着记录。。。今早眼睛刚睁开，就想码代码了，那就开始吧~~~前面我们讲述了用面向对象的思想去优化我们的组件，今天我们又要如何去优化呢？大家有没有发现，我们的代码对边界值其实也是有处理的：![](https://raw.githubusercontent.com/Anjing1993/mypassages/master/images/3.png)

&ensp;&ensp;很明显，当我们输入的内容有一定的长度，就会提示输不进去，那么来看看我们的代码是怎么实现的呢：![](https://raw.githubusercontent.com/Anjing1993/mypassages/master/images/tipe.png)

&ensp;&ensp;一般我们都会利用条件判断去实现，但是如果我们的情况很多，代码很多，那是不是就要写mang个if呢？这时候我们就应该想想办法啦~看了前辈的博客，说是要使用观察者模式，我们知道js最常用的有九种设计模式，那么是什么是观察者模式呢？
###什么是观察者模式
 &ensp;&ensp;在此种模式中，一个目标物件管理所有相依于它的观察者物件，并且在它本身的状态改变时主动发出通知。这通常透过呼叫各观察者所提供的方法来实现。此种模式通常被用来实现事件处理系统。

&ensp;&ensp;也就是说，我们需要观察输入的数字并且去发布通知，另一个人在一旁听我们的通知，发现了边界值，立马就去做些操作，那么就会有一个通知和监听机制，如下：

![](https://raw.githubusercontent.com/Anjing1993/mypassages/master/images/observe1.png)
![](https://raw.githubusercontent.com/Anjing1993/mypassages/master/images//observe2.png)
 &ensp;&ensp;我们就需要像上面这样去处理，多了两个方法fire(),on(),要想所有继承Base的都具有观察这模式，那就需要修改我们的Base了
###观察者机制实现

	//辅组函数，获取数组里某个元素的索引 index
    var _indexOf = function(array,key){
	  if (array === null) return -1
	  var i = 0, length = array.length
	  for (; i < length; i++) if (array[i] === item) return i
	  return -1
	}

	var Event = Class.extend({
 
    //添加监听
  	   
    on:function(key,listener){
    //this.__events存储所有的处理函数
    if (!this.__events) {
      this.__events = {}
    }
    if (!this.__events[key]) {
      this.__events[key] = []
    }
    if (_indexOf(this.__events,listener) === -1 && typeof listener === 'function') {
      this.__events[key].push(listener)
    }

    return this
    },
    //触发一个事件，也就是通知

    fire:function(key){

    if (!this.__events || !this.__events[key]) return

    var args = Array.prototype.slice.call(arguments, 1) || []

    var listeners = this.__events[key]
    var i = 0
    var l = listeners.length

    for (i; i < l; i++) {
      listeners[i].apply(this,args)
    }

    return this
    },
    //取消监听

    off:function(key,listener){

    if (!key && !listener) {
      this.__events = {}
    }
    //不传监听函数，就去掉当前key下面的所有的监听函数
    if (key && !listener) {
      delete this.__events[key]
    }

    if (key && listener) {
      var listeners = this.__events[key]
      var index = _indexOf(listeners, listener)

      (index > -1) && listeners.splice(index, 1)
    }

    return this;
    }
    })
&ensp;&ensp;这个观察者机制实现的思路是使用this._events存下所有的监听函数，在fire的时候找到相应的函数执行就ok，代码来自由网络~有了它的类，我们就可以让Base去继承它，然后让我的需要的类继承Base，然后实例化的东西就有了所有我们需要的方法：init,bind,render,fire,on。那么我们就可以很好地开发组件了

&ensp;&ensp;当然我们还可以继续深挖，比如在数据更新上实现单向绑定，模板渲染，更多我们可以参考前辈牛人的[博客](http://purplebamboo.github.io/2015/03/16/javascript-component/)
本文的学习也来自于这篇文章，特别鸣谢~~~





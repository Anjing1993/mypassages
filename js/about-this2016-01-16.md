&ensp;今天看代码途中发现了this，突然想起来前几个月面试的时候，面试官总是会问或者让写与this相关的代码，刚开始确实答得很不好，也是因为自己理解的不好，后面就很好的理解了，但是一直没有系统的记录一下，那今天就详细的记一下吧。

&ensp;我相信学过js的人都知道this，而且在谁身上调用，this就指向谁。在js中它确实算是一个坑（要不面试官就不会问了），但是很多时候还是很方便的，只要理解了this的工作原理，那么它将不再是一个坑!下面就让我们娓娓道来~~~

## 千变万化的this ##

**1. 全局中的this**

  `console.log(this)` //window

   全局范围内的this是指向全局的，即window

**2. 作为函数调用**

      function test（x）{ 
         this.x = x;
      }
      test(2);
      console.log(x);//这里的函数时定义在全局的，所以这里的this也是指向全局的（注意在严格模式下时undefined）

**3. 作为对象的方法**

      var obj = {
          name : 'wj',
          say : function(sth){
             console.log(this.name+ ' say ' +sth);
          }       
      }
      obj.say('love rose');  //wj love rose
      这里的this是指向obj对象的，也就是当前对象
      

**4. 作为构造函数**
     
      var a =new Test();
      函数内部的this是指向当前新创建的对象的，即是谁new，就指向谁

**5. 作为内部函数**
  
      var name = 'jingjing';
      var obj = {
          name : 'wj',
          say : function(sth){
            var says = function(sth){
                 console.log(this.name+ ' say ' +sth);
             }
            says(sth);
          }
             
                 
      }
      obj.say('love rose'); //jingjing say love rose
      看了输出的内容，很明显这里的this是指向全局的，因为在say函数内部是调用了says的，obj.say已经指向了says的返回值，obj.say()就会立即调用它返回的函数，所以才会识别全局的name，我想这个问题是我们遇到最多的吧，但是只要事先将当前的this存下来，利用that或者self
      var name = 'jingjing';
      var obj = {
          name : 'wj',
          say : function(sth){
            var that = this;  //事先将this存下来
            var says = function(sth){
                 console.log(that.name+ ' say ' +sth);
             }
            says(sth);
          }
             
                 
      }
      obj.say('love rose'); //wj say love rose
     看吧，是不是搞定了~~~
&ensp;虽然this是如此的调皮，但是我们知道人类是一切的主宰，所以我们还是完全可以去随便改变它的~

## 改变this的指向 ##
 
**1. call和apply**

    var name = '王静'；
    var obj ={
        name: '小静静',
        child :{
         name :'小小静',
         getName:function(){
           return this.name;
       }
     }
    }
    obj.child.getName.call();          //王静
    obj.child.getName.call(window);    //王静
    obj.child.getName.call(obj);       //小静静
    obj.child.getName.call(obj.child); //小小静

  看！有了call,我们可以随意改变我们的this指向，是不是很棒呢，同样我们把call改成apply,也是可以的，但是可能有人就会有问题，这两个有什么不同吗？答案是肯定的啦~

    call(string,[arg1,arg2,'''])
    apply(string,[,Array])
    他们两个都是将某个函数绑定带某个特定对象上使用，但是唯一的不同就是他们的第二个参数的区别，apply的第二个参数是数组

**2. 使用bind**

    var windowName = obj.child.getName.bind(window);
    var objName = obj.child.getName.bind(obj);
    windowName();  //王静
    objName();     //小静静
这是ES5提供的，其实bind的作用就是会创建一个新的函数，称为绑定函数。绑定函数会以创建它时传入bind的第一个参数作为this,传入bind的第二个参数以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数~
bind实现的源码如下：
![](https://raw.githubusercontent.com/Anjing1993/mypassages/master/images/bind.png)
它其实也是利用了call和apply的，这下this对我们来说应该不再是坑了吧~~~

     

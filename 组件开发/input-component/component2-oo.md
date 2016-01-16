&ensp;&ensp;我们在上一篇说过我们的页面可能会需要很多组件，那么我们当然希望各个组件的编写方式很类似，所以我们打算使用面向对象的思想。首先，我们的组件可能会有很多方法，但是有些方法是许多组件共有的，比如：

    （1）init--->初始化

    （2）​render--->页面渲染

    （3）bind--->事件绑定​

&ensp;&ensp;我突然想起实习的时候用的那些组件，他们确实是有这些方法唉，当时还有点纳闷，这下有点大彻大悟了。。。。如果大家都这样写，那么以后我们开发大规模的组件库时，相互之间的配合是不是就very nice了啊！所以，我们是不是可以把这些共有的方法抽取成一个基类，然后在这个继承的基础上我们再去开发自己的组件呢？嘿嘿，那么我们就开始吧：
### 一.抽取去基类Base：
    var  Base = Class.extend({

     //自动保存配置项

     init : function(target){

     this._target = target;

     this.bind();

     this.render();

     },

     //设置配置项

     set : function(key, value){

           return this._target[key] = value;

     },

     //获取配置项

     get : function(key){

     return this._target[key];

     },

     //事件绑定

     bind : function(){},

     //渲染

     render : function(){}

     });
&ensp;&ensp;当然，或许还有其他的方法，比如销毁什么的，这里就先不写了。当我们有了暂且完美的基类，接下来快要大功告成啦~~~我们编写自己的组件时，直接继承基类，然后覆盖函数名相同的方法就ok！来吧，跟着我，左手右手一个慢动作.......
### 二.继承Base类：​
var InputCount = Class.extend(Base,{

     //获取目标长度

     _getNum : function(){

     return this.get('input').val().length;

     },

     bind : function(){

     var that = this;

     that.get('input').on('input',function(){

     that.render();

     })

     },

     render : function(){

     var numMax = 20;

     var num =this. _getNum();

            var vInput_after = $('#input-contain-count');

              if(vInput_after.length == 0){

                  this.get('input').after('');

              }

              if(numMax-num < 0|| numMax-num === 0){

                vInput_after.html('还能输入0个字');

              }else{

                 vInput_after.html('还能输入'+(numMax-num)+'个字');

              }

     }

     });

     $(function(){

     new InputCount({

      //因为基类里面有get,set函数，所以这边就直接传input的节点，Base会帮我们自动处理

     input : $('#input-contain')

     });

     })
&ensp;&ensp;大家可以看到，除过一些bind, render我们需要自己处理之外，其他的Base都替我们处理好了，​这是不是特别方便呢。。。容我再大笑几声，啊哈哈哈哈~~~

&ensp;&ensp;是的，我们大功告成了！但是这还是不够规范化，体系化。一个完整的组件，应该有自己处理错误的机制，我们需要对一些意外情况做特殊处理，也就是像单元测试，我们既要做用例测试，又要做边界值测试。该怎么处理这些边界值呢，是不是有点期待呢？下期我们继续~~~
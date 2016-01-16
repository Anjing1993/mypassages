   
&ensp;&ensp;在类的构造器和单元测试告一段落后，接下来的任务是运用面向对象的思想，自由封装一个组件。听到这个词是有些许迷惑的，然后脑子里就奔出了“插件”这个词，组件？插件？这两者到底有什么是非纠葛呢？待我头脑风暴和查询资料后，得知：插件其实就是一些软件的辅助功能的扩展小“附件，而组件是软件的一些功能所必需有的小“附件”，其实在某种程度上说，组件也就是插件的子集。搞清楚这些概念，就要开始行动啦~~~

&ensp;&ensp;我想要封装得功能是一个限定了字数的文本输入框，在输入内容的同时，文本框后面做一些内容边界值的提示；
###一.具体实现：
&ensp;&ensp;首先去分析实现这个功能的步骤都有神马，然后，不假思索~~~一串串面向过程的代码脱颖而出
$(function(){

       //1.获取元素

        var numMax = 20;
        var vInput = $('#input-contain');

       //2.获取字数

        var getNum = function(){
         //val()表示input的value值
            return vInput.val().length;
        }

       //3.渲染元素

        var render = function(){
        var num = getNum();
        var vInput_after = $('#input-contain-count');

        //如果没有数字容器，就创建一个
        if(vInput_after.length == 0){
            vInput.after('');
            console.log(vInput_after);
        }
        if(numMax-num < 0|| numMax-num === 0){
            vInput_after.html('还能输入0个字');
        }else{
            vInput_after.html('还能输入'+(numMax-num)+'个字');
        }
     }

        //4.监听事件

        vInput.on('input',function(){
        render();
       });

        //5.初始化
        render();
      });
 &ensp;&ensp;很快，想要的效果实现了，我想很多初学者都会这样写，但是这样的代码真的是杂乱无章，如果这是活动页面上的，那还可以，但是假如我们的代码很多，很复杂，而且还是几个人合作写的，各种同名变量，同名函数...那我估计大家在合代码的时候要哭死吧。。。那么我们都知道js有作用域这个东西，我们是不是可以利用这个特性给自己的code给打上特意的“标签”，避免与别人的code发生冲突？答案是肯定的！这时候隔离作用域就要出场了~~~也就是利用变量去模拟命名空间
###二.利用命名空间优化
 &ensp;&ensp;然而什么是命名空间呢？我觉得它就像是java里面的包，它有自己的名字，它将我们需要的东西包在里面，需要的时候，引入这个包，通过这个包去使用包里面的东西，这是不是很友好呢？那么命名空间具体该怎么去使用呢？如图1和图2：就是起一个名字，把自己的东西包起来喔
![](https://raw.githubusercontent.com/Anjing1993/mypassages/master/images/1.png)
![](https://raw.githubusercontent.com/Anjing1993/mypassages/master/images/2.png)

 &ensp;&ensp;说了这么多，还是看看在我们的code具体如何实现吧...
	
	//2.使用命名空间​

    var inputCount = {

      numMax : 20,
      num : 0, 
      vInput : null,
      vInput_after : null,

      init : function(targat){
              this.vInput = $(targat.id);
              this.bind();
              return this;
      },
      bind : function(){
      //这里的this作为了对象中方法的内部函数，它是指向全局的，所以我们事先应该存下来
      
关于this的详细讲解，请[戳链接](https://github.com/Anjing1993/mypassages/blob/master/js/about-this.md)

              var _this = this;
              this.vInput.on('input',function(){
                   _this.render();
           });
      },
      getNum : function(){
              return this.vInput.val().length;
      },
      render : function(){
              this.num = this.getNum();
              this.vInput_after = $('#input-contain-count');
              if(this.vInput_after.length == 0){
                  this.vInput.after('');
              }
              //边界值处理
              if(this.numMax-this.num < 0|| this.numMax-this.num === 0){
                 this.vInput_after.html('还能输入0个字');
              }else{
                 this.vInput_after.html('还能输入'+(this.numMax-this.num)+'个字');
              }
	        }	
	}
	
	$(function(){
	
	  inputCount.init({
	
	    id : '#input-contain'
	
	  }).render();
	
	})
 &ensp;&ensp; 这样一改造，立马变的清晰了很多，所有的功能都在一个变量inputCount下面。我们使用它里面的方法，可直接通过它去调用，这样我们就不怕和别人出现发生代码冲突了，但是这种写法没有私有的概念，比如上面的getNum,bind应该都是私有的方法。但是其他代码可以很随意的改动这些。当代码量特别特别多的时候，很容易出现变量重复，或被修改的问题，于是闭包就派上用场了，至于闭包这里就不用在详细解释了吧：
### 三.利用闭包优化
  //3.让方法私有,getName, bind应该是私有的方法,私有方法一般以_开头

    var inputCount = (function(){

    var vInput_after = null;

        var numMax = 20;

    var _bind =function(that){

             that.vInput.on('input',function(){

             that.render();

             });

    }

        var _getNum =function(that){

             return that.vInput.val().length;

        }

        var inputCountFunc = function(target){}

        inputCountFunc.prototype.init = function(target){

         this.vInput = $(target.id);

              _bind(this);

              return this;

        }

        inputCountFunc.prototype.render = function(){

              var num = _getNum(this);

              this.vInput_after = $('#input-contain-count');

              if(this.vInput_after.length == 0){

                  this.vInput.after('');

              }

              if(numMax-num < 0|| numMax-num === 0){

                 this.vInput_after.html('还能输入0个字');

              }else{

                 this.vInput_after.html('还能输入'+(numMax-num)+'个字');

              }

        }

        return inputCountFunc;

    })();

    $(function(){

    new inputCount().init({

          id : '#input-contain'

    }).render();

    });
&ensp;&ensp; 这种写法，把所有的东西都包在了一个自动执行的闭包里面，所以不会受到外面的影响，并且只对外公开了TextCountFun构造函数，生成的对象只能访问到init,render方法。这种写法已经满足绝大多数的需求了。事实上大部分的jQuery插件都是这种写法哦。

&ensp;&ensp; 我想这种写法已经可以满足大多数人的需求了，但是我觉得写一个组件这样还好，要是我们需要很多组件，要是都这样写，那是不是一万点伤害啊？？？？​我们既需要代码好维护，又需要大家不约而同写出来的code都比较相近，那我觉得最好的方式就是“面向对象了”，这样的话我们可能会需要一个类，刚好我们前面实现了一个类的构造器唉，那是不是接下来就要事半功倍了，容我大笑几下，啊哈哈哈~~~

&ensp;&ensp;  那么下期，再让我细细道来其中细节哈~​​
​​
js精粹对js中的方法做了一个大的整合，很多方法也是比较常用的，下面就列举一些常用的：
## 数组： ##
### concat方法： ###
这个方法是数组和字符串共有的，实现数组或字符串的连接，返回一个新数组或字符串，是一种浅复制，也就是说只会复制数组的值，并不会复制它上面的方法等。

    //典型应用：用数组的方法实现数组复制
    arryobj.concat();  //返回一个新数组

### join方法： ###
将数组中的每个元素构造成一个字符串，并用特定分隔符分割字符串：

	var arr = [1,2,3];
	arr.join('');      //"123"
	arr.join('-');     //"1-2-3"

### push，pop方法： ###
push用于向数组添加一个或多个元素，并返回新的长度，而pop则相反，用于删除并返回数组的最后一个元素：

    var arr = [1,2,3];
    arr.push(5);       //4
    arr.pop();         //5
这种方法是很常用的，并且也是其他编程通用的，在这里可以看一下push怎么去实现：

	Array.method('push',function(){
	   this.splice.apply(
	     this,
	     [this.length,0].concat(Array.prototype.slice.apply(arguments)));
	     return this.length;
	});
实质上也是利用了concat方法去连接。

### shift，unshift方法： ###
这两个方法刚好与前面的方法相反，shift用于删除数组的第一个元素，并返回它的值,如果数组是空的，将会返回undefined，而且通常会比pop慢很多；unshift用于在数组开头添加一个或多个元素，并返回新的长度

    var arr = [1,2,3];
    arr.unshift(5);      //4
    arr.shift();         //5
    
    Array.mothod('shift',function(){
          return this.splice[0，1][0];   //在0位置，删除一个元素
    });
### reverse方法： ###
这个方法会反转数组中元素的顺序，然后返回当前数组：

    var a =[4,1,7];
    a.reverse();        //[7,1,4]

### sort方法： ###
这个方法是对数组的内容进行一定的排序，但很多时候结果并不是我们想要的：

    var n = [18,19,0,2,5,6,56];
    n.sort();           //[0, 18, 19, 2, 5, 56, 6]
这时候我们就需要做一定的处理：

    var n = [18,19,0,2,5,6,56];
    n.sort(function(a,b){
      return a-b;
    });                 //[0, 2, 5, 6, 18, 19, 56]
然而sort方法是不稳定的，不能保证产生正确的序列，就拿这个例子来说吧：

    <!DOCTYPE html>
    <html lang="en">
    <head>
	<meta charset="UTF-8">
	<title>对象扁平化</title>
	<script type="text/javascript">
	    //对象扁平化：总分从小到大排序，总分一样的按照yw成绩排序
		var arr=[{
						  name:'张三',
						  yw: 4,
						  sx: 5
						},{
						  name:'李四',
						  yw: 5,
						  sx: 5
						},{
						  name:'王五',
						  yw: 6,
						  sx: 6
						},{
						  name:'赵六',
						  yw: 4,
						  sx: 6
						},{
						  name:'马七',
						  yw: 5,
						  sx: 4
						}];
    //方法一：直接排序，但是优先级规则变动，代码就变动大，不通用
	//      console.log(arr.sort(function(a,b){
	//     if(a.yw + a.sx < b.yw+b.sx){
	//       return -1;
	//     } else if(a.yw + a.sx === b.yw+b.sx){
	//       if( a.sx < b.sx){
	//         return -1
	//       } else{
	//         return 1;
	//       }
	//     } else if(a.yw + a.sx > b.yw+b.sx){
	//       return 1
	//     }
	    
	// }))
 
    //方法二：规定优先级
	function compare(a,b){
	  return a - b;
	}
	//这个是优先级
	var sortOrder = ['yw','sx','hx','sw'];
	console.log(arr.sort(function(a,b){
	    var sumA = 0, sumB = 0;
	    for(var i = 0; i < sortOrder.length; i++) {
	      //console.log(a[sortOrder[i]])
	      if (typeof a[sortOrder[i]] !== 'undefined' ){
	        sumA += a[sortOrder[i]];
	        sumB += b[sortOrder[i]];
	      }
	      
	    }
	    
	    var j = 0;
	    var result = compare(sumA,sumB);
	    if(result !== 0 ){
	      return result;
	    }
	    do{
	      result = compare(a[sortOrder[j]], b[sortOrder[j]]);
	      j++;
	      //0表示分数一样，继续下一个优先级
	    }while(j < sortOrder.length && result )
	    return result;
	}));
如果用方法一去实现，这样就无法保证排序前与排序后相等值的相对位置一致，但是使用方法二做更多的处理，就可以保证。

### slice方法： ###
表示从已有的数组中返回选定的元素，从start到end，但是不含end

### splice方法： ###
从数组中添加或删除特定元素，然后返回被删除的元素：

    var arr = [1,3,4];
    arr.splice(2,0,"a");
    arr.splice(1,1);    //3    在1号位置，删除一个元素
    arr.splice(1,0,2);  //[]

## 函数 ##
### apply方法： ###
apply方法调用一个函数，传递一个将被绑定到this上的对象和一个可选的参数集合

而call, apply都属于Function.prototype的一个方法,它是JavaScript引擎内在实现的,因为属于Function.prototype,所以每个Function对象实例,也就是每个方法都有call, apply属性.既然作为方法的属性,那它们的使用就当然是针对方法的了.这两个方法是容易混淆的,因为它们的作用一样,只是使用方式不同.

- 相同点:两个方法产生的作用是完全一样的
- 不同点:方法传递的参数不同
 - 那什么是方法产生的作用,方法传递的参数是什么呢?
     - call, apply方法区别是,从第二个参数起, call方法参数将依次传递给借用的方法作参数, 而apply直接将这些参数放到一个数组中再传递, 最后借用方法的参数列表是一样的.
     - 当参数明确时可用call, 当参数不明确时可用apply给合arguments
  
## Number方法： ##
### numder.toExponential(arguments)： ###
表示将number转化为一个指数形式的字符串，参数用来规定小数点后面的数字位数，必须在0到20之间。

### numder.toFixed(arguments)： ###
把number转化为一个十进制数形式的字符串，参数可选，也表示小数点后面的数字位数,必须在0到20之间

### numder.toPrecision(arguments)： ###
把number转化为一个十进制数形式的字符串，参数可选，也表示小数点后面的数字位数,必须在0到21之间

### numder.toString(arguments)： ###
把number转化为一个字符串，可选参数确定基数

## Object方法： ##
hasOwnProperty(name):
我们通常用它来判断方法或数学存在于本地还是原型中，而且一旦原型链和本地种都有某个属性，那么原型链中的将不会被它检查到

## RegExp方法： ##
关于正则是在前一张刚刚说过的：

### exec(string)： ###
exec的返回值是数组，此数组的第 0 个元素是与正则表达式相匹配的文本，第 1 个元素是与 RegExpObject 的第 1 个子表达式相匹配的文本（如果有的话），第 2 个元素是与 RegExpObject 的第 2 个子表达式相匹配的文本（如果有的话），以此类推。除了数组元素和 length 属性之外，exec() 方法还返回两个属性。index 属性声明的是匹配文本的第一个字符的位置
### test(string)： ###

在字符串中查找是否有满足正则表达式的值，返回布尔值

## String方法： ##
### indexOf(x)： ###
返回字符串中子串第一次出现的索引，无匹配则返回-1：

    var str = "abcdfr";
    str.indexOf(2);        //-1
    str.indexOf("cd");     //2

### charAt(x)： ###
返回指定位置的字符

    var str = "abcdfr";
    str.charAt("3");       //d
    str.indexOf("11");     //""

### strsub(x,y)： ###
返回子串，传入的参数是起始位置和长度

### slice(x)： ###
提取字符串的一部分，不包含end：

    var s = "hello";
    s.slice(1,4);         //ell

### split(x)： ###
将字符串分割成数组：

    var s = "wangjing";
    s.split("");          //["w", "a", "n", "g", "j", "i", "n", "g"]

### search(x)： ###
执行一个正则匹配查找，成功返回索引，否则返回-1

### replace(x)： ###
查找匹配一个正则表达式的字符串，然后使用新的字符串代替匹配的字符串

    var s = a.replace(re, "haha");    //haha

### toLowerCase(x)： ###
字符串变小写：

   `"WANGJING".toLowerCase();          //wangjing`
### toUpperCase(x)： ###
字符串字母大写：

    "wangjing".toLowerCase();          //WANGJING
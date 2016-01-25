    var d= null;
    console.log(typeof d);    //object 
    console.log(Object.prototype.toString.call(d));  //object null
我想大家会对这里有疑问，其实从逻辑的角度，null值表示一个空对象，所以typeof后的值才会是一个object。这是前几天在js高程的数据类型那块的理解，今天看前辈阮一峰的微博有重大发现：

实质上，1995年JS诞生时，根本没把null当作数据类型，而是Object的一种特殊值。下面是当年C源码，其中完全没考虑null。这就是typeof null === object的根本原因。

![](https://raw.githubusercontent.com/Anjing1993/mypassages/master/images/null.jpg)

[文章链接](http://www.2ality.com/2013/10/typeof-null.html)

可是真的是这样末？我也开始疑惑了......

----------

> 引自MDN：

    typeof null === 'object';

In the first implementation of JavaScript, JavaScript values were represented as a type tag and a value. The type tag for objects was 0. null was represented as the NULL pointer (0x00 in most platforms). Consequently, null had 0 as type tag, hence the bogus typeof return value.

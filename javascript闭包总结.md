---
title: javascript闭包总结
date: 2017-07-18 09:02:40
categories: 前端
tags: [javascript]
---
<Excerpt in index | 首页摘要> 
javascript闭包总结
<!-- more -->
<The rest of contents | 余下全文>

-----

### 1.作用域
#### 1.1全局变量
函数内部可以读取全局变量
```javascript
//这里声明的x是全局变量
var x=1;
function aa(){
    //这里调用全局变量里面的x
    console.log(x);
}
aa();
```

#### 1.2 局部变量
位于某个函数内部的变量是局部变量,在这个函数外部无法读取这个变量
```javascript
function aa(){
var y=2;
}
//调用函数aa内部的局部变量y会报错
console.log(y);
// y is not defined
```

#### 1.3 不加var 声明变量
函数内部不加var声明变量，会导致此变量变为全局变量！
```javascript
function aa(){
    y=2;
}
//注意的先运行一下函数aa
aa();
//调用函数aa内部的局部变量y会报错
console.log(y);//2
```

#### 1.4 如何在外部读取局部变量？
- 链式作用域结构
```javascript
　　function f1(){
　　　　n=999;
　　　　function f2(){
　　　　　　alert(n); // 999
　　　　}
　　}
```
函数f2就被包括在函数f1内部，这时f1内部的所有局部变量，对f2都是可见的。但是反过来就不行，f2内部的局部变量，对f1 就是不可见的。这就是Javascript语言特有的“链式作用域”结构（chain scope），
子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。

- 实现外部读取局部变量 
在函数内部在定义一个函数
```javascript
function f1(){
    var x=1;
    return (function (){
        return x;
    })();
}
console.log(f1());//1

//上面写的有点复杂，简单点
function f3(){
    var y=3;
    function f4(){
        return y;
    }
    return f4();
}
console.log(f3());//3
```
### 2. 闭包的概念
#### 2.1
闭包就是能够读取其他函数内部变量的函数.由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

### 3. 闭包的用途
- 读取函数内部的变量

- 让这些变量始终保存在内存中

#### 3.1
```javascript
    function f1(){
        var n=999;
        //下面的ndd没有加var,所以他是全局变量，后面调用不会出错，如果加
        //上，就成了f1的私有变量，后面调用会出错
        ndd=function(){
            n+=1;
        }
        function f2(){
            alert(n);
        }
        return f2;
    }
    var result=f1();
    result();//999
    ndd();
    result();//1000
```
分析：
在上面这段代码中，result其实就是f2,他一共运行了两次，第一个是999，第二次是1000。这说明函数f1里面的局部变量n,一直保存在内存中，并没有在f1调用后立即被删除。

为什么会这样呢？原因就在于f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。


这段代码中另一个值得注意的地方，就是“nAdd=function(){n+=1}”这一行，首先在nAdd前面没有使用var关键字，因此 nAdd是一个全局变量，而不是局部变量。其次，nAdd的值是一个匿名函数（anonymous function），而这个
匿名函数本身也是一个闭包，所以nAdd相当于是一个setter，可以在函数外部对函数内部的局部变量进行操作。

#### 3.2

### 4.使用闭包的注意点
- 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

- 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便
改变父函数内部变量的值。

### 5.闭包实例
#### 5.匿名函数闭包(经典！)
这个在js高级程序设计P182有
**匿名函数的调用环境是window！！！！**
```javascript
var name = "The Window";
var object = {
    name:"my objcet",
    getName:function(){
        return function(){
            return this.name;
        }
    }
};
console.log(object.getName()());
//结果是The Window
```
为什么是The Window而不是my object??
将上面改为：
```javascript
var name = "The Window";
var object = {
    name:"my objcet",
    getName:function(){
        return function(){
            return this.name;
        }
    }
};
var x=object.getName();
console.log(x());
//结果是The Window
```
object.getName()返回的是一个匿名函数，匿名函数的执行环境是window。在第二例中可以输出一下window.x！

补充其他人的说法
[点我](http://blog.csdn.net/u013250416/article/details/40869287)

 在一般情况下，this对象时在运行时基于函数的执行环境绑定的：在全局函数中，this等于window，而当函数被作为某个对象的方法调用时，this等于那个对象。但是，匿名函数的执行环境具有全局性，因此它的this对象通常指向windows.
 ```javascript
 var name = "The Window";  
var object = {  
name : "My Object",  
getNameFunc : function(){  
return function(){  
return this.name;  
        };  
    }  
};  
alert(object.getNameFunc()()); //"The Window"（在非严格模式下）  
```
因为在getNameFunc（）函数中，返回的是一个匿名函数，而在匿名函数中的return this.name语句中，this指的是全局环境中的window，因而结果为"The Window"那么，如果我们想要使得匿名函数中的this，指向的是当前的Object对象，应该如何处理呢。可以在匿名函数外声明一个变量,将this赋给变量，因为在匿名函数外的this就等于该对象。代码如下：

```javascript
var name = "The Window";  
var object = {  
name : "My Object",  
getNameFunc : function(){  
var that=this;  
return function(){  
    return that.name;  
        };  
    }  
};  
alert(object.getNameFunc()());//The Object 
```

这样就达到了我们的目的了，结果为My Object。当然了，上文在很大程度上，是为了说明，匿名函数中this的特殊性，如果只是想得到对象中变量的值，最简单的一种方法就是：
```javascript
var name = "The Window";  
var object = {  
name : "My Object",  
getNameFunc :function(){  
    return this.name;  
    }  
};  
alert(object.getNameFunc());//The Object  
```
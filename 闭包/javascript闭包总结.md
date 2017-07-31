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
[老外的讨论](https://stackoverflow.com/questions/111102/how-do-javascript-closures-work)
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
#### 2.2
闭包，官方对闭包的解释是：一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分。闭包的特点：

　　1. 作为一个函数变量的一个引用，当函数返回时，其处于激活状态。

　　2. 一个闭包就是当一个函数返回时，一个没有释放资源的栈区。
　　简单的说，Javascript允许使用内部函数---即函数定义和函数表达式位于另一个函数的函数体内。而且，这些内部函数可以访问它们所在的外部函数中声明的所有局部变量、参数和声明的其他内部函数。当其中一个这样的内部函数在包含它们的外部函数之外被调用时，就会形成闭包。
### 3. 闭包的用途
- 读取函数内部的变量

- 让这些变量始终保存在内存中

- 使用闭包代替全局变量

- 函数外或在其他函数中访问某一函数内部的参数

- 在函数执行之前为要执行的函数提供具体参数

- 在函数执行之前为函数提供只有在函数执行或引用时才能知道的具体参数

- 为节点循环绑定click事件，在事件函数中使用当次循环的值或节点，而不是最后一次循环的值或节点

- 暂停执行

- 包装相关功能

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

#### 3.2 使用闭包代替全局变量
[参考这里](http://www.cnblogs.com/star-studio/archive/2011/06/22/2086493.html)
```javascript
//test1是全局变量
var test1=111;
function outer(){
    alert(test1);
}
outer();//111
alert(test1);//111


//闭包，test2是局部变量，这是闭包的目的
//我们经常在小范围内使用全局变量，这时候就可以使用b包来代替
(function(){
    var test2 = 222;
    function outer(){
        alert(test2);
    }
    function test(){
        alert("测试闭包:"+test2);
    }
    outer();//222
    test();//测试闭包：222
})()
alert(test2);//未定义，这里就访问不到test2
```
#### 3.3 函数外或在其他函数中访问某一函数内部的参数
为了解决在Ajax callback回调函数中经常需要继续使用主调函数的某一些参数。
```javascript
function f1(){
    var test=111;
    tmp_test=function(){return test;} //tmp_test是全局变量,这里对test的引用，产生闭包
}

function f2(){
    alert("测试一："+tmp_test());
    var test1=tmp_test();
    alert("测试二："+test1);
}
f1();
f2();
//测试一：111
//测试二：111
alert(tmp_test()); //111
//销毁
tmp_test=null;
```

#### 3.4在函数执行之前为要执行的函数提供具体参数
某些情况下，是无法为要执行的函数提供参数，只能在函数执行之前，提前提供参数。

有哪些情况是延迟执行？

如：setTimeOut 

     setInterval

     Ajax callbacks

     event handler[el.onclick=func 、 el.attachEvent("onclick",func)]


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="hello">测试</div>
</body>
<script>
    //无法传参的情况
    var parm = 222;
    function f1(){
        alert(111);
    }
    function f2(obj){
        alert(obj);
    }
    //setTimeout(f1,500);//正确无参数111
    //var test1=f2(parm);//执行以下f2函数 222

    //setTimeout(f2,1000);//undefined 传参失败
    //setTimeout(f2(parm),5000);//可以弹出222，但是定时器失效

    // setTimeout(function(parm){alert(parm)},3000);//undefined，传参失败 
    //document.getElementById("hello").onclick=f1; //正确111
    //document.getElementById("hello").addEventListener("click",f1,false)//111

    //正确做法，使用闭包
    function f3(obj){
        return function(){
            alert(obj)
            }
        }
    var test2=f3(parm);//返回f3的内部函数的引用
    setTimeout(test2,500);//正确,222
    document.getElementById("hello").onclick=test2;//正确,222
    document.getElementById("hello").attachEvent("onclick",test2);//正确,222
</script>
</html>
```
#### 3.5 在函数执行之前为函数提供只有在函数执行或引用时才能知道的具体参数 
没看懂
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <a href="">你好1</a>
    <a href="">你好2</a>
    <a href="">你好3</a>
    <a href="">你好4</a>
    <a href="">你好5</a>
</body>
<script>
    //动态绑定a集合的注册点击事件，在事件处理函数test1中提供参数-该点击事件的a元素本身。
var aa="ha!";
function test(obj){
    return function(){
        alert(obj);
    }
}
var nodes=document.getElementsByTagName("a");
for(var i=0;i<nodes.length;i++){
     var test1=test(aa);//由于是提前提供参数，只能提供已知的具体参数，无法事先得知点击事件的a元素本身。
                //这里想提供点击事件的a元素本身作为参数宣告失败！
    nodes[i].onclick=test1;//只有在注册点击事件时，才会知道该点击事件的a元素是哪个
} 


//以下是解决方式
function associateObjWithEvent(obj,methodName){
    return (function(e){
        e=e||window.event;
        return obj[methodName](e,this);//重点看这里！有两个参数，
        //e:event，元素绑定事件时，绑定的是对内部函数的引用，故在触发事件时，执行的是内部函数。
        //内部函数有个e参数，刚好在事件触发时，能捕获到是什么事件类型。
        //this:这里需要的就是this参数，以便当元素触发事件处理函数执行时，this=触发事件的元素本身
        //this参数无法从外部传入进来。传入进来的this都会被转化特定对象   
        } 
    );
} 

function DhtmlObject(elId){
    var el=document.getElementById(elId);
    if(el){
        //el.onclick=associateObjWithEvent(this,"doOnClick");
        el.onmouseover=associateObjWithEvent(this,"doMouseOver");
        el.onmouseout=associateObjWithEvent(this,"doMouseOut");
    }
}
DhtmlObject.prototype.doMouseOver=function(event,element){
    alert(event);//第一个参数，只在事件执行时，才知道是什么事件，这里是MouseEvent
    alert(arguments[0]);//第一参数，
    alert(element);//第二个参数，只在事件执行时，才知道是指代触发事件的元素本身
    alert(arguments[1]);//第二个参数
}

var hello=new DhtmlObject("hello");  //执行
</script>
</html>
```

没看懂
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<div id="hello">你好</div>
</body>
<script>
function associateObjWithEvent(obj,methodName){
    return (function(e){
        e=e||window.event;
        return obj[methodName](e);
    });
}

function DragListener(){
    this.down=function(){
        alert(this)
        alert(arguments[0])
    },
    this.move=function(){
        alert(2)
    }
}

var obj=new DragListener();

document.getElementById("hello").onmousedown =obj.down;//正确 但我们在方法中用this访问到的对象是 dom
document.getElementById("hello").onmousemove = obj.move;//正确

document.getElementById("hello").onmousedown =associateObjWithEvent(obj,'down');//正确
document.getElementById("hello").onmousemove = associateObjWithEvent(obj,'move');//正确
</script>
</html>
```
改进的例子，无限参数
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<div id="hello">你好</div>
</body>
<script>
    function associateObjWithEvent(obj, methodName){
    slice = Array.prototype.slice;
    var args=slice.call(arguments,2);//从第三个参数开始赋值
    return (function(e){       
        e = e||window.event;
        var array = slice.call(arguments, 0);
        array.push(e); //第一个参数为event
        return obj[methodName].apply(this,array.concat(args));//第二个参数，依次...
    });
}


function DhtmlObject(elementId){   
    var el = document.getElementById(elementId); 
    if(el){
        //el.onclick = associateObjWithEvent(this, "doOnClick");
        el.onmouseover = associateObjWithEvent(this, "doMouseOver","hello2","aaa",event);//第一个参数为event，hello2是第二个参数，
        //因为event只有在事件处理函数执行时才会知道是具体什么事件类型，无法通过传参提供，传参的event只能识别为null
    }
}

DhtmlObject.prototype.doMouseOver = function(event){
    // doMouseOver 方法体。
    alert("this:"+this.id);//this:hello
    alert("arg0:"+arguments[0]) ;//arg0:MouseEvent
    alert("arg1:"+arguments[1]);//arg1:hello2
    alert("arg2:"+arguments[2]);//arg2:aaa
    alert("arg3-event:"+arguments[3]);//arg3-event:null
    alert("event:"+event);//event:MouseEvent
}
//DhtmlObject("hello"); 这样写，反而出错。因为是类的写法，必须实例化才会执行。
var hello=new DhtmlObject("hello")
</script>
</html>
```

#### 3.6为节点循环绑定click事件，在事件函数中使用当次循环的值或节点，而不是最后一次循环的值或节点
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<a href="hello">hello1</a>
<a href="world">hello2</a>
</body>
<script>
    //假设有两个a链接，id分别为"hello"、"world"
function test(obj){ 
    return function(){
        alert(obj.id);
    }
}
var nodes=document.getElementsByTagName("a");
for(var i=0;i<nodes.length;i++){
    var node=nodes[i]
    nodes[i].attachEvent("onclick", function(){alert(node.id)})//点击链接时，hello链接弹出world，world链接也弹出world；
                                                                  //这是因为循环完毕后，node被赋值为world元素
                                                                  //这不是我们预期的结果！！！
        
    //正确写法一   
   // 内部参数parm为任意指定的参数,如：(function(node){return function(){alert(node.id)}})(node)                                                         
    nodes[i].attachEvent("onclick",(function(parm){return function(){alert(parm.id)}})(node))
    //第一次循环创建了一个闭包，缓存的node参数为hello链接。
    //第二次循环又创建了一个闭包，缓存的node参数为world链接。
    //点击链接时，hello链接弹出hello，world链接弹出world，因为他们调用的是各自的node参数
    
    //正确写法二
    var func=test(node);
    nodes[i].attachEvent("onclick",func)
}
</script>
</html>
```

#### 3.7暂停执行
[参考](http://ljchow.cnblogs.com)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<input type="button" value="继续" onclick='st();'/> 
<script type="text/javascript">
    var st = (function () {
        alert(1);
        alert(2);
        return function () {
            alert(3);
            alert(4);
        }
    })(); 
</script>
</body>

</html>
```
把这个作用延伸下，我想到了用他来实现window.confirm。
```html
<html xmlns="http://www.w3.org/1999/xhtml">

<head>
    <title></title>
    <script type="text/javascript">
        var $ = function (id) { return "string" == typeof id ? document.getElementById(id) : id; }
        var doConfirm = function (divId) {
            $(divId).style.display = "";
            function closeDiv() { $(divId).style.display = "none"; }
            return function (isOk) {
                if (isOk) { alert("Do deleting..."); }
                closeDiv();
            }
        }

    </script>
    <style type="text/css">
        body {
            font-family: Arial;
            font-size: 13px;
            background-color: #FFFFFF;
        }

        #confirmDiv {
            width: 200px;
            height: 100px;
            border: dashed 1px black;
            position: absolute;
            left: 200px;
            top: 150px;
        }
    </style>
</head>

<body>
    <div>
        <input name="btn2" type="button" value="删除" onclick="doConfirm('confirmDiv');" />
        <div id="confirmDiv" style="display: none;">
            <div style='position: absolute; left: 50px; top: 15px;'>
                <p>
                    你确定要删除吗?</p>
                <input type="button" value="确定" onclick="doConfirm('confirmDiv')(true);" />
                <input type="button" value="取消" onclick="doConfirm('confirmDiv')(false);" />
            </div>
        </div>
    </div>
</body>

</html>
```

#### 3.8包装相关功能
下面的代码定义了一个函数，这个函数用于返回一个 HTML 字符串，其中大部分内容都是常量，但这些常量字符序列中需要穿插一些可变的信息，而可变的信息由调用函数时传递的参数提供。


通过执行单行函数表达式返回一个内部函数，并将返回的函数赋给一个全局变量，因此这个函数也可以称为全局函数。而缓冲数组被定义为外部函数表达式的一个局部变量。它不会暴露在全局命名空间中，而且无论什么时候调用依赖它的函数都不需要重新创建这个数组。


如果一个函数依赖于另一（或多）个其他函数，而这些其他函数又没有必要被其他代码直接调用，那么可以运用相同的技术来包装这些函数，而通过一个公开暴露的函数来调用它们。这样，就将一个复杂的多函数处理过程封装成了一个具有移植性的代码单元。
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>

</body>
<script>
    /* 声明一个全局变量 - getImgInPositionedDivHtml - 
并将一次调用一个外部函数表达式返回的内部函数赋给它。

   这个内部函数会返回一个用于表示绝对定位的 DIV 元素
   包围着一个 IMG 元素 的 HTML 字符串，这样一来，
   所有可变的属性值都由调用该函数时的参数提供：
*/
    var getImgInPositionedDivHtml = (function () {
        /* 外部函数表达式的局部变量 - buffAr - 保存着缓冲数组。
        这个数组只会被创建一次，生成的数组实例对内部函数而言永远是可用的
        因此，可供每次调用这个内部函数时使用。
    
        其中的空字符串用作数据占位符，相应的数据
        将由内部函数插入到这个数组中：
        */
        var buffAr = [
            '<div id="',
            '',   //index 1, DIV ID 属性
            '" style="position:absolute;top:',
            '',   //index 3, DIV 顶部位置
            'px;left:',
            '',   //index 5, DIV 左端位置
            'px;width:',
            '',   //index 7, DIV 宽度
            'px;height:',
            '',   //index 9, DIV 高度
            'px;overflow:hidden;\"><img src=\"',
            '',   //index 11, IMG URL
            '\" width=\"',
            '',   //index 13, IMG 宽度
            '\" height=\"',
            '',   //index 15, IMG 调蓄
            '\" alt=\"',
            '',   //index 17, IMG alt 文本内容
            '\"><\/div>'
        ];
        /* 返回作为对函数表达式求值后结果的内部函数对象。
        这个内部函数就是每次调用执行的函数
        - getImgInPositionedDivHtml( ... ) -
        */
        return (function (url, id, width, height, top, left, altText) {
            /* 将不同的参数插入到缓冲数组相应的位置：
            */
            buffAr[1] = id;
            buffAr[3] = top;
            buffAr[5] = left;
            buffAr[13] = (buffAr[7] = width);
            buffAr[15] = (buffAr[9] = height);
            buffAr[11] = url;
            buffAr[17] = altText;
            /* 返回通过使用空字符串（相当于将数组元素连接起来）
            连接数组每个元素后形成的字符串：
            */
            return buffAr.join('');
        }); //:内部函数表达式结束。
    })();
    /*^^- :单行外部函数表达式。*/
    //输出HTML字符串
    document.write(getImgInPositionedDivHtml("www.baidu.com", "hello", 200, 100, 100, 40, "hello"))

</script>

</html>
```
### 4.使用闭包的注意点
- 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

- 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便
改变父函数内部变量的值。

### 5.闭包实例
#### 5.1 匿名函数闭包(经典！)
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

#### 5.2
```javascript
function outerFun(){
    var a=0;
    function innerFun(){
        a++;
        alert(a);
    }
}
innerFun();
// 报错，innerFun is not defined
```
上面的代码是错误的.innerFun()的作用域在outerFun()内部,所在outerFun()外部调用它是错误的.改成如下,也就是闭包:

```javascript
function outerFun(){
    var a=0;
    function innerFun(){
        a++;
        alert(a);
    }
    return innerFun;//注意这里
}
var x=outerFun();
x();//1
x();//2
//innerFun内部函数在定义他的作用域外部引用,就创建了该内部函数的闭包
//如果内部函数引用了位于外部函数的变量.当外部函数调用完毕以后.这些
//变量在内存中不会被释放.因为闭包需要他们。
```
innerFun内部函数在定义他的作用域外部引用,就创建了该内部函数的闭包如果内部函数引用了位于外部函数的变量.当外部函数调用完毕以后.这些变量在内存中不会被释放.因为闭包需要他们。

#### 5.3
```javascript
function outerFun(){
    var a=0;
    alert(a);
}
var a=4;
outerFun();//0
alert(a);//4
```

改一下
```javascript
function outerFun(){
    a=0;//没有var关键字
    alert(a);
}
var a=4;
outerFun();//0
alert(a);//0
```
为什么都是0？？？？**重点**
作用域链是描述一种路径的术语,沿着该路径可以确定变量的值 .当执行a=0时,因为没有使用var关键字,因此赋值操作会沿着作用域链到var a=4;  并改变其值.

#### 5.4
```javascript
function createFunctions(){
    var result=new Array();
    for(var i=0;i<10;i++){
        //脚标0-9都是匿名函数
        result[i]=function(){
            return i;
        };
    }
    //result是一个数组，且脚标0-9都是匿名函数
    return result;
}
var funcs=createFunctions();//他就是result
for(var i=0;i<funcs.length;i++){
    console.log(funcs[i]());
}
```
乍一看，以为输出 0~9 ，万万没想到输出10个10？
这里的陷阱就是：函数带()才是执行函数！ 

单纯的一句 var f = function() { alert('Hi'); }; 是不会弹窗的，后面接一句 f(); 才会执行函数内部的代码。上面代码翻译一下就是：

```javascript
var result = new Array(), i;
result[0] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！
result[1] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！
...
result[9] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！
i = 10;
funcs = result;
result = null;

console.log(i); // funcs[0]()就是执行 return i 语句，就是返回10
console.log(i); // funcs[1]()就是执行 return i 语句，就是返回10
...
console.log(i); // funcs[9]()就是执行 return i 语句，就是返回10
```

#### 5.5 利用闭包设置私有对象
```javascript
var db = (function() {
// 创建一个隐藏的object, 这个object持有一些数据
// 从外部是不能访问这个object的
var data = {};
// 创建一个函数, 这个函数提供一些访问data的数据的方法
return function(key, val) {
    if (val === undefined) { return data[key] } // get
    else { return data[key] = val } // set
    }
// 我们可以调用这个匿名方法
// 返回这个内部函数，它是一个闭包
})();

db('x'); // 返回 undefined
db('x', 1); // 设置data['x']为1
db('x'); // 返回 1
// 我们不可能访问data这个object本身
// 但是我们可以设置它的成员
```

### 6.闭包的几种写法(不是很好，可以不看)
[看这里](http://www.cnblogs.com/yunfeifei/p/4019504.html)
#### 6.1
```
    //这种写法没什么特别的，只是给函数添加一些属性。
    function Circle(r){
        this.r=r;
    }
    Circle.PI=3.1415926;
    Circle.prototype.area=function(){
        return Circle.PI*this.r*this.r;
    }
    var c=new Circle(1);
    alert(c.area);
```

#### 6.2
```
    //这种写法是声明一个变量，将一个函数当作值赋给变量。
    var Circle=function(){
        var obj=new Object();
        obj.PI=3.1415926;
        obj.area=function(r){
            return this.PI*r*r;
        }
        return obj;
    };
    var c=new Circle();
    alert(c.area(1.0));
```

#### 6.3
```
    //这种方法最好理解，就是new 一个对象，然后给对象添加属性和方法。
    var Circle=new Object();
    Circle.PI=3.1415926;
    Circle.area=function(r){
        return this.PI*r*r;
    }
    alert(Circle.area(1.0));
```

#### 6.4
```
//这种方法使用较多，也最为方便。var obj = {}就是声明一个空的对象。
var Circle={  
   "PI":3.14159,  
 "area":function(r){  
          return this.PI * r * r;  
        }  
};  
alert( Circle.area(1.0) ); 
```

#### 6.5
```
//第5种写法  
var Circle = new Function("this.PI = 3.14159;this.area = function( r ) {return r*r*this.PI;}");  
  
alert( (new Circle()).area(1.0) );  
```
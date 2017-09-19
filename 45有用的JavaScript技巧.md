---
title: 避免这些常见的JavaScript错误
date: 2017-08-09 19:45:40
categories: 前端
tags: [javascript]
---
<Excerpt in index | 首页摘要> 
避免这些常见的JavaScript错误
<!-- more -->
<The rest of contents | 余下全文>

-----
参考：https://modernweb.com/45-useful-javascript-tips-tricks-and-best-practices/


下面的代码在Chrome(v30),V8js引擎(V8 3.20.17.15)测试通过

### 1.使用var关键字声明变量
如果不加var关键字会导致其成为全局的属性！污染顶级变量

### 2.使用===而不是==
`==`和`!=`具有强制类型转换功能；`===`,`!==`则不会对数据进行转换，他们同时比较值和类型是否相等

```javascript
[10] === 10    // is false
[10]  == 10    // is true
'10' == 10     // is true
'10' === 10    // is false
 []   == 0     // is true
 [] ===  0     // is false
 '' == false   // is true but true == "a" is false
 '' ===   false // is false 
```

### 3.undefined, null, 0, false, NaN, ''(空字符串)
他们都是false

### 4.在每一行末尾添加分号
我们最好在每一行的结尾处使用分号，当然如果你不加也不会产生什么错误，JS解析器会自动帮你加上。更多资料请看这里https://davidwalsh.name/javascript-semicolons

### 5.使用构造函数创建对象
```javascript
function Person(firstName, lastName){
    this.firstName =  firstName;
    this.lastName = lastName;        
}  

var Saad = new Person("Saad", "Mousliki");
```

### 6.谨慎使用typeof/instanceof还有constructor
- typeof:js一元操作符，返回当前变量的最原始类型
```
typeof null  object
typeof 大部分的object对象返回的都是object，比如Array，Date
```

- constructor ：是原型的一个性质，可以被其他代码重写

- instanceof :在原型链中查找构造函数，如果有则返回true，否则false

```javascript
var arr = ["a", "b", "c"];
typeof arr;   // return "object" 
arr  instanceof Array // true
arr.constructor();  //[]
```

### 7.使用自调函数
通常被称为匿名回调函数或者立即调用函数表达式

```javascript
(function(){
    // some private code that will be executed automatically
})();  
(function(a,b){
    var result = a+b;
    return result;
})(10,20)
```

### 8.得到数组中随机的一项
```javascript
var items = [12, 548 , 'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' , 2145 , 119];

var  randomItem = items[Math.floor(Math.random() * items.length)];
```

### 9.获得某个区间的数字
这段代码在你用于伪造数据的时候会很有用，比如薪水

```javascript
var x = Math.floor(Math.random() * (max - min + 1)) + min;
```

### 10.将某个范围的数字变成数组
```javascript
var numbersArray = [] , max = 100;

for( var i=1; numbersArray.push(i++) < max;);  // numbers = [1,2,3 ... 100] 
```

### 11.随机生成一段字母和数字
```javascript
function generateRandomAlphaNum(len) {
    var rdmString = "";
    for( ; rdmString.length < len; rdmString  += Math.random().toString(36).substr(2));
    return  rdmString.substr(0, len);

}
```

### 12.将数组中的数字重新排列
```javascript
var numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411];
numbers = numbers.sort(function(){ return Math.random() - 0.5});
/* the array numbers will be equal for example to [120, 5, 228, -215, 400, 458, -85411, 122205]  */
```

可能比sort方法还好用：https://stackoverflow.com/questions/962802/is-it-correct-to-use-javascript-array-sort-method-for-shuffling/962890#962890

### 13.js中没有传统语言中（php，java）专门用来去除字符串中空格的函数，所以需要我们自己加

```javascript
String.prototype.trim = function(){return this.replace(/^s+|s+$/g, "");};  
```

### 14.将一个数组插到另一个数组后面
```javascript
var array1 = [12 , "foo" , {name "Joe"} , -2458];

var array2 = ["Doe" , 555 , 100];
Array.prototype.push.apply(array1, array2);
/* array1 will be equal to  [12 , "foo" , {name "Joe"} , -2458 , "Doe" , 555 , 100] */
```

### 15.将参数对象转换为一个数组
```javascript
var argArray = Array.prototype.slice.call(arguments);
```

### 16.验证是否是数字
```javascript
function isNumber(n){
    return !isNaN(parseFloat(n)) && isFinite(n);
}
```

### 17.验证是否是数组
```javascript
function isArray(obj){
    return Object.prototype.toString.call(obj) === '[object Array]' ;
}
```
**注意：**如果你重写了toString方法，上面的代码就失效了

可以使用数组的isArray方法，这是新加的
```javascript
Array.isArray(obj); // its a new Array method
```

如果你使用了很多框架，我们推荐使用instanceof。由于你有很多的上下文环境，导致你得到错误的答案

```javascript
var myFrame = document.createElement('iframe');
document.body.appendChild(myFrame);

var myArray = window.frames[window.frames.length-1].Array;
var arr = new myArray(a,b,10); // [a,b,10]  

// instanceof will not work correctly, myArray loses his constructor 
// constructor is not shared between frames
arr instanceof Array; // false
```

### 18. 获取数组中的最大最小值

```javascript
var  numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411]; 
var maxInNumbers = Math.max.apply(Math, numbers); 
var minInNumbers = Math.min.apply(Math, numbers);
```

### 19.清除数组里面的元素
```javascript
var myArray = [12 , 222 , 1000 ];  
myArray.length = 0; // myArray will be equal to [].
```

### 20.不要使用delete去删除数组中的某一项
使用splice去删除数组中的元素而不使用delete。使用delete会导致被删除项变为undefined而不是被移除。

**不用**
```javascript
var items = [12, 548 ,'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' ,2154 , 119 ]; 
items.length; // return 11 
delete items[3]; // return true 
items.length; // return 11 
/* items will be equal to [12, 548, "a", undefined × 1, 5478, "foo", 8852, undefined × 1, "Doe", 2154,       119]   */
```

**用这个**
```javascript
var items = [12, 548 ,'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' ,2154 , 119 ]; 
items.length; // return 11 
items.splice(3,1) ; 
items.length; // return 10 
/* items will be equal to [12, 548, "a", 5478, "foo", 8852, undefined × 1, "Doe", 2154,       119]   */
```
delete应该用来删除对象的属性


### 21.使用长度来截取数组元素
像上面我们说的那个清空数组一样，我们可以使用数组长度的性质来截取数组元素

```javascript
var myArray = [12 , 222 , 1000 , 124 , 98 , 10 ];  
myArray.length = 4; // myArray will be equal to [12 , 222 , 1000 , 124].
```
相反的，如果你把数组的length设置为高于其元素个数的值，不仅改变数组的length，而且其新的元素也会被赋值为undefined。数组的length属性不只是可读的。

```javascript
myArray.length = 10; // the new array length is 10 
myArray[myArray.length - 1] ; // undefined
```

### 22.条件语句使用逻辑或/与
```javascript
var foo = 10;  
foo == 10 && doSomething(); // is the same thing as if (foo == 10) doSomething(); 
foo == 5 || doSomething(); // is the same thing as if (foo != 5) doSomething();
```

逻辑或可以用来设置一个默认值
```javascript
function doSomething(arg1){ 
    arg1 = arg1 || 10; // arg1 will have 10 as a default value if it’s not already set
}
```

### 23.使用map()方法来循环遍历一个数组

```javascript
var squares = [1,2,3,4].map(function (val) {  
    return val * val;  
}); 
// squares will be equal to [1, 4, 9, 16] 
```

### 24.保留几位小数
```javascript
var num =2.443242342;
num = num.toFixed(4);  // num will be equal to 2.4432
```
注意，toFixed方法得到的是字符串而不是数值型！！

### 25.浮点运算问题
```javascript
0.1 + 0.2 === 0.3 // is false 
9007199254740992 + 1 // is equal to 9007199254740992  
9007199254740992 + 2 // is equal to 9007199254740994
```
为什么会出现：0.1 +0.2=0.30000000000000004这种情况？你需要知道的是,所有的JavaScript数据内部浮动点代表64位二进制根据IEEE 754标准，关于这个，[请看这里](http://2ality.com/2012/04/number-encoding.html)

可以使用toFixed和toPrecision来修复这个问题

### 26.使用for..in..来检测对象的属性(避免取到原型属性)
避免遍历对象的原型的属性
```javascript
for (var name in object) {  
    if (object.hasOwnProperty(name)) { 
        // do something with name                    
    }  
}
```

### 27.逗号运算符
```javascript
var a = 0; 
var b = ( a++, 99 ); 
console.log(a);  // a will be equal to 1 
console.log(b);  // b is equal to 99
```

### 28.缓存需要计算或查询的变量
我们可以使用jQuery选择器来选择变量
```javascript
var navright = document.querySelector('#right'); 
var navleft = document.querySelector('#left'); 
var navup = document.querySelector('#up'); 
var navdown = document.querySelector('#down');
```

### 29.使用isFinite来验证参数
```javascript
isFinite(0/0) ; // false 
isFinite("foo"); // false 
isFinite("10"); // true 
isFinite(10);   // true 
isFinite(undefined);  // false 
isFinite();   // false 
isFinite(null);  // true  !!! 
```

### 30避免使用负数的索引数组
```javascript
var numbersArray = [1,2,3,4,5]; 
var from = numbersArray.indexOf("foo") ;  // from is equal to -1 
numbersArray.splice(from,2);    // will return [5]
```
确保传递给splice 的参数不是负数

### 31.序列化和反序列化
```javascript
var person = {name :'Saad', age : 26, department : {ID : 15, name : "R&D"} }; 
var stringFromPerson = JSON.stringify(person); 
/* stringFromPerson is equal to "{"name":"Saad","age":26,"department":{"ID":15,"name":"R&D"}}"   */ 
var personFromString = JSON.parse(stringFromPerson);  
/* personFromString is equal to person object  */
```

### 32.避免使用eval和构造函数
eval好构造函数非常的影响性能，每次都必须被js引擎转换为可执行的代码
```javascript
var func1 = new Function(functionCode);
var func2 = eval(functionCode);
```

### 33.避免使用with()
[看这里](https://modernweb.com/ecommerce-conversion-optimization/)

在全局作用域下使用with()来插入一个变量，这可能导致重名的变量被覆盖。

### 34.避免在数组里面使用for..in..

不要这么做
```javascript
var sum = 0;  
for (var i in arrayNumbers) {  
    sum += arrayNumbers[i];  
}
```
最好这么做
```javascript
var sum = 0;  
for (var i = 0, len = arrayNumbers.length; i < len; i++) {  
    sum += arrayNumbers[i];  
}
```
i和len在循环之前只声明了一次，比下面这种写法好
```
for (var i = 0; i < arrayNumbers.length; i++)
```
为什么呢？因为上面这种写法，在每次循环的时候都会被重新声明一次

问题的根源就在于每次js引擎都要去重新计算一下length值，其实它已经是固定值了

### 35.应该传递给setTimeout()和setInterval()函数名而不是字符串！

如果你传递的参数是字符串，setTimeout()和setInterval()就会和eval一样了，影响速度

不要这么写
```javascript
setInterval('doSomethingPeriodically()', 1000);  
setTimeout('doSomethingAfterFiveSeconds()', 5000);
```
要这么写
```javascript
setInterval(doSomethingPeriodically, 1000);  
setTimeout(doSomethingAfterFiveSeconds, 5000);
```

### 36.多用switch/case而不是if/else
当有两个以上的case选项的时候，switch比if/else元算速度快，并且代码比较整洁；如果你有超过10个以上的case，就不要用了

### 37.当switch/case用于一个数字区间的时候

Using a switch/case statement with numeric ranges is possible with this trick.

```javascript
function getCategory(age) {  
    var category = "";  
    switch (true) {  
        case isNaN(age):  
            category = "not an age";  
            break;  
        case (age >= 50):  
            category = "Old";  
            break;  
        case (age <= 20):  
            category = "Baby";  
            break;  
        default:  
            category = "Young";  
            break;  
    };  
    return category;  
}  
getCategory(5);  // will return "Baby"
```

### 38.给新建对象的prototype赋值为一个对象
It’s possible to write a function that creates an object whose prototype is the given argument like this…
一个用来新建对象的函数，我们可以把他的原型prototype当做形参一样传进来。(翻译的不准确就看上面的英文)
```javascript
function clone(object) {  
    function OneShotConstructor(){}; 
    OneShotConstructor.prototype= object;  
    return new OneShotConstructor(); 
} 
clone(Array).prototype ;  // []
```
### 39. An HTML escaper function
```javascript
function escapeHTML(text) {  
    var replacements= {"<": "&lt;", ">": "&gt;","&": "&amp;", """: "&quot;"};                      
    return text.replace(/[<>&"]/g, function(character) {  
        return replacements[character];  
    }); 
}
```

### 40.避免在内部循环里面使用try-catch-finally 
The try-catch-finally construct creates a new variable in the current scope at runtime each time the catch clause is executed where the caught exception object is assigned to a variable.

不要这么写
```javascript
var object = ['foo', 'bar'], i;  
for (i = 0, len = object.length; i <len; i++) {  
    try {  
        // do something that throws an exception 
    }  
    catch (e) {   
        // handle exception  
    } 
}
```

要这么写
```javascript
var object = ['foo', 'bar'], i;  
try { 
    for (i = 0, len = object.length; i <len; i++) {  
        // do something that throws an exception 
    } 
} 
catch (e) {   
    // handle exception  
} 
```

### 41.设计超时限定来终止XMLHttpRequests
可能由于网络问题导致很长时间内XMLHttpRequests不能访问。我们需要设计一个超时限定来终止请求

```javascript
var xhr = new XMLHttpRequest (); 
xhr.onreadystatechange = function () {  
    if (this.readyState == 4) {  
        clearTimeout(timeout);  
        // do something with response data 
    }  
}  
var timeout = setTimeout( function () {  
    xhr.abort(); // call error callback  
}, 60*1000 /* timeout after a minute */ ); 
xhr.open('GET', url, true);  

xhr.send();
```
避免完全同步XHR调用

#### 42.处理WebSocket超时
Generally when a WebSocket connection is established, a server could time out your connection after 30 seconds of inactivity. The firewall could also time out the connection after a period of inactivity.

To deal with the timeout issue you could send an empty message to the server periodically. To do this, add these two functions to your code: one to keep alive the connection and the other one to cancel the keep alive. Using this trick, you’ll control the timeout.

Add a timerID…

```javascript
var timerID = 0; 
function keepAlive() { 
    var timeout = 15000;  
    if (webSocket.readyState == webSocket.OPEN) {  
        webSocket.send('');  
    }  
    timerId = setTimeout(keepAlive, timeout);  
}  
function cancelKeepAlive() {  
    if (timerId) {  
        cancelTimeout(timerId);  
    }  
}
```
The keepAlive() function should be added at the end of the onOpen() method of the webSocket connection and the cancelKeepAlive() at the end of the onClose() method.

### 43.[基本操作比函数调用运算速度快，性能高](https://dev.opera.com/articles/efficient-javascript/?page=2#primitiveoperator)

不要这么用：
```javascript
var min = Math.min(a,b); 
A.push(v);
```
可以这么写
```javascript
var min = a < b ? a : b; 
A[A.length] = v;
```

### 44.使用jslint和JSMin来优化你的代码

### 45.最棒的javascript学习资源
- Code Academy JavaScript tracks:https://www.codecademy.com/learn/javascript

- Eloquent JavaScript by Marjin Haverbeke:http://eloquentjavascript.net/

- Advanced JavaScript by John Resig:http://ejohn.org/apps/learn/


### 参考
- [JavaScript Performance Best Practices (CC)](https://networks.nokia.com/developer/mn)

- [Google Code JavaScript tips](https://code.google.com/p/jslibs/wiki/JavascriptTips)

- [StackOverFlow tips and tricks](https://stackoverflow.com/questions/724826/javascript-tips-and-tricks-javascript-best-practices)

- [TimeOut for XHR](https://stackoverflow.com/questions/6888409/settimeout-for-xhr-requests)
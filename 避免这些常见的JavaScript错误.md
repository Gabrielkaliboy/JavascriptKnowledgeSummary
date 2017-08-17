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

### 19.情况数组里面的元素
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
```
var foo = 10;  
foo == 10 && doSomething(); // is the same thing as if (foo == 10) doSomething(); 
foo == 5 || doSomething(); // is the same thing as if (foo != 5) doSomething();
```
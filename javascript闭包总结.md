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
```
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
```
function aa(){
var y=2;
}
//调用函数aa内部的局部变量y会报错
console.log(y);
// y is not defined
```

#### 1.3 不加var 声明变量
函数内部不加var声明变量，会导致此变量变为全局变量！
```
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
```
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
```
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

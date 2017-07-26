---
title: javascript不常见问题
date: 2017-07-25 13:15:40
categories: 前端
tags: [javascript]
---
<Excerpt in index | 首页摘要> 
javascript不常见问题
<!-- more -->
<The rest of contents | 余下全文>

-----
### 1.自执行匿名函数
常见：
```javascript
(function(){
function a(){
   alert("a");
}
})();
```
其他：
```javascript
(function () { /* code */ } ()); 
!function () { /* code */ } ();
~function () { /* code */ } ();
-function () { /* code */ } ();
+function () { /* code */ } ();
```
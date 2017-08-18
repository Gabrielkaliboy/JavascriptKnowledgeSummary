---
title: IE里面的条件注释
date: 2017-08-18 11:00:40
categories: 前端
tags: [javascript]
---
<Excerpt in index | 首页摘要> 
IE里面的条件注释
<!-- more -->
<The rest of contents | 余下全文>

-----

[英文官方版本](https://msdn.microsoft.com/en-us/library/ms537512(v=vs.85).aspx)

除了使用15中的脚本检测IE版本，我们还可以使用IE的条件注释。条件注释可以轻易地更早发现早期的IE版本。条件注释是层叠样式表（CSS）用于区分IE特定版本的首选方式。

**重要提示** 自IE10起，标准模式不再支持条件注释。而是采用特征检测给浏览器不支持的功能来提供备用策略。有关标准模式的详细信息，请参阅定义文档兼容性。

### 1.术语
熟悉下列术语有助于你学习文档兼容性。　

| 名词        | 描述   |  
| --------   | -----:  | 
| expression     | 由运算符、特征和（或）值组合形成一个条件语句 |   
| downlevel browser        |   任何浏览器除了IE5+，其他低版本浏览器不支持条件注释   |   
| uplevel browser        |    IE5+支持条件注释  |  
| downlevel-revealed     | 低版本浏览器经过条件注释的解析。如果表达式为true时，IE5+会渲染HTML页面 |   
| downlevel-hidden        |   低版本浏览器会忽视条件注释。如果表达式为true时，IE5+会渲染HTML页面|    

### 2.使用条件注释的好处
下列表格中展示了基本语法类型，第一个注释是最基本的HTML注释。表格比较并展示每一种条件注释的不同语法的用法。

| 注释类型        | 语法或可能的值   |  
| --------   | -----:  | 
| HTML标准注释	     | 	\<!-- Comment content  --> |   
| downlevel-hidden        |   \<!--[if expression]> HTML <![endif]-->   |  
| downlevel-revealed        |    \<![if expression]> HTML <![endif]>    | 


expression是由功能、操作符和值组成的。下表列出了支持的功能，并介绍了每个功能支持的值。


| Item        | Example   |  注释  |
| --------   | -----:  | :----:  |
| IE     | [if IE] |   对应IE的版本功能来查看该网页     |
| value        |  [if IE 7]   |   一个整数或浮点标号对应于浏览器的版本。如果是与版本号匹配的浏览器版本，则返回true。   |
| WindowsEdition        |   [if WindowsEdition]    |  Windows 7的IE8。  "WindowsEdition"对应Windows的版本功能。  |
| value     | [if WindowsEdition 1] |   整数对应Windows版本。如果正在使用的的值相匹配，则返回true。     |
| true        |   [if true]   |   结果始终为true.   |
| false        |    	[if false]    |  结果始终为false.  |



下表描述了可用于创建条件表达式的运算符。
| Item        | Example   |  注释  |
| --------   | -----:  | :----:  |
| !     | [if !IE] |   NOT运算符.。被放置在要素、运算符或表达式之前，扭转表达式的布尔含义。     |
| lt        |   	[if lt IE 5.5]   |   小于运算符。如果第一个参数小于第二个参数，返回true。   |
| lte        |    [if lte IE 6]    |  小于或等于运算符。如果第一个参数小于或等于第二个参数，返回true。  |
| gt     | [if gt IE 5] |   大于运算符。如果第一个参数大于第二个参数，返回true。     |
| gte        |   [if gte IE 7]   |   大于或等于运算符。如果第一个参数大于或等于第二个参数，返回true。   |
| ( )        |    [if !(IE 7)]    |  子表达式运算符。配合使用布尔运算符来创建更复杂的表达式。  |
| &        |   [if (gt IE 5)&(lt IE 7)]   |   AND运算符。如果所有的子表达式的值为真，返回true。   |
| \|        |    [if (IE 6)|(IE 7)]    |  OR运算符。如果任何一个子表达式的计算结果为true，返回true。  |


### 3.Downlevel-hidden条件注释
此示例显示了一个低版本隐藏的条件注释，其中包含文本。
```html
<!--[if IE 8]>
<p>Welcome to Internet Explorer 8.</p>
<![endif]-->
```

Downlevel-hidden条件注释类似于基本的HTML注释，包含连字符（“ - ”）在开启和关闭标签。条件显示在标签的开口部，和[ENDIF]被放置在标签的封闭部分之前。内容放在注释标签内。

因为前四个字符和注释的最后三个字符是相同的HTML注释元素，所以低版本浏览器会忽略注释块内的HTML内容。由于内容被有效地不支持条件注释的浏览器隐藏，这种类型的条件注释被称为**低版本隐藏**。


如果条件表达式的结果为真，则对注释块里面的内容进行分析，并通过Internet Explorer 5及更高版本的渲染。针对Internet Explorer而专门设计的内容，这种做法特别有效。
下一个示例演示了如何在客户端脚本块放置一个条件注释;在这种情况下，消息在IE5+浏览器如何显示的。

```html
<!--[if gte IE 7]>
<script>
  alert("Congratulations! You are running Internet Explorer 7 or a later version of Internet Explorer.");
</script>
<p>Thank you for closing the message box.</p>
<![endif]--> 
```
在上面的例子中，浏览器的版本只有主要的数字进行比较，因为它是在条件表达式指定的唯一数字。要比较这两个主要和次要版本号，要同时指定数字。

### 4.ownlevel-revealed条件注释
ownlevel-revealed条件注释允许在不承认条件注释的浏览器上包含内容。尽管条件注释被忽略，但它包含的HTML内容却没有被忽略。如果条件为true,IE5+仍然解析和渲染内容。ownlevel-revealed条件注释和Downlevel-hidden条件注释是互补的。
下面片段展示了一个典型的低版本条件注释：

```html
<![if lt IE 8]>
<p>Please upgrade to Internet Explorer version 8.</p>
<![endif]>
```

当比较这种类型HTML注释时，发现在注释块在"<!"和 ">"之后（前）没有连字符("--") ，因此，注释分隔符被视为无法识别的HTML。因为浏览器不能识别Downlevel-hidden条件注释，那么它就什么都不做了。

### 5.版本号
条件表达式可以用来确定查看网页浏览器的版本或Windows上用于运行浏览器的版本。在这两种情况下，表达式的值称为版本号，必须正确地指定，以获得所需的结果。
检测浏览器的版本时，主要的浏览器版本被指定为整数。要检查是否有轻微的浏览器版本，版本号遵循由一个小数点和四位数组成的规则。例如，微软的Internet Explorer5.5的发布版本的版本载体是5.5000。

在下面的例子中，仅主要版本号被指定。
```html
<!--[if IE 5]>
<p>Welcome to any incremental version of Internet Explorer 5!</p>
<![endif]-->
```

下面的测试正确地识别Internet Explorer 5。

```html
<!--[if IE 5.0000]>
<p>Welcome to Internet Explorer 5.0!</p>
<![endif]-->
```

例子　
```html
<!--[if IE]><p>You are using Internet Explorer.</p><![endif]-->
<![if !IE]><p>You are not using Internet Explorer.</p><![endif]>

<!--[if IE 7]><p>Welcome to Internet Explorer 7!</p><![endif]-->
<!--[if !(IE 7)]><p>You are not using version 7.</p><![endif]-->

<!--[if gte IE 7]><p>You are using IE 7 or greater.</p><![endif]-->
<!--[if (IE 5)]><p>You are using IE 5 (any version).</p><![endif]-->
<!--[if (gte IE 5.5)&(lt IE 7)]><p>You are using IE 5.5 or IE 6.</p><![endif]-->
<!--[if lt IE 5.5]><p>Please upgrade your version of Internet Explorer.</p><![endif]-->

<!--[if true]>You are using an <em>uplevel</em> browser.<![endif]-->
<![if false]>You are using a <em>downlevel</em> browser.<![endif]>

<!--[if true]><![if IE 7]><p>This nested comment is displayed in IE 7.</p><![endif]><![endif]-->
```
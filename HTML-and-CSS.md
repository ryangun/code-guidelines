# HTML和CSS编码指导
编写灵活、健壮、可维护性的HTML和CSS的标准



----------



## 目录

* [黄金法则](#golden-rule)
* [HTML](#html)
  * [HTML语法](#html-syntax)
  * [HTML5 doctype](#html5-doctype)
  * [实用大于语义](#pragmatism-over-semantics)
  * [属性顺序](#attribute-order)
  * [JavaScript生成标签](#javascript-generated markup)
* [CSS](#css)
  * [CSS语法](#css-syntax)
  * [声明顺序](#declaration-order)
  * [细微偏移原则的情况](#formatting-exceptions)
    * [带前缀的属性](#prefixed-properties)
    * [大量包含单条声明的声明块](#rules-with-single-declarations)
  * [可读性](#human-readable)
    * [注释](#comments)
    * [class名](#classes)
    * [选择器](#selectors)
  * [JavaScript钩子](#javascript-hooks)
  * [代码组织](#organization)


----------



<h2 id="golden-rule">黄金法则</h2>

> 在任何代码库中，无论有多少人参与及贡献，所有代码都应该如同一个人编写的一样。

这意味着应一直严格遵循这份指导。补充与贡献，请[在GitHub提交一个issue](https://github.com/AirCoding/code-guidelines)。



----------



<h2 id="html">HTML</h2>


<h3 id="html-syntax">HTML语法</h3>

* 使用两个空格长度的软缩进（使用空格缩进）
* 嵌套的元素应该被缩进一次（两个空格）
* 一直使用双引号，而不要使用单引号
* 自闭合元素不要包含最后的斜杠


**不正确的例子：**

````html
<!DOCTYPE html>
<html>
<head>
<title>Page title</title>
</head>
<body>
<img src='images/company-logo.png' alt='Company' />
<h1 class='hello-world'>Hello, world!</h1>
</body>
</html>
````

**正确的例子：**

````html
<!DOCTYPE html>
<html>
  <head>
    <title>Page title</title>
  </head>
  <body>
    <img src="images/company-logo.png" alt="Company">
    <h1 class="hello-world">Hello, world!</h1>
  </body>
</html>
````


<h3 id="html5-doctype">HTML5 doctype</h3>

在每个HTML页面前面包含下面的doctype声明，尽可能地强制每个浏览器使用标准模式

````html
<!DOCTYPE html>
````


<h3 id="pragmatism-over-semantics">实用大于语义</h3>

努力保持HTML的标准和语义，但不要以牺牲实用为代价。使用尽可能少的标签和尽量避免复杂。


<h3 id="attribute-order">属性顺序</h3>

为了便于阅读代码，HTML属性应该按照特定的顺序书写

* class
* id
* data-*
* for|type|href

因此你的代码看起来像：

````html
<a class="" id="" data-modal="" href="">Example link</a>
````

<h3 id="javascript-generated markup">JavaScript生成标签</h3>

在javascript中生成标签让其难以查找、编辑和性能变差。因此，别这样做。



----------



<h2 id="css">CSS</h2>

<h3 id="css-syntax">CSS语法</h3>

* 使用两个空格长度的软缩进（使用空格缩进）
* 使用分组选择器时，每个单独的选择器独占一行
* 在声明块的左大括号前保留一个空格
* 声明块的右大括号独占一行
* 每个声明的<code>:</code>后保留一个空格
* 在声明块中，每个声明独占一行
* 对于声明块的最后一个声明，始终保留结束的分号
* 每个声明块之间保留一个空行
* 以逗号分隔的值应该在每个逗号后面包含一个空格
* 不要在RGB或者RGBa颜色值中的逗号后面包含空格，小数不带前导零
* 十六进制数值使用简写，如使用<code>#fff</code>代替<code>#FFF</code>
* 可能的情况下使用简写的十六进制值，如使用<code>#fff</code>代替<code>#ffffff</code>
* 选择器中的属性值加上引号，如<code>input[type="text"]</code>
* 避免为值为零的数值指定单位，如使用<code>margin: 0;<code>代替<code>margin: 0px;</code>

**不正确的例子：**

````css
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0px 0px 15px;
  background-color:rgba(0, 0, 0, 0.5);
  box-shadow:0 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}
````

**正确的例子**

````css
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin: 0 0 15px;
  background-color: rgba(0,0,0,.5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
````


<h3 id="declaration-order">声明顺序</h3>

将相关的属性组合在一起，并且将对结构来说比较重要的属性（如定位或者盒模型）放在前面，先于排版、背景及颜色等属性。

````css
.declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Misc */
  opacity: 1;
}
````


<h3 id="formatting-exceptions">细微偏移原则的情况</h3>

某些情况下，允许细微偏移原则。

<h4 id="prefixed-properties">带前缀的属性</h4>

使用带前缀的属性时，缩进每一个属性，让其值对齐，方便多行编辑。

````css
.selector {
  -webkit-border-radius: 3px;
     -moz-border-radius: 3px;
          border-radius: 3px;
}
````

Vim和Sublime Text等编辑器支持多行编辑。

<h4 id="rules-with-single-declarations">大量包含单条声明的声明块</h4>

对于大量仅包含单条声明的声明块，可以使用一种略微不同的单行格式。在这种情况下，在左大括号之后及右大括号之前都应该保留一个空格。

````css
.span1 { width: 60px; }
.span2 { width: 140px; }
.span3 { width: 220px; }

.sprite {
  display: inline-block;
  width: 16px;
  height: 15px;
  background-image: url(../img/sprite.png);
}
.icon           { background-position: 0 0; }
.icon-home      { background-position: 0 -20px; }
.icon-account   { background-position: 0 -40px; }
````


<h3 id="human-readable">可读性</h3>

代码由人书写及维护，确保你的代码具有描述性、很好的注释和他人易于上手。

<h4 id="comments">注释</h4>

好的代码注释交代了上下文或者目的，而不应该只是注明了组件、类的名字。

**不好的例子：**

````css
/* Modal header */
.modal-header {
  ...
}
````

**好的例子：**

````css
/* Wrapping element for .modal-title and .modal-close */
.modal-header {
  ...
}
````

<h4 id="classes">class名</h4>

* 保持class名的小写和使用连字符(-)（不要使用下划线或者驼峰命名法）
* 避免无意义的简写标记
* 尽可能地保持class名简短和简洁
* 使用有意义的名字；使用能表达结构或者有意义的的名字，而不是即兴起的名字
* class名的前缀基于最近的父元素的class名


**不好的例子：**

````css
.t { ... }
.red { ... }
.header { ... }
````

**好的例子：**

````css
.tweet { ... }
.important { ... }
.tweet-header { ... }
````

<h4 id="selectors">选择器</h4>

* 使用class名，而不是一般的元素标签
* 保持它们足够的短及限制每个选择器元素的数量在三个以内
* 必要的时候指定最近的父元素（例如，class名没有使用前缀的时候）
* 避免过分修饰选择器，如使用`.nav{}`代替`ul.nav{}`

**不好的例子：**

````css
span { ... }
.page-container #stream .stream-item .tweet .tweet-header .username { ... }
.avatar { ... }
````

**好的例子：**

````css
.avatar { ... }
.tweet-header .username { ... }
.tweet .avatar { ... }
````

<h3 id="javascript-hooks">JavaScript钩子</h3>

千万不要用CSS class作为JavaScript钩子。如果你要把JavaScript和某些标记绑定起来的话，写一个JavaScript专用的class，划定一个前缀.js-的命名空间，例如`.js-toggle`，`.js-drag-and-drop`。这意味着我们可以通过class同时绑定JavaScript和CSS而不会因为冲突而引发麻烦。

````html
<th class="is-sortable  js-is-sortable">
</th>
````

上面的这个标记有两个class，你可以用其中一个来给这个可排序的表格栏添加样式，用另一个添加排序功能。


<h3 id="organization">代码组织</h3>

* 逻辑上对不同的代码进行分离
* 不同的组件(component)的css尽量用不同的css文件（可以在build阶段用工具合并到一起）
* 如果用到了预处理器（比如less），把一些公共的样式代码抽象成变量，例如颜色，字体等等

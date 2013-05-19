基本编码标准
=====================

This section of the standard comprises what should be considered the standard
coding elements that are required to ensure a high level of technical
interoperability between shared PHP code.

这份文档中的关键词""MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY" 和 "OPTIONAL"在[RFC 2119][]中的描述中有解释。

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md


1. 前言
-----------

- 文件必须(MUST)只使用`<?php`和`<?=`标签。

- 文件必须(MUST)只使用不带的BOM的UTF-8格式保存。

- 文件不应该(SHOULD)同时进行符号声明（类、函数、常量等）和有副作用产生的操作（例如输出、改变.ini设置等）。

- 命名空间和类必须(MUST)遵循[RSR-0][]。

- 类名必须(MUST)使用`StudlyCaps`命名格式声明。

- 类常量必须(MUST)使用所有大写字母或下划线分割符进行声明。

- 方法名必须(MUST)使用`camelCase`命名格式声明。


2. 文件
--------

### 2.1. PHP标签

PHP代码必须(MUST)使用`<?php ?>`长标签或者`<?= ?>`短输出标签；不能(MUST NOT)使用其它标签。

### 2.2. 字符编码

PHP代码必须只使用不带BOM的UTF-8格式。

### 2.3. 副作用 (Side Effects)

一个文件应该(SHOULD)进行符号声明（类、函数、常量等）而无其它副作用产生，或者它应该(SHOULD)执行逻辑代码，伴有副作用产生，两者不应该(SHOULD NOT)同时进行。

词语"副作用"意为逻辑代码的执行不直接与类、函数、常量等的声明产生联系，*有时在包含文件时发生*。

“副作用”包含但不限于：输出、`require`或者`include`的直接使用、连接到额外的服务、修改ini设置、报错或异常、修改全局或静态变量、从一个文件读或写等。

以下是一个同时进行声明和产生副作用的例子；
即，一个应该避免的例子：


```php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
    // function body
}
```

以下是一个文件包含声明而无副作用的例子；即应该参考的例子：

```php
<?php
// declaration
function foo()
{
    // function body
}

// conditional declaration is *not* a side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
```


3. 命名空间和类名
----------------------------

命名空间和类必须(MUST)遵循[PSR-0][]。

这意味着一个文件一个类，类至少在一级命名空间内：一个顶级的vendor名称。

类名必须(MUST)使用`StudlyCaps`命名格式声明。

PHP 5.3及以后的代码必须(MUST)使用正式的命名空间。

例如：

```php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
```

5.2.x及以前的代码应该(SHOULD)使用在类名前加上`Vendor_`的伪命名空间惯例

```php
<?php
// PHP 5.2.x and earlier:
class Vendor_Model_Foo
{
}
```

4. 类常量，属性和方法
-------------------------------------------

术语“类”指所有的类、接口和traits

### 4.1. 常量

类常量必须(MUST)使用所有大写字母或下划线分割符进行声明。
例如：

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. 属性

这个指导故意避免了提及属性名使用`$StudlyCaps`、`$camelCase`还是`$under_score`命名法命名。

无论哪种命名方法被使用，它都应该在一个合理的范围内持续应用。这个范围可以是vendor-level、package-level、class-level或者method-level。

### 4.3. 方法

方法名必须(MUST)使用`camelCase`命名格式声明。

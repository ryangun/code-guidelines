# PHP编码指导



----------



## 目录

* [基本编码标准]()
* [代码风格指导]()



----------



## 基本编码标准

1. 前言
-----------

- 文件必须只使用`<?php`和`<?=`标签。

- 文件必须只使用不带的BOM的UTF-8格式保存。

- 文件不应该同时进行符号声明（类、函数、常量等）和有副作用产生的操作（例如输出、改变.ini设置等）。

- 类名必须使用`StudlyCaps`命名格式声明。

- 类常量必须使用所有大写字母或下划线分割符进行声明。

- 方法名必须使用`camelCase`命名格式声明。


2. 文件
--------

### 2.1. PHP标签

PHP代码必须使用`<?php ?>`长标签或者`<?= ?>`短输出标签；不能(MUST NOT)使用其它标签。

### 2.2. 字符编码

PHP代码必须只使用不带BOM的UTF-8格式。

### 2.3. 副作用 (Side Effects)

一个文件应该进行符号声明（类、函数、常量等）而无其它副作用产生，或者它应该执行逻辑代码，伴有副作用产生，两者不应该同时进行。

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

命名空间和类必须遵循[PSR-0][]。

这意味着一个文件一个类，类至少在一级命名空间内：一个顶级的vendor名称。

类名必须使用`StudlyCaps`命名格式声明。

PHP 5.3及以后的代码必须使用正式的命名空间。

例如：

```php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
```

5.2.x及以前的代码应该使用在类名前加上`Vendor_`的伪命名空间惯例

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

类常量必须使用所有大写字母或下划线分割符进行声明。
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

方法名必须使用`camelCase`命名格式声明。



----------



## 编码风格指导

1. 前言
-----------

- 代码必须使用tab进行缩进，而不是空格

- 不必有一个硬限制(hard limit)代码行长度；软限制(soft limit)必须是120个字符；行应该为80字符长度或者更短。

- 在`namespace`声明之后必须有一个空行，在use声明块后必须有一个空行。

- 类的左大括号必须独占一行，右大括号必须在主体后独占一行。

- 方法的左大括号必须独占一行，右大括号必须在主体后独占一行。

- 可见性必须在所有属性和方法中声明；`abstract`和`final`必须在可见性之前声明；`static`必须在可见性之后声明。
  
- 控制结构关键字后面必须有一个空格；方法和函数调用后面不能有空格。

- 控制结构的左大括号必须在同一行，右大括号必须在主体后独占一行。

- 控制结构的左小括号后面不能有空格，右小括号前面不能有空格。

### 1.1. 例子

这个例子作为一个快速概观，包含了下面的一些规则：

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
	public function sampleFunction($a, $b = null)
	{
		if ($a === $b) {
			bar();
		} elseif ($a > $b) {
			$foo->bar($arg1);
		} else {
			BazClass::bar($arg2, $arg3);
		}
	}

	final public static function bar()
	{
		// method body
	}
}
```

2. 通用
----------

### 2.1 文件

所有的PHP文件必须使用Unix LF(linefeed)换行符。

所有只包含PHP代码的文件省略`?>`结束标签

### 2.2. 行

不能有硬限制(hard limit)的行长度。

行长度的软限制(soft limit)必须为120字符；自动风格检查器在超过软限制(soft limit)时必须警告而不能报错。

行不应该长于80字符；行长于80字符应该分多行写，后续的行都不超过80字符。

在非空的行后面不能有尾部空白。

空白行可以用来提高可读性和区分出相关的代码块。


每一行不能多于一条语句。

### 2.3. 缩进

代码必须使用tab进行缩进，而不要使用空格缩进。

### 2.4. 关键字和Ture/False/Null

PHP关键字[keywords][]必须使用小写形式。

PHP常量`true`、`false`和`null`必须小写。

[keywords]: http://php.net/manual/en/reserved.keywords.php



3. 命名空间和Use声明
---------------------------------

`namespace`声明存在的时候后面必须有一个空行。

When present, all `use` declarations MUST go after the `namespace`
declaration.
所有`use`声明存在的时候必须在`namespace`声明之后。

每个`use`声明必须独占一行。

`use`代码块后面必须有一个空格。

例如：

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... additional PHP code ...

```


4. Classes, Properties, and Methods
4. 类、属性和方法
-----------------------------------

术语“类”指所有的类、接口和traits


### 4.1. Extends和Implements

`extends`和`implements`关键字必须和类名在同一行声明。

类的左大括号必须独占一行；类的右大括号必须在类主体之后独占一行。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
	// constants, properties, methods
}
```

`implements`的列表应该分开多行写，后续的每一行都应该缩进一次。这样做，列表中的第一项应该处于下一行，一行一个接口。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
	\ArrayAccess,
	\Countable,
	\Serializable
{
	// constants, properties, methods
}
```

### 4.2. 属性

所有属性必须声明可见性

`var`关键字不能在声明属性中使用。

每个语句不应该多于一个属性的声明。

属性名不应该以一个下划线在开头表明protected和private的可见性。

一个属性的声明看起来应该像下面这样：

```php
<?php
namespace Vendor\Package;

class ClassName
{
	public $foo = null;
}
```

### 4.3. 方法

可见性必须在所有方法中声明

方法名不应该以一个下划线在开头表明protected和private的可见性。

方法名后不能有空格，左大括号必须独占一行，右大括号必须在方法主体后面独占一行。在左小括号后面不能有空格，也不能在右小括号前有空格。

一个方法的声明看起来应该像下面这样，注意小括号、逗号、空格和大括号的位置：

```php
<?php
namespace Vendor\Package;

class ClassName
{
	public function fooBarBaz($arg1, &$arg2, $arg3 = [])
	{
		// method body
	}
}
``` 

### 4.4. 方法参数

在参数列表中，在每个逗号前不应该有空格，在每个逗号后都应该有个空格。

带有默认值的参数应该在参数列表的后面部分。

```php
<?php
namespace Vendor\Package;

class ClassName
{
	public function foo($arg1, &$arg2, $arg3 = [])
	{
		// method body
	}
}
```

参数列表可以跨多行写，每个后续行都要缩进一次。这样做，列表中的第一项必须在下一行，每一行只能有一个参数。

参数列表跨多行写的时候，右小括号和左大括号必须一起占一行，中间保留一个空格。

```php
<?php
namespace Vendor\Package;

class ClassName
{
	public function aVeryLongMethodName(
		ClassTypeHint $arg1,
		&$arg2,
		array $arg3 = []
	) {
		// method body
	}
}
```

### 4.5. `abstract`, `final`和`static`

若有`abstract`和`final`声明，它们必须在可见性声明之前。

若有`static`声明，它必须在可见性声明之后。

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
	protected static $foo;

	abstract protected function zim();

	final public static function bar()
	{
		// method body
	}
}
```

### 4.6. 方法和函数调用

执行一个方法或者函数调用的时候，在方法或者函数名和左小括号之间不能有空格，在左小括号之后不能有空格，以及在右小括号前不能有空格。在参数列表中，每个逗号前不能有空格，每个逗号后必须有一个空格。

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

参数列表可以跨多行写，后续的行都要缩进一次。这样做，列表中的第一项必须放在下一行，每一行放置一个参数。

```php
<?php
$foo->bar(
	$longArgument,
	$longerArgument,
	$muchLongerArgument
);
```

5. 控制结构
---------------------

控制结构通用风格的规则如下：

- 控制结构关键字后面必须有一个空格
- 左小括号后面不能有空格
- 右小括号之前不能有空格
- 右小括号和左大括号之间必须保留一个空格
- 结构主体必须缩进一次
- 右大括号必须在主体后独占一行

每个结构的主体必须用大括号包围起来，这样做使结构的表示标准化，并降低添加新行到主体时引入错误的可能性。


### 5.1. `if`, `elseif`, `else`

`if`结构看起来像下面这样。注意小括号、空格和大括号的位置；`else`和`elseif`和前一个主体的右大括号在同一行。

```php
<?php
if ($expr1) {
	// if body
} elseif ($expr2) {
	// elseif body
} else {
	// else body;
}
```

应该使用关键字`elseif`代替`else if`，使得所有的控制关键字看起来都是一个单词。


### 5.2. `switch`, `case`

`switch`结构看起来想下面这样。注意小括号、空格和大括号的位置。`case`语句必须缩进一次，`break`关键字（或者其它结束关键字）必须和`case`的主体在同一个缩进级别。在一个非空`case`主体中如果没有`break`，必须注释`// no break`。

```php
<?php
switch ($expr) {
	case 0:
		echo 'First case, with a break';
		break;
	case 1:
		echo 'Second case, which falls through';
		// no break
	case 2:
	case 3:
	case 4:
		echo 'Third case, return instead of break';
		return;
	default:
		echo 'Default case';
		break;
}
```


### 5.3. `while`, `do while`

`while`语句看起来像下面这样。注意小括号、空格和大括号的位置。

```php
<?php
while ($expr) {
	// structure body
}
```

类似的，`do while`语句开起来像下面这样。注意小括号、空格和大括号的位置。

```php
<?php
do {
	// structure body;
} while ($expr);
```

### 5.4. `for`

`for`语句看起来像下面这样。注意小括号、空格和大括号的位置。

```php
<?php
for ($i = 0; $i < 10; $i++) {
	// for body
}
```

### 5.5. `foreach`
	
`foreach`语句看起来像下面这样。注意小括号、空格和大括号的位置。

```php
<?php
foreach ($iterable as $key => $value) {
	// foreach body
}
```

### 5.6. `try`, `catch`

`try catch`块看起来像下面这样。注意小括号、空格和大括号的位置。

```php
<?php
try {
	// try body
} catch (FirstExceptionType $e) {
	// catch body
} catch (OtherExceptionType $e) {
	// catch body
}
```

6. 闭包
-----------

闭包声明必须在`function`关键字后保留一个空格，在`use`关键字前后保留一个空格。

左大括号必须与`function`关键字在同一行，及右大括号必须在主体后独占一行。

在参数列表或者变量列表的左小括号后面不能有空格，在参数列表或者变量列表的右小括号前不能有空格。

在参数列表和变量列表中，在每个逗号钱不能有空格，在每个逗号后面必须有一个空格。

带有默认值的闭包参数必须放置在参数列表的最后面。

一个闭包声明看起来像下面这样。注意小括号、逗号、空格和大括号的位置：

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
	// body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
	// body
};
```

参数列表和变量列表可以跨多行写，每个后续行都要缩进一次。这样做，列表中的第一项必须在下一行，每个参数或变量必须独占一行。

在列表末尾（无论是参数或者变量）跨多行的时候，右小括号和左大括号必须一起占一行，它们之间保留一个空格。

以下是闭包有和没有参数列表和变量列表，跨多行写的例子。

```php
<?php
$longArgs_noVars = function (
	$longArgument,
	$longerArgument,
	$muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
	$longVar1,
	$longerVar2,
	$muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
	$longArgument,
	$longerArgument,
	$muchLongerArgument
) use (
	$longVar1,
	$longerVar2,
	$muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
	$longArgument,
	$longerArgument,
	$muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
	$longVar1,
	$longerVar2,
	$muchLongerVar3
) {
   // body
};
```

注意当闭包直接作为一个函数或者方法调用的参数时，格式化的规则同样适用。

```php
<?php
$foo->bar(
	$arg1,
	function ($arg2) use ($var1) {
		// body
	},
	$arg3
);
```


7. 结论
--------------

有很多风格实践故意被这份指导省略掉，包括但不限于：

- 全局变量和常量的声明

- 函数的声明

- 操作符和赋值

- 对齐

- 注释和文档块

- 类名前缀和后缀

- 最佳实践

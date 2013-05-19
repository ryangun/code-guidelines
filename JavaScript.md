# JavaScript编码指导
A mostly reasonable approach to JavaScript



----------



## 目录

  1. [类型](#types)
  1. [对象](#objects)
  1. [数组](#arrays)
  1. [字符串](#strings)
  1. [函数](#functions)
  1. [属性](#properties)
  1. [变量](#variables)
  1. [提升(Hoisting)](#hoisting)
  1. [条件语句和相等](#conditionals)
  1. [代码块](#blocks)
  1. [注释](#comments)
  1. [空格](#whitespace)
  1. [逗号置于行末](#trailing-commas)
  1. [分号](#semicolons)
  1. [类型转换和强制转换](#type-coercion)
  1. [命名约定](#naming-conventions)
  1. [取值函数和赋值函数](#accessors)
  1. [构造函数](#constructors)
  1. [模块](#modules)
  1. [jQuery](#jquery)
  1. [License](#license)

<h2 id="types">类型</h2>

- **基本类型**： 获取一个基本类型的的时候，你得到的是值本身。

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

```javascript
var foo = 1,
    bar = foo;

bar = 9;

console.log(foo, bar); // => 1, 9
```
- **复合类型**： 获取一个复合类型的时候，你得到是值的引用。

    + `object`
    + `array`
    + `function`

```javascript
var foo = [1, 2],
    bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
```

<h2 id="objects">对象</h2>

- 使用literal语法创建对象。

```javascript
// bad
var item = new Object();

// good
var item = {};
```

- 不要使用[保留字](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Reserved_Words)作为键。

```javascript
// bad
var superman = {
  class: 'superhero',
  default: { clark: 'kent' },
  private: true
};

// good
var superman = {
  klass: 'superhero',
  defaults: { clark: 'kent' },
  hidden: true
};
```

<h2 id='arrays'>数组</h2>

- 使用literal语法创建数组。

```javascript
// bad
var items = new Array();

// good
var items = [];
```

- 不知道数组的长度的时候使用Array#push。

```javascript
var someStack = [];


// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

- 需要复制一个数组的时候使用Array#slice [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)。

```javascript
var len = items.length,
    itemsCopy = [],
    i;

// bad
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
itemsCopy = items.slice();
```

- 将array-like的对象转换成数组时，使用Array#slice。

```javascript
function trigger() {
  var args = Array.prototype.slice.call(arguments);
  ...
}
```


<h2 id="strings">字符串</h2>

- 字符串使用单引号`''`。

```javascript
// bad
var name = "Bob Parr";

// good
var name = 'Bob Parr';

// bad
var fullName = "Bob " + this.lastName;

// good
var fullName = 'Bob ' + this.lastName;
```

- 长于80字符的字符串应该使用字符串连接符`+`跨多行书写。
- 注意：带字符串连接符的长字符串在过长的时候会影响性能。 [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

```javascript
// bad
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
var errorMessage = 'This is a super long error that \
was thrown because of Batman. \
When you stop to think about \
how Batman had anything to do \
with this, you would get nowhere \
fast.';


// good
var errorMessage = 'This is a super long error that ' +
  'was thrown because of Batman.' +
  'When you stop to think about ' +
  'how Batman had anything to do ' +
  'with this, you would get nowhere ' +
  'fast.';
```

- 在构建一个字符串时，使用Array#join代替字符串连接符。尤其在IE下：[jsPerf](http://jsperf.com/string-vs-array-concat/2)

```javascript
var items,
    messages,
    length,
    i;

messages = [{
  state: 'success',
  message: 'This one worked.'
},{
  state: 'success',
  message: 'This one worked as well.'
},{
  state: 'error',
  message: 'This one did not work.'
}];

length = messages.length;

// bad
function inbox(messages) {
  items = '<ul>';

  for (i = 0; i < length; i++) {
    items += '<li>' + messages[i].message + '</li>';
  }

  return items + '</ul>';
}

// good
function inbox(messages) {
  items = [];

  for (i = 0; i < length; i++) {
    items[i] = messages[i].message;
  }

  return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
}
```


<h2 id='functions'>函数</h2>

- 函数表达式：

```javascript
// anonymous function expression
var anonymous = function() {
  return true;
};

// named function expression
var named = function named() {
  return true;
};

// immediately-invoked function expression (IIFE)
(function() {
  console.log('Welcome to the Internet. Please follow me.');
})();
```

- Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
- **注意：** ECMA-262定义了`block`为一系列语句，一个函数声明不是一个语句。 [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)

```javascript
// bad
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
if (currentUser) {
  var test = function test() {
    console.log('Yup.');
  };
}
```

- 永远不要命名一个形参为arguments，这会导致形参arguments的优先级比存在于所有函数作用域的arguments对象高。

```javascript
// bad
function nope(name, options, arguments) {
  // ...stuff...
}

// good
function yup(name, options, args) {
  // ...stuff...
}
```


<h2 id='properties'>属性</h2>

- 获取一个属性的值使用点语法。

```javascript
var luke = {
  jedi: true,
  age: 28
};

// bad
var isJedi = luke['jedi'];

// good
var isJedi = luke.jedi;
```

- 在键为变量时使用`[]`获取属性的值。

```javascript
var luke = {
  jedi: true,
  age: 28
};

function getProp(prop) {
  return luke[prop];
}

var isJedi = getProp('jedi');
```


<h2 id='variables'>变量</h2>

- 一直使用`var`去声明变量，不这样做的话会导致生成全局变量，我们应该避免污染全局命名空间。

```javascript
// bad
superPower = new SuperPower();

// good
var superPower = new SuperPower();
```

- 使用一个`var`声明多个变量，每个变量独占一行。

```javascript
// bad
var items = getItems();
var goSportsTeam = true;
var dragonball = 'z';

// good
var items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';
```

- 最后才声明没有赋值的变量，这在你稍后想通过前面已赋值的变量来赋值给另一个变量是很有用。

```javascript
// bad
var i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
var i, items = getItems(),
    dragonball,
    goSportsTeam = true,
    len;

// good
var items = getItems(),
    goSportsTeam = true,
    dragonball,
    length,
    i;
```

- 在作用域顶部进行变量赋值，这个避免了变量声明提升(hoist)带来的问题。

```javascript
// bad
function() {
  test();
  console.log('doing stuff..');

  //..other stuff..

  var name = getName();

  if (name === 'test') {
    return false;
  }

  return name;
}

// good
function() {
  var name = getName();

  test();
  console.log('doing stuff..');

  //..other stuff..

  if (name === 'test') {
    return false;
  }

  return name;
}

// bad
function() {
  var name = getName();

  if (!arguments.length) {
    return false;
  }

  return true;
}

// good
function() {
  if (!arguments.length) {
    return false;
  }

  var name = getName();

  return true;
}
```


<h2 id='hoisting'>提升(hoisting)</h2>

- 变量声明会被提升(hoist)到它们作用域的顶部，而声明的赋值部分不会被提升。

```javascript
// we know this wouldn't work (assuming there
// is no notDefined global variable)
function example() {
  console.log(notDefined); // => throws a ReferenceError
}

// creating a variable declaration after you
// reference the variable will work due to
// variable hoisting. Note: the assignment
// value of `true` is not hoisted.
function example() {
  console.log(declaredButNotAssigned); // => undefined
  var declaredButNotAssigned = true;
}

// The interpreter is hoisting the variable
// declaration to the top of the scope.
// Which means our example could be rewritten as:
function example() {
  var declaredButNotAssigned;
  console.log(declaredButNotAssigned); // => undefined
  declaredButNotAssigned = true;
}
```

- 匿名函数表达式提升它们的变量名，而不会提升函数赋值。

```javascript
function example() {
  console.log(anonymous); // => undefined

  anonymous(); // => TypeError anonymous is not a function

  var anonymous = function() {
    console.log('anonymous function expression');
  };
}
```

- 命名函数表达式提升变量名，而不会提升函数名和函数体。

```javascript
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  superPower(); // => ReferenceError superPower is not defined

  var named = function superPower() {
    console.log('Flying');
  };


  // the same is true when the function name
  // is the same as the variable name.
  function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    var named = function named() {
      console.log('named');
    };
  }
}
```

- 函数声明提升它们的名称和函数体。

```javascript
function example() {
  superPower(); // => Flying

  function superPower() {
    console.log('Flying');
  }
}
```

- 详细请查看 [Ben Cherry](http://www.adequatelygood.com/) 的 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting)



<h2 id='conditionals'>条件语句和相等</h2>

- 使用`===`和`!==`，替代`==`和`!=`
- 条件语句执行时会使用`ToBoolean`方法进行强制转换，遵循以下简单的规则：

    + **Objects**转换为**true**
    + **Undefined**转换为**false**
    + **Null**转换为**false**
    + **Booleans**转换为**布尔值**
    + **Numbers**为**+0, -0, or NaN**转换为**false**，否则转换为**true**
    + **Strings**为空字符串`''`时转换为**false**，否则转换为**true**

```javascript
if ([0]) {
  // true
  // An array is an object, objects evaluate to true
}
```

- 使用简写。

```javascript
// bad
if (name !== '') {
  // ...stuff...
}

// good
if (name) {
  // ...stuff...
}

// bad
if (collection.length > 0) {
  // ...stuff...
}

// good
if (collection.length) {
  // ...stuff...
}
```

- 详情请查看 Angus Croll 的 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)。


<h2 id='blocks'>代码块</h2>

- 多行代码块使用大括号。

```javascript
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function() { return false; }

// good
function() {
  return false;
}
```


<h2 id='comments'>注释</h2>

- 使用`/** ... */`进行多行注释，包含一个描述、注明所有参数的类型和值及返回值。

```javascript
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param <String> tag
// @return <Element> element
function make(tag) {

  // ...stuff...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed in tag name
 *
 * @param <String> tag
 * @return <Element> element
 */
function make(tag) {

  // ...stuff...

  return element;
}
```

- 使用`//`进行单行注释，在被注释的主体的上一行进行注释，注释行之前保留一个空行。

```javascript
// bad
var active = true;  // is current tab

// good
// is current tab
var active = true;

// bad
function getType() {
  console.log('fetching type...');
  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}

// good
function getType() {
  console.log('fetching type...');

  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}
```

- 在你的注释前加上`FIXME`或者`TODO`帮助其它开发者快速了解你是否指出了一个问题，或者你是否针对问题提出了需要实现的解决方法。这些注释不同与常规的注释，它们代表“可执行”，分别是`FIXME --需要解决`或者`TODO --需要实现`。

- 使用`// FIXME:`注明问题。

```javascript
function Calculator() {

  // FIXME: shouldn't use a global here
  total = 0;

  return this;
}
```

- 使用`// TODO:`注明问题的解决方法。

```javascript
function Calculator() {

  // TODO: total should be configurable by an options param
  this.total = 0;

  return this;
}
```


<h2 id="whitespace">空格</h2>

- 使用两个空格长度的软缩进（使用空格缩进）。

```javascript
// bad
function() {
∙∙∙∙var name;
}

// bad
function() {
∙var name;
}

// good
function() {
∙∙var name;
}
```

- 在左大括号前保留一个空格。

```javascript
// bad
function test(){
  console.log('test');
}

// good
function test() {
  console.log('test');
}

// bad
dog.set('attr',{
  age: '1 year',
  breed: 'Bernese Mountain Dog'
});

// good
dog.set('attr', {
  age: '1 year',
  breed: 'Bernese Mountain Dog'
});
```

- 文件末尾保留一个空行。

```javascript
// bad
(function(global) {
  // ...stuff...
})(this);
```

```javascript
// good
(function(global) {
  // ...stuff...
})(this);
// new line here
```

- 长的方法链时使用缩进。

```javascript
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// good
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();

// bad
var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
    .attr('width',  (radius + margin) * 2).append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);

// good
var leds = stage.selectAll('.led')
    .data(data)
  .enter().append('svg:svg')
    .class('led', true)
    .attr('width',  (radius + margin) * 2)
  .append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);
```

<h2 id='trailing-commas'>逗号置于行末</h2>

- 逗号置于行未。

```javascript
// bad
var once
  , upon
  , aTime;

// good
var once,
    upon,
    aTime;

// bad
var hero = {
    firstName: 'Bob'
  , lastName: 'Parr'
  , heroName: 'Mr. Incredible'
  , superPower: 'strength'
};

// good
var hero = {
  firstName: 'Bob',
  lastName: 'Parr',
  heroName: 'Mr. Incredible',
  superPower: 'strength'
};
```


<h2 id='semicolons'>分号</h2>

- 分号。

```javascript
// bad
(function() {
  var name = 'Skywalker'
  return name
})()

// good
(function() {
  var name = 'Skywalker';
  return name;
})();

// good
;(function() {
  var name = 'Skywalker';
  return name;
})();
```


<h2 id='type-coercion'>类型转换和强制转换</h2>

- 在语句开头执行强制转换。

- 字符串：

```javascript
// => this.reviewScore = 9;

// bad
var totalScore = this.reviewScore + '';

// good
var totalScore = '' + this.reviewScore;

// bad
var totalScore = '' + this.reviewScore + ' total score';

// good
var totalScore = this.reviewScore + ' total score';
```

- 使用`parseInt`对数字进行强制转换，并保证提供基数。
- 出于不得已的原因，`parseInt`成为了瓶颈，你因为[性能原因](http://jsperf.com/coercion-vs-casting/3)需要使用位移，在注释里解释你这样做的原因。

```javascript
var inputValue = '4';

// bad
var val = new Number(inputValue);

// bad
var val = +inputValue;

// bad
var val = inputValue >> 0;

// bad
var val = parseInt(inputValue);

// good
var val = Number(inputValue);

// good
var val = parseInt(inputValue, 10);

// good
/**
 * parseInt was the reason my code was slow.
 * Bitshifting the String to coerce it to a
 * Number made it a lot faster.
 */
var val = inputValue >> 0;
```

- 布尔值：

```javascript
var age = 0;

// bad
var hasAge = new Boolean(age);

// good
var hasAge = Boolean(age);

// good
var hasAge = !!age;
```


<h2 id='naming-conventions'>命名约定</h2>

- 避免使用单个字母命名，让你的命名具有描述性。

```javascript
// bad
function q() {
  // ...stuff...
}

// good
function query() {
  // ..stuff..
}
```

- 使用驼峰命名法命名对象、函数和实例。

```javascript
// bad
var OBJEcttsssss = {};
var this_is_my_object = {};
var this-is-my-object = {};
function c() {};
var u = new user({
  name: 'Bob Parr'
});

// good
var thisIsMyObject = {};
function thisIsMyFunction() {};
var user = new User({
  name: 'Bob Parr'
});
```

- 使用Pascal(PascalCase)命名法命名构造函数或者类。

```javascript
// bad
function user(options) {
  this.name = options.name;
}

var bad = new user({
  name: 'nope'
});

// good
function User(options) {
  this.name = options.name;
}

var good = new User({
  name: 'yup'
});
```

- 命名私有属性时在属性名前加下划线`_`。

```javascript
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';

// good
this._firstName = 'Panda';
```

- 保存一个`this`的引用时使用`_this`。

```javascript
// bad
function() {
  var self = this;
  return function() {
    console.log(self);
  };
}

// bad
function() {
  var that = this;
  return function() {
    console.log(that);
  };
}

// good
function() {
  var _this = this;
  return function() {
    console.log(_this);
  };
}
```

- 给函数命名，有利于堆栈跟踪。

```javascript
// bad
var log = function(msg) {
  console.log(msg);
};

// good
var log = function log(msg) {
  console.log(msg);
};
```


<h2 id='accessors'>取值函数和赋值函数</h2>

- 取、赋值函数是非必须的。
- 如果确实需要取、赋值函数，使用getVal()和setVal()。

```javascript
// bad
dragon.age();

// good
dragon.getAge();

// bad
dragon.age(25);

// good
dragon.setAge(25);
```

- 如果属性值是布尔值，使用isVal()或者hasVal()。

```javascript
// bad
if (!dragon.age()) {
  return false;
}

// good
if (!dragon.hasAge()) {
  return false;
}
```

- 也可以像下面这样使用get()和set()函数。

```javascript
function Jedi(options) {
  options || (options = {});
  var lightsaber = options.lightsaber || 'blue';
  this.set('lightsaber', lightsaber);
}

Jedi.prototype.set = function(key, val) {
  this[key] = val;
};

Jedi.prototype.get = function(key) {
  return this[key];
};
```


<h2 id='constructors'>构造函数</h2>

- 将方法赋值给prototype对象，而不要用新的对象覆盖prototype的值。Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!


```javascript
function Jedi() {
  console.log('new jedi');
}

// bad
Jedi.prototype = {
  fight: function fight() {
    console.log('fighting');
  },

  block: function block() {
    console.log('blocking');
  }
};

// good
Jedi.prototype.fight = function fight() {
  console.log('fighting');
};

Jedi.prototype.block = function block() {
  console.log('blocking');
};
```

- 方法可以返回`this`，帮助链式方法的实现。

```javascript
// bad
Jedi.prototype.jump = function() {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function(height) {
  this.height = height;
};

var luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20) // => undefined

// good
Jedi.prototype.jump = function() {
  this.jumping = true;
  return this;
};

Jedi.prototype.setHeight = function(height) {
  this.height = height;
  return this;
};

var luke = new Jedi();

luke.jump()
  .setHeight(20);
```


- 可以重写toString()方法，只要确保其正常工作及无副作用。

```javascript
function Jedi(options) {
  options || (options = {});
  this.name = options.name || 'no name';
}

Jedi.prototype.getName = function getName() {
  return this.name;
};

Jedi.prototype.toString = function toString() {
  return 'Jedi - ' + this.getName();
};
```


<h2 id='modules'>模块</h2>

- 模块应该以一个`!`开始，这样确保了脚本部署的时候不会因为模块缺少了最后一个分号而报错。
- 文件名用驼峰命名法命名，保存在同名的文件夹，与单一入口的名字匹配。
- 添加一个noConflict()方法，设置exported模块的值为前一个版本并返回当前模块。
- 在模块顶部保持声明`'use strict';`。

```javascript
// fancyInput/fancyInput.js

!function(global) {
  'use strict';

  var previousFancyInput = global.FancyInput;

  function FancyInput(options) {
    this.options = options || {};
  }

  FancyInput.noConflict = function noConflict() {
    global.FancyInput = previousFancyInput;
    return FancyInput;
  };

  global.FancyInput = FancyInput;
}(this);
```


<h2 id='jquery'>jQuery</h2>

- 命名jQuery对象时在对象名前加`$`。

```javascript
// bad
var sidebar = $('.sidebar');

// good
var $sidebar = $('.sidebar');
```

- 缓存jQuery的查找结果。

```javascript
// bad
function setSidebar() {
  $('.sidebar').hide();

  // ...stuff...

  $('.sidebar').css({
    'background-color': 'pink'
  });
}

// good
function setSidebar() {
  var $sidebar = $('.sidebar');
  $sidebar.hide();

  // ...stuff...

  $sidebar.css({
    'background-color': 'pink'
  });
}
```

- 对于DOM查询，使用Cascading `$('.sidebar ul')`或者 parent > child `$('.sidebar > ul')` [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)。
- 对于表示特定范围的jQuery对象使用`find`查询。

```javascript
// bad
$('.sidebar', 'ul').hide();

// bad
$('.sidebar').find('ul').hide();

// good
$('.sidebar ul').hide();

// good
$('.sidebar > ul').hide();

// good (slower)
$sidebar.find('ul');

// good (faster)
$($sidebar[0]).find('ul');
```


<h2 id='license'>License</h2>

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

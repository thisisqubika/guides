# JavaScript Style Guide (EcmaScript 5)

## Table of Contents

* [Types](#types)
* [Objects](#objects)
* [Arrays](#arrays)
* [Strings](#strings)
* [Functions](#functions)
* [Properties](#properties)
* [Variables](#variables)
* [Hoisting](#hoisting)
* [Conditional Expressions and Equality](#conditional-expressions-and-equality)
* [Blocks](#blocks)
* [Comments](#comments)
* [Whitespace](#whitespace)
* [Commas](#commas)
* [Semicolons](#semicolons)
* [Type Casting and Coercion](#type-casting-and-coercion)
* [Naming Conventions](#naming-conventions)
* [Accessors](#accessors)
* [Constructors](#constructors)
* [Events](#events)
* [Modules](#modules)
* [jQuery](#jquery)
* [Standard Features](#standard-features)
* [Resources](#resources)


## Types

  * **Primitives:** When you access a primitive type you work directly on its value
    * string
    * number
    * boolean
    * null
    * undefined

``` javascript
var foo = 1,
    bar = foo;

bar = 9;

console.log(foo, bar); // => 1, 9
```

  * **Complex:** When you access a complex type you work on a reference to its value
    * object
    * array
    * function

``` javascript
var foo = [1, 2],
    bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
```

## Objects

  * Use the literal syntax for object creation.

``` javascript
// bad
var item = new Object();

// good
var item = {};
```

  * Don't use [reverved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8.

``` javascript
// bad
var superman = {
  default: { clark: 'kent' },
  private: true
};

// good
var superman = {
  defaults: { clark: 'kent' },
  hidden: true
};
```

  * Use readable synonyms in place of reserved words.

``` javascript
// bad
var superman = {
  class: 'alien'
};

// bad
var superman = {
  klass: 'alien'
};

// good
var superman = {
  type: 'alien'
};
```

## Arrays

  * Use the literal syntax for array creation

``` javascript
// bad
var items = new Array();

// good
var items = [];
```

  * If you don't know array length use `Array#push`

``` javascript
var someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

  * When you need to copy an array use `Array#slice`

``` javascript
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

  * To convert an array-like object to an array, use Array#slice.

``` javascript
function trigger() {
  var args = Array.prototype.slice.call(arguments);
  ...
}
```

## Strings

  * Use single quotes `' '` for strings

``` javascript
// bad
var name = "Bob Parr";

// good
var name = 'Bob Parr';

// bad
var fullName = "Bob " + this.lastName;

// good
var fullName = 'Bob ' + this.lastName;
```

  * Strings longer than 80 characters should be written across multiple lines using string concatenation.

  * Note: If overused, long strings with concatenation could impact performance.

``` javascript
// bad
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
var errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// good
var errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';
```

  * When programmatically building up a string, use `Array#join` instead of string concatenation. Mostly for IE

``` javascript
var items,
    messages,
    length,
    i;

messages = [{
  state: 'success',
  message: 'This one worked.'
}, {
  state: 'success',
  message: 'This one worked as well.'
}, {
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

## Functions

  * Function expressions:

``` javascript
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

  * Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

  * **Note:** ECMA-262 defines a block as a list of statements. A function declaration is not a statement.

``` javascript
// bad
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
var test;
if (currentUser) {
  test = function test() {
    console.log('Yup.');
  };
}
```
  * Never name a parameter argument, this will take precedence over the arguments object that is given to every function scope.

``` javascript
// bad
function nope(name, options, arguments) {
  // ...stuff...
}

// good
function yup(name, options, args) {
  // ...stuff...
}
```

  * **eval():** Only for code loaders and REPL (Read–eval–print loop)
  * `eval()` makes for confusing semantics and is dangerous to use if the string contains user input. There's usually a better, clearer, and safer way to write your code, so its use is generally not permitted.

## Properties

  * Use dot notation `.` when accessing properties.

``` javascript
var luke = {
  jedi: true,
  age: 28
};

// bad
var isJedi = luke['jedi'];

// good
var isJedi = luke.jedi;
```

  * Use subscript notation `[ ]` when accessing properties with a variable.

``` javascript
var luke = {
  jedi: true,
  age: 28
};

function getProp(prop) {
  return luke[prop];
}

var isJedi = getProp('jedi');
```

  * **Deleting property:** prefer `this.foo = null`

``` javascript
// bad
Foo.prototype.dispose = function() {
  delete this.property_;
};

// good
Foo.prototype.dispose = function() {
  this.property_ = null;
};
```


## Variables

  * Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace.

``` javascript
// bad
superPower = new SuperPower();

// good
var superPower = new SuperPower();
```

  * Use one `var` declaration for multiple variables and declare each variable on a newline.

``` javascript
// bad
var items = getItems();
var goSportsTeam = true;
var dragonball = 'z';

// good
var items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';
```

  * Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

``` javascript
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

  * Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

``` javascript
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


## Hoisting

  * Variable declarations get hoisted to the top of their scope, their assignment does not.

``` javascript
// we know this would not work (assuming there
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

  * Anonymous function expressions hoist their variable name, but not the function assignment.

``` javascript
function example() {
  console.log(anonymous); // => undefined

  anonymous(); // => TypeError anonymous is not a function

  var anonymous = function() {
    console.log('anonymous function expression');
  };
}
```

  * Named function expressions hoist the variable name, not the function name or the function body.

``` javascript
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  superPower(); // => ReferenceError superPower is not defined

  var named = function superPower() {
    console.log('Flying');
  };
}

// the same is true when the function name
// is the same as the variable name.
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  var named = function named() {
    console.log('named');
  }
}
```

  * Function declarations hoist their name and the function body.

``` javascript
function example() {
  superPower(); // => Flying

  function superPower() {
    console.log('Flying');
  }
}
```


## Conditional Expressions and Equality

  * Use `===` and `!==` over `==` and `!=`.
  * Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:
    * **Objects** evaluate to `true`
    * **Undefined** evaluates to `false`
    * **Null** evaluates to `false`
    * **Booleans** evaluate to **the value of the boolean**
    * **Numbers** evaluate to `false` if `+0, -0, or NaN`, otherwise `true`
    * **Strings** if an empty string `''`, otherwise `true`

``` javascript
if ([0]) {
  // true
  // An array is an object, objects evaluate to true
}
```

  * Use shortcuts.

``` javascript
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


## Blocks

  * Use braces with all multi-line blocks.

``` javascript
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


## Comments

  * Use `/** ... */` for multiline comments. Include a description, specify types and values for all parameters and return values.

``` javascript
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

  * Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

``` javascript
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

  * Use `// FIXME: ...` to annotate problems

``` javascript
function Calculator() {

  // FIXME: should not use a global here
  total = 0;

  return this;
}
```

  * Use `// TODO: ...` to annotate solutions to problems

``` javascript
function Calculator() {

  // TODO: total should be configurable by an options param
  this.total = 0;

  return this;
}
```


## Whitespace

  * Use soft tabs set to 2 spaces

``` javascript
// bad
function() {
    var name;
}

// bad
function() {
 var name;
}

// good
function() {
  var name;
}
```

  * Place 1 space before the leading brace.

``` javascript
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

  * Set off operators with spaces.

``` javascript
// bad
var x=y+5;

// good
var x = y + 5;
```

  * End files with a single newline character.

``` javascript
// bad
(function(global) {
  // ...stuff...
})(this);
// bad
(function(global) {
  // ...stuff...
})(this);↵
↵
// good
(function(global) {
  // ...stuff...
})(this);↵
```

  * Use indentation when making long method chains.

``` javascript
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


## Commas

  * Leading commas: **Nope**.

``` javascript
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

  * Additional trailing comma: **Nope**.

``` javascript
// bad
var hero = {
  firstName: 'Kevin',
  lastName: 'Flynn',
};

var heroes = [
  'Batman',
  'Superman',
];

// good
var hero = {
  firstName: 'Kevin',
  lastName: 'Flynn'
};

var heroes = [
  'Batman',
  'Superman'
];
```


## Semicolons

  * Yup.

``` javascript
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

// good (guards against the function becoming an argument when two files with IIFEs are concatenated)
;(function() {
  var name = 'Skywalker';
  return name;
})();
```


## Type Casting and Coercion

  * Perform type coercion at the beginning of the statement.
  * Strings

``` javascript
//  => this.reviewScore = 9;

// bad
var totalScore = this.reviewScore + '';

// good
var totalScore = '' + this.reviewScore;

// bad
var totalScore = '' + this.reviewScore + ' total score';

// good
var totalScore = this.reviewScore + ' total score';
```

  * Use `parseInt` for Numbers and always with a radix for type casting.

``` javascript
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
```

  * If for whatever reason you are doing something wild and parseInt is your bottleneck and need to use bitshift for performance reasons, leave a comment explaining why and what you're doing.

``` javascript
// good
/**
 * parseInt was the reason my code was slow.
 * Bitshifting the String to coerce it to a
 * Number made it a lot faster.
 */
var val = inputValue >> 0;
```

  * **Note:** Be careful when using bitshift operations. Numbers are represented as 64-bit values, but nitshift operations always return a 32-bit integer. bitshift can lead to unexpected behavior for integer values larger than 32 bits. Largest signed 32-bit Int is 2,147,483,647:

``` javascript
2147483647 >> 0 //=> 2147483647
2147483648 >> 0 //=> -2147483648
2147483649 >> 0 //=> -2147483647
```

  * Booleans

``` javascript
var age = 0;

// bad
var hasAge = new Boolean(age);

// good
var hasAge = Boolean(age);

// good
var hasAge = !!age;
```


## Naming Conventions

  * Avoid single letter names. Be descriptive with your naming.

``` javascript
// bad
function q() {
  // ...stuff...
}

// good
function query() {
  // ..stuff..
}
```

  * Use `NAMES_LIKE_THIS` for constant values
  * Never use the const keyword as it's not supported in Internet Explorer.

``` javascript
// bad
var myConstant = 1;

// bad
const  a = 7;

// good
var MY_CONSTANT = 1;
```

  * Use camelCase when naming objects, functions, and instances

``` javascript
// bad
var OBJEcttsssss = {};
var this_is_my_object = {};
function c() {}
var u = new user({
  name: 'Bob Parr'
});

// good
var thisIsMyObject = {};
function thisIsMyFunction() {}
var user = new User({
  name: 'Bob Parr'
});
```

  * Use PascalCase when naming constructors or classes

``` javascript
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

  * Use a leading underscore `_` when naming private properties

``` javascript
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';

// good
this._firstName = 'Panda';
```

  * When saving a reference to `this` use `_this`.

``` javascript
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

  * Name your functions. This is helpful for stack traces.

``` javascript
// bad
var log = function(msg) {
  console.log(msg);
};

// good
var log = function log(msg) {
  console.log(msg);
};
```

## Accessors

  * Accessor functions for properties are not required
  * If you do make accessor functions use `getVal()` and `setVal('hello')`

``` javascript
// bad
dragon.age();

// good
dragon.getAge();

// bad
dragon.age(25);

// good
dragon.setAge(25);
```

  * If the property is a boolean, use `isVal()` or `hasVal()`

``` javascript
// bad
if (!dragon.age()) {
  return false;
}

// good
if (!dragon.hasAge()) {
  return false;
}
```

  * It's okay to create `get()` and `set()` functions, but be consistent.

``` javascript
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


## Constructors

  * Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

``` javascript
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

  * Methods can return `this` to help with method chaining.

``` javascript
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

  * It's okay to write a custom `toString()` method, just make sure it works successfully and causes no side effects.

``` javascript
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


## Events

  * When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event.

For example, instead of:

``` javascript
// bad
$(this).trigger('listingUpdated', listing.id);

...

$(this).on('listingUpdated', function(e, listingId) {
  // do something with listingId
});
```

prefer:

``` javascript
// good
$(this).trigger('listingUpdated', { listingId : listing.id });

...

$(this).on('listingUpdated', function(e, data) {
  // do something with data.listingId
});
```


## Modules

  * The module should start with a `!`. This ensures that if a malformed module forgets to include a final semicolon there aren't errors in production when the scripts get concatenated.
  * The file should be named with PascalCase, live in a folder with the same name, and match the name of the single export.
  * Add a method called `noConflict()` that sets the exported module to the previous version and returns this one.
  * Always declare `'use strict';` at the top of the module.

``` javascript
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


## jQuery

  * Prefix jQuery object variables with a `$`

``` javascript
// bad
var sidebar = $('.sidebar');

// good
var $sidebar = $('.sidebar');
```

  * Cache jQuery lookups.

``` javascript
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

  * For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`.

  * Use find with scoped jQuery object queries.

``` javascript
// bad
$('ul', '.sidebar').hide();

// bad
$('.sidebar').find('ul').hide();

// good
$('.sidebar ul').hide();

// good
$('.sidebar > ul').hide();

// good
$sidebar.find('ul').hide();
```

## Standard Features

  * Always preferred over non-standards features
  * For maximum portability and compatibility, always prefer standards features over non-standards features

``` javascript
// bad
string[3];

// good
string.charAt(3);
```

## Resources

  * [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
  * [Google JavaScript Style Guide](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)

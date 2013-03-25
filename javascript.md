# Lever JavaScript Style Guide

## Table of contents

  1. [Banned features and gotchas](#banned-features-and-gotchas)
  1. [Naming](#naming)
  1. [Whitespace](#whitespace)
  1. [Punctuation](#punctuation)
  1. [Comments](#comments)
  1. [Inline documentation](#inline-documentation)
  1. [Types](#types)
  1. [Conditionals](#conditionals)
  1. [Loops and comprehensions](#loops-and-comprehensions)
  1. [Modules](#modules)
  1. [Resources](#resources)
  1. [License](#license)


## Banned features and gotchas

### Banned features

  - `with`
  - `eval`
  - `new Array`
  - `new Object`
  - `__proto__`
  - Modifying native objects or their prototypes
  - The comma operator, except in `for` loop statements
  - Bit math and clever optimizations

### Gotchas

JavaScript is full of them! Avoid being clever. If you wonder whether something works the way you wrote it, rewrite it with simpler syntax.

Avoid declaring functions inside of a block, such as a conditional. This is undefined behavior, and browsers handle it differently. Instead, assign the function to a variable.

``` javascript
// No
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// Yes
if (currentUser) {
  var test = function test() {
    console.log('Yup.');
  };
}
```

Avoid implicit type coercion, especially with `==` and `!=`. Instead, always use `===` and `!==`. The exception is that `item == null` is OK.

Be careful when adding numbers to explicitly cast to a number type. Otherwise, you might accidentally perform string concatenation if one of the arguments is a string.

Use a null check of `if (value != null)` instead of `if (value)` when `value` can be any number. It is easy to forget that zero is falsey.

`NaN` is the only value in JavaScript that does not equal itself.

All JavaScript numbers are doubles. Be careful when using large integers, doing decimal math, and when casting numbers to and from strings. Bit operators are 32-bit, which can lead to unexpected results.

**[[⬆]](#table-of-contents)**


## Naming

  - `lowerCamelCase` - Local variables and functions as well as public properties or functions
  - `_underscoreCamelCase` - Internal properties or functions
  - `UpperCamelCase` - Constructor functions
  - `SCREAMING_SNAKE_CASE` - Constants
  - `dashed-lower-case` - HTML ids, classes, and attribute names

When camelcasing acronyms, always treat them like words. For example: `htmlSection` or `newHtml`

Avoid [reserved words](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Reserved_Words) as property names.

### Abbreviations

Do not abbreviate variable or function names unless they are explicitly listed here. If an abbreviation is listed here, always use it instead of the full word.

Generic abbreviations:

  - `err` - Error arguments in callbacks and catch
  - `cb` - Callback
  - `i` - Index
  - `e` - HTML event object
  - `el` - HTML element
  - `fn` - Variable representing a function

Abbreviations especially for use in Share and Racer:

  - `op` - Operation
  - `doc` - Data document
  - `v` - Version
  - `txn` - Transaction
  - `db` - Database

When iterators are nested or functions that take a callback are nested, use specific names for the different callbacks or indices.

**[[⬆]](#table-of-contents)**


## Whitespace

  - Two space indentation
  - No trailing line whitespace
  - No whitespace on empty lines
  - Has extra linebreak at end of file

### Indentation

Indent statements continued between lines one level (two spaces). Place operators that continue a line at the end of the line.

``` javascript
greeting = (isNew) ?
  'Welcome!' :
  'Welcome back!';

farewell = 'Come back to our place soon ' +
  user.firstName;
```

In chained method calls that don't fit on a single line, place each call on a separate line and indented by one level, with a leading `.`.

``` javascript
expressApp
  .use(express.logger())
  .use(express.favicon())
  .use(expressApp.router);
```

Do not vertically align items in consecutive lines.

``` javascript
// No
var x        = 0;
var y        = 0;
var position = 'top';

// No
var magicWords = [
                   'abracadabra'
                   'gesundheit'
                   'ventrilo'
                 ];

// No
var message = (regExp.test(text)) ?
              'You have a match!' :
              'No match';

// Yes
var x = 0;
var y = 0;
var position = 'top';

// Yes
var magicWords = [
  'abracadabra'
  'gesundheit'
  'ventrilo'
];

// Yes
var message = (regExp.test(text)) ?
  'You have a match!' :
  'No match';
```

**[[⬆]](#table-of-contents)**


## Punctuation

Prefer single quoted strings (`''`) instead of double quoted (`""`) strings.

**[[⬆]](#table-of-contents)**


## Comments

Capitalize the first word of the comment, unless the first word is an identifier that begins with a lower-case letter. If a comment is short, omit the period at the end. Use backticks to quote identifiers inside of comment descriptions.

### Block Comments

Block comments apply to the block of code that follows them.

Each line of a block comment starts with a `//` and a single space, and should be indented at the same level of the code that it describes.

Paragraphs inside of block comments are separated by a line containing `//`.

``` javascript
// This is a block comment. Note that if this were a real block
// comment, we would actually be describing the proceeding code.
//
// This is the second paragraph of the same block comment. Note
// that this paragraph was separated from the previous paragraph
// by a line containing a single comment character.
start();
stop();
```

### Inline Comments

Place inline comments on the line immediately above the statements they are describing. Do not place inline comments on the same line unless it aids in clarity, such as documenting a multi-line regular expression.

The use of inline comments should be limited, because their existence is typically a sign of a code smell. Especially do not use inline comments when they state the obvious.

``` javascript
// No

// Increment x
x = x + 1;
```

However, inline comments can be useful in certain scenarios:

``` javascript
// Yes

// Compensate for border
x = x + 1;
```

### Annotations

Use annotations when necessary to describe a specific action that must be taken against the indicated block of code.

Write the annotation on the line immediately above the code that the annotation is describing.

Follow the annotation keyword by a colon and a space, and a descriptive note.

```javascript
// FIXME: The client's current state should *not* affect payload processing.
resetClientState();
processPayload();
```

If multiple lines are required by the description, indent subsequent lines with two spaces:

```javascript
// TODO: Ensure that the value returned by this call falls within a certain
//   range, or throw an exception.
analyze();
```

Annotation types:

  - `TODO`: describe missing functionality that should be added at a later date
  - `FIXME`: describe broken code that must be fixed
  - `HACK`: describe the use of a questionable coding practice
  - `OPTIMIZE`: describe code that is inefficient and may become a bottleneck
  - `REVIEW`: describe code that should be reviewed to confirm implementation

**[[⬆]](#table-of-contents)**


## Inline documentation

Use [JSDoc](https://github.com/jsdoc3/jsdoc) to document functions and types inline. All public interfaces should be annotated.

  - [JSDoc documentation](http://usejsdoc.org/)
  - [Annotating JavaScript for the Google Closure Compiler](https://developers.google.com/closure/compiler/docs/js-for-compiler)

Installing [DocBlockr](https://github.com/spadgos/sublime-jsdocs) is recommended for Sublime Text.

``` javascript
/**
 * A tasty treat with a hole
 * @param {string} flavor Name of the flavor
 * @constructor
 */
function Donut(flavor) {
  this.flavor = flavor;
}

/**
 * Decorate a donut
 * @param {Donut} donut The donut to decorate
 * @param {Object} [options] Options for decorating the donut
 * @param {boolean} [options.needsSprinkles] Whether the donut needs sprinkles
 * @param {boolean} [options.needsGlaze] Whether the donut needs glaze
 * @return {Donut} The decorated donut
 */
function decorateDonut(donut, options) {
  // ...
}
```

**[[⬆]](#table-of-contents)**


## Types

V8 and modern JavaScript compilers can better optimize code when objects follow more regular type structures. In addition, Chrome's memory profiling tools provide more rapid debugging and more useful insights when objects are of named types.

Thus, when objects are created with a regular set of properties, using a constructor is preferred to object literals. This is especially important for long-lived objects and objects that act as maps, since these are more likely to be involved in memory leaks. However, using object literals is OK for boxing up multiple arguments to get immediately parsed out and irregular blobs of data.

For performance reasons, the properties of a typed object should not be changed after instantiation. New properties should not be added dynamically, and properties should not be removed with a `delete` statement. Instead, properties that are only sometimes existant should be initialized to `null` in the constructor and cleared via a set to `null`. Adding properties dynamically and `delete` should ideally only be used on objects that act as maps.

Arrays should be avoided as containers of mixed values, and they should only be used to contain lists of a single type.

``` javascript
// No

// Constructors are preferred even for singleton objects and simple map
// objects. This gives them type names in the memory profiler
var library = {
  isbnMap: {}
};

// Prototypes are preferred to dynamically adding methods
library.add = function(book) {
  this.isbnMap[book.isbn] = book;
  // Avoid dynamically adding or removing properties of a typed object
  book.currentLibrary = this;
};

// A constructor should be used for greater efficiency, better profiling,
// and to make it more explicit what are the optional and required fields
library.add({
  isbn: '978-1470178192'
, title: 'Moby Dick'
, author: 'Herman Melville'
});

// Yes

function Book(isbn, options) {
  options || (options = {});
  this.isbn = isbn;
  this.title = options.title;
  this.author = options.author;
  // Make sure to initialize all properties that a typed object can have in
  // the constructor. Set properties to `null` if there is no default value
  this.currentLibrary = null;
}

function IsbnMap() {}

function Library() {
  this.isbnMap = new IsbnMap;
}

Library.prototype.add = function(book) {
  this.isbnMap[book.isbn] = book;
  book.currentLibrary = this;
}

var library = new Library;

// This options object literal is fine, because it is immediately parsed
// out and can be easily garbage collected
var book = new Book('978-1470178192', {
  title: 'Moby Dick'
, author: 'Herman Melville'
});
library.add(book);
```

**[[⬆]](#table-of-contents)**


## Conditionals

**[[⬆]](#table-of-contents)**


## Loops and comprehensions

**[[⬆]](#table-of-contents)**


## Modules

Use the Node.js module convention for both server and client code. Each module is a file. Modules may export an object with stateless methods, a single function, or a constructor.

### Requires

Place require statements at the top of a file. Group them in the following order:

  1. Standard library imports
  2. Third party library imports
  3. Local imports

When using imported functions, don't destructure them at the top of the file. This can make it harder for others to figure out the context of the function when they are reading the code later.

``` javascript
// No
{join} = require 'path'
join __dirname, '/foo'

// Yes
path = require 'path'
path.join __dirname, '/foo'
```

### Modules that export an object

Modules that export an object have a camel case name starting with a lowercase letter. Where either singular or plural would make sense, pluaral names are preferred.

They only export stateless functions and global constants shared among all instances of the module. Public exported functions start with a lowercase letter, and functions exported for internal use start with an underscore.

Modules that export an object only use `exports` and they do NOT use `module.exports`. All exported functions are exported where they are defined. Use of exported functions in the same file also refer to the function as `exports.xxx`.

**`fruits.coffee`**

``` javascript
exports.TIMEOUT = 1000

exports.ripen = (fruit, amount) ->
  fruit.ripeness *= amount

exports.ripenEach = (fruits, amount) ->
  for fruit in fruits
    exports.ripen fruit, amount
  return
```

### Modules that export a function or constructor

Modules that export a function or constructor are named the same thing as the function they export. Modules exporting a constructor start with an uppercase character.

They always export using `exports = module.exports =`.

**`Fruit.coffee`**

``` javascript
exports = module.exports = (@name) ->
  @ripeness = 0
  return

exports::isRipe = ->
  return @ripeness > 0.5
```

**[[⬆]](#table-of-contents)**


## Resources

### Read this

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

### Other styleguides

  - [npm Coding Style](https://npmjs.org/doc/coding-style.html)
  - [Felix's Node.js Style Guide](http://nodeguide.com/style.html)
  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery JavaScript Style Guide](http://contribute.jquery.org/style-guide/js/)
  - [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)
  - [Crockford's Code Conventions for the JavaScript Programming Language](http://javascript.crockford.com/code.html)
  - [Dojo Style Guide](http://dojotoolkit.org/community/styleGuide)

### Performance

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)

### Books

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch

### Blogs

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**[[⬆]](#table-of-contents)**


## License

(The MIT License)

Copyright (c) 2013 Lever

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


This document forked from the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript).

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

**[[⬆]](#table-of-contents)**

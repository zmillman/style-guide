# Lever JavaScript Style Guide

## Table of contents

  1. [Banned features](#banned-features)
  1. [Naming, whitespace, and puctuation](#naming-whitespace-and-puctuation)
  1. [Complexity](#complexity)
  1. [Modules](#modules)
  1. [Modules](#modules)
  1. [Loops](#loops)
  1. [Resources](#resources)
  1. [License](#license)


## Banned features and gotchas

### Banned features

  - Coffee classes
  - with
  - eval
  - new Array
  - new Object
  - __proto__
  - switch / case

Do not ever modify native objects or their prototypes.

### Gotchas

CoffeeScript implicitly returns from every function and statement. This is a problem for constructor functions (which shouldn't return anything) and functions that might produce complex return values by accident. To avoid this problem, always write an explicit return at the end of function constructors and functions that end in a loop or conditional.

``` coffeescript
## BAD ##

Color = (@name, @hex) ->
  @rgb = hexToRgb hex

eatEach = (items) ->
  for item in items
    eat item

## Good ##

Color = (@name, @hex) ->
  @rgb = hexToRgb hex
  return

eatEach = (items) ->
  for item in items
    eat item
  return
```

**[[⬆]](#table-of-contents)**


## Naming, whitespace, and puctuation

### Naming

  - *lowerCamelCase* - Local variables and functions as well as public properties or functions
  - *_underscoreCamelCase* - Internal properties or functions
  - *UpperCamelCase* - Constructor functions
  - *SCREAMING_SNAKE_CASE* - Constants
  - *dashed-lower* - HTML ids, classes, and attribute names

When camelcasing acronyms, always treat them like words. For example: `htmlSection` or `newHtml`

### Whitespace

  - Two space indentation
  - No vertical alignment between lines
  - No trailing line whitespace
  - No whitespace on empty lines
  - Has extra linebreak at end of file

### Punctuation

  - Don't use parentheses around non-nested function calls, and always do use them when a function call is part of another expression

**[[⬆]](#table-of-contents)**


## Complexity

### Lines

Maximum line length is 80 characters. A line performs one logical step, and nested function calls are minimized.

### Files

Most files are about 10 to 500 lines. They are organized around objects, and file names are nouns.

### Functions

Functions are usually less than 10 lines and only accomplish one task. Closures are rarely nested more than one level. Functions are generally not defined within conditionals.

**[[⬆]](#table-of-contents)**


## Modules

We use the Node.js module convention wherever possible for both server and client code. Each module is a file. Modules export either an object or a constructor, and they do not otherwise export a single function.

### Modules that export an object

Modules that export an object have a camel case name starting with a lowercase letter. Where either singular or plural would make sense, pluaral names are preferred.

They only export stateless functions and global constants shared among all instances of the module. Public exported functions start with a lowercase letter, and functions exported for internal use start with an underscore.

Modules that export an object only use `exports` and they do NOT use `module.exports`. All exported functions are exported where they are defined. Use of exported functions in the same file also refer to the function as `exports.xxx`.

`fruits.coffee`

``` coffeescript
exports.TIMEOUT = 1000

exports.ripen = (fruit, amount) ->
  fruit.ripeness *= amount

exports.ripenEach = (fruits, amount) ->
  for fruit in fruits
    exports.ripen fruit, amount
  return
```

### Modules that export a constructor

Modules that export a constructor are named the same thing as the constructor, and they start with an uppercase character.

They only export a single function using `module.exports`.

`Fruit.coffee`

```
module.exports = Fruit(@name) ->
  @ripeness = 0
  return

Fruit::isRipe = ->
  return @ripeness > 0.5
```

**[[⬆]](#table-of-contents)**


## Resources

See [JavaScript resources](javascript.md#resources).

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

**[[⬆]](#table-of-contents)**

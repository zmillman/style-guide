# Lever CoffeeScript Style Guide

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

  - Coffee classes
  - `with`
  - `eval`
  - `new Array`
  - `new Object`
  - `__proto__`
  - Modifying native objects or their prototypes

### Gotchas

CoffeeScript implicitly returns from every function and statement. This is a problem for constructor functions (which shouldn't return anything) and functions that might produce complex return values by accident. To avoid this problem, always write an explicit `return` at the end of function constructors and functions that end in a loop or conditional.

``` coffeescript
# No
Color = (@name, @hex) ->
  @rgb = hexToRgb hex

# No
eatEach = (items) ->
  for item in items
    eat item

# Yes
Color = (@name, @hex) ->
  @rgb = hexToRgb hex
  return

# Yes
eatEach = (items) ->
  for item in items
    eat item
  return
```

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

``` coffeescript
greeting = if isNew
    'Welcome!'
  else
    'Welcome back!'

farewell = 'Come back to our place soon ' +
  user.firstName
```

In chained method calls that don't fit on a single line, place each call on a separate line and indented by one level, with a leading `.`.

```coffeescript
[1..3]
  .map((x) -> x * x)
  .concat([10..12])
  .filter((x) -> x < 11)
  .reduce((x, y) -> x + y)
```

Do not vertically align items in consecutive lines.

``` coffeescript
# No
x        = 0
y        = 0
position = 'top'

# No
magicWords = [
               'abracadabra'
               'gesundheit'
               'ventrilo'
             ]

# No
message = if regExp.test text
            'You have a match!'
          else
            'No match'

# Yes
x = 0
y = 0
position = 'top'

# Yes
magicWords = [
  'abracadabra'
  'gesundheit'
  'ventrilo'
]

# Yes
message = if regExp.test text
    'You have a match!'
  else
    'No match'
```

**[[⬆]](#table-of-contents)**


## Punctuation

Prefer single quoted strings (`''`) instead of double quoted (`""`) strings, unless string interpolation is being used for the given string.

Omit parentheses around function calls that begin an expression. However, when nesting or at all ambiguous, use parentheses for clarity.

``` coffeescript
# Parentheses omitted

app.get '/', (req, res) ->
  res.send 'Hi!'

launch spaceship, coordinates

name = model.get 'name'

return cb? err if err

# Parentheses included

out = Math.max 0, Math.min(1, value)

text = prefix + model.get('message')
```

Do not use Lisp-like function grouping style.

``` coffeescript
# No
text = prefix + (model.get 'message')
```

Do not use parentheses when declaring functions that accept no arguments.

``` coffeescript
# No
bar = () ->
# Yes
bar = ->
```

Use curly braces when creating object literals on a single line. While this is optional in CoffeScript, we think it is generally more clear and readable to include the curly braces.

``` coffeescript
# No
singers = Jagger: "Rock", Elvis: "Roll"
client = createClient port: 3000

# Yes
singers = {Jagger: "Rock", Elvis: "Roll"}
client = createClient {port: 3000}
```

Use `@property` instead of `this.property`.

``` coffeescript
# No
this.property
# Yes
@property
```

However, avoid using **standalone** `@`:

``` coffeescript
# No
return @
# Yes
return this
```

Prefer shorthand notation (`::`) for accessing an object's prototype:

``` coffeescript
# No
Array.prototype.slice
# Yes
Array::slice
```

**[[⬆]](#table-of-contents)**


## Comments

The first word of the comment should be capitalized, unless the first word is an identifier that begins with a lower-case letter. If a comment is short, the period at the end can be omitted.

### Block Comments

Block comments apply to the block of code that follows them.

Each line of a block comment starts with a `#` and a single space, and should be indented at the same level of the code that it describes.

Paragraphs inside of block comments are separated by a line containing a single `#`.

``` coffeescript
# This is a block comment. Note that if this were a real block
# comment, we would actually be describing the proceeding code.
#
# This is the second paragraph of the same block comment. Note
# that this paragraph was separated from the previous paragraph
# by a line containing a single comment character.
start()
stop()
```

### Inline Comments

Place inline comments on the line immediately above the statements they are describing. Do not place inline comments on the same line unless it aids in clarity, such as documenting a multi-line regular expression.

The use of inline comments should be limited, because their existence is typically a sign of a code smell. Especially do not use inline comments when they state the obvious.

``` coffeescript
# No

# Increment x
x = x + 1
```

However, inline comments can be useful in certain scenarios:

``` coffeescript
# Yes

# Compensate for border
x = x + 1
```

### Annotations

Use annotations when necessary to describe a specific action that must be taken against the indicated block of code.

Write the annotation on the line immediately above the code that the annotation is describing.

Follow the annotation keyword by a colon and a space, and a descriptive note.

```coffeescript
# FIXME: The client's current state should *not* affect payload processing.
resetClientState()
processPayload()
```

If multiple lines are required by the description, indent subsequent lines with two spaces:

```coffeescript
# TODO: Ensure that the value returned by this call falls within a certain
#   range, or throw an exception.
analyze()
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

In CoffeeScript, block comments that get passed through use triple-hashes, so a JSDoc annoation in CoffeeScript looks like:

``` coffeescript
###*
 * A tasty treat with a hole
 * @param {string} flavor Name of the flavor
 * @constructor
###
Donut = (@flavor) ->

###*
 * Decorate a donut
 * @param {Donut} donut The donut to decorate
 * @param {Object} [options] Options for decorating the donut
 * @param {boolean} [options.needsSprinkles] Whether the donut needs sprinkles
 * @param {boolean} [options.needsGlaze] Whether the donut needs glaze
 * @return {Donut} The decorated donut
###
decorateDonut = (donut, options = {}) ->
  # ...
```

### Return values

CoffeeScript implicitly returns the last line of a function. For clarity, explicitly write out `return` if a function does not fit on a single line and its return value is important.

**[[⬆]](#table-of-contents)**


## Types

V8 and modern JavaScript compilers can better optimize code when objects follow more regular type structures. In addition, Chrome's memory profiling tools provide more rapid debugging and more useful insights when objects are of named types.

Thus, when objects are created with a regular set of properties, using a constructor is preferred to object literals. This is especially important for long-lived objects and objects that act as maps, since these are more likely to be involved in memory leaks. However, using object literals is OK for boxing up multiple arguments to get immediately parsed out and irregular blobs of data.

For performance reasons, the properties of a typed object should not be changed after instantiation. New properties should not be added dynamically, and properties should not be removed with a `delete` statement. Instead, properties that are only sometimes existant should be initialized to `null` in the constructor and cleared via a set to `null`. Adding properties dynamically and `delete` should ideally only be used on objects that act as maps.

Arrays should be avoided as containers of mixed values, and they should only be used to contain lists of a single type.

``` coffeescript
# No

# Constructors are preferred even for singleton objects and simple map
# objects so that they have type names in the memory profiler
library =
  isbnMap: {}

# Prototypes are preferred to dynamically adding methods
library.add = (book) ->
  @isbnMap[book.isbn] = book
  # Avoid dynamically adding or removing properties of a typed object
  book.currentLibrary = this

# A constructor should be used for greater efficiency, better profiling,
# and to make it more explicit what are the optional and required fields
library.add {
  isbn: '978-1470178192'
  title: 'Moby Dick'
  author: 'Herman Melville'
}

# Yes

Book = (@isbn, options = {}) ->
  @title = options.title
  @author = options.author
  # Make sure to initialize all properties that a typed object can have in
  # the constructor. Set properties to `null` if there is no default value
  @currentLibrary = null
  return

IsbnMap = ->

Library = ->
  @isbnMap = new IsbnMap
  return

Library::add = (book) ->
  @isbnMap[book.isbn] = book
  book.currentLibrary = this

library = new Library

# This options object literal is fine, because it is immediately parsed
# out and can be easily garbage collected
library.add new Book '978-1470178192', {
  title: 'Moby Dick'
  author: 'Herman Melville'
}
```

**[[⬆]](#table-of-contents)**


## Conditionals

Always use `if` instead of `unless` when there is an `else`.

``` coffeescript
# No
unless false
  ...
else
  ...

# Yes
if true
  ...
else
  ...
```

**[[⬆]](#table-of-contents)**


## Loops and comprehensions

Prefer comprehensions for map and filter tasks.

``` coffeescript
# No
results = []
for item in array
  results.push item.name

# No
results = array.map (item) -> item.name

# Yes
results = (item.name for item in array)
```

Use for loops and not Array#forEach in cases where a closure wrapper is not needed.

``` coffeescript
# No
items.forEach (item) ->
  console.log item

# Yes
for item in items
  console.log item
```

If a closure wrapper is needed when iterating over an array, use Array#forEach instead of the CoffeeScript `do` keyword.

``` coffeescript
# No
for filename in files
  do (filename) ->
    fs.readFile filename, (err, contents) ->
      compile filename, contents.toString()

# Yes
files.forEach (filename) ->
  fs.readFile filename, (err, contents) ->
    compile filename, contents.toString()
```

**[[⬆]](#table-of-contents)**


## Modules

Use the Node.js module convention for both server and client code. Each module is a file. Modules may export an object with stateless methods, a single function, or a constructor.

### Requires

Place require statements at the top of a file. Group them in the following order:

  1. Standard library imports
  2. Third party library imports
  3. Local imports

When using imported functions, don't destructure them at the top of the file. This can make it harder for others to figure out the context of the function when they are reading the code later.

``` coffeescript
# No
{join} = require 'path'
join __dirname, '/foo'

# Yes
path = require 'path'
path.join __dirname, '/foo'
```

### Modules that export an object

Modules that export an object have a camel case name starting with a lowercase letter. Where either singular or plural would make sense, pluaral names are preferred.

They only export stateless functions and global constants shared among all instances of the module. Public exported functions start with a lowercase letter, and functions exported for internal use start with an underscore.

Modules that export an object only use `exports` and they do NOT use `module.exports`. All exported functions are exported where they are defined. Use of exported functions in the same file also refer to the function as `exports.xxx`.

**`fruits.coffee`**

``` coffeescript
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

``` coffeescript
exports = module.exports = (@name) ->
  @ripeness = 0
  return

exports::isRipe = ->
  return @ripeness > 0.5
```

**[[⬆]](#table-of-contents)**


## Resources

### Read this

  - [CoffeeScript Documentation](http://coffeescript.org/)

### Style guides

  - [Polar Mobile CoffeeScript Style Guide](https://github.com/polarmobile/coffeescript-style-guide)


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

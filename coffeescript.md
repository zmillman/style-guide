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

See [JavaScript Style Guide](javascript.md#banned-features).

In addition, do not use CoffeeScript classes.

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

See [JavaScript Style Guide](javascript.md#naming).

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

See [JavaScript Style Guide](javascript.md#comments).

Rules are the same, except that `#` is used instead of `//`.

**[[⬆]](#table-of-contents)**


## Inline documentation

See [JavaScript Style Guide](javascript.md#inline-documentation).

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

See [JavaScript Style Guide](javascript.md#types).

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

See [JavaScript Style Guide](javascript.md#modules).

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

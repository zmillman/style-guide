# Lever JavaScript Style Guide

## Table of contents

  1. [File structure](#file-structure)
  1. [Resources](#resources)
  1. [License](#license)

## File structure

**Primitives**: When you access a primitive type you work directly on its value

  - `string`
  - `number`
  - `boolean`
  - `null`
  - `undefined`

```javascript
var foo = 1,
    bar = foo;

bar = 9;

console.log(foo, bar); // => 1, 9
```

**Complex**: When you access a complex type you work on a reference to its value

  - `object`
  - `array`
  - `function`

```javascript
var foo = [1, 2],
    bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
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

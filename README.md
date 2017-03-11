![Vult](http://modlfo.github.io/vult/images/VultH.png?raw=true "Vult")

A node.js module providing all the functionality of the Vult compiler.

You can find the full project in https://github.com/modlfo/vult

Tutorials and documentation can be found the main site http://modlfo.github.io/vult/

# Usage

```
var vult = require('vultlib')

// Example to generate C/C++ code
var result = vult.generateC([{ file : 'test.vult', code : 'fun foo() return 0;' }], { output : "Test" });

```

# API

### Types (description of the objects)

#### Input and output files
Files are represented with an object containing the fields `file` and (optionally) `code`.
```
input_file =
   {
      file : String, // required
      code : String  // optional
   }
```
For example:

```
var test1 = { file : 'test.vult', code : 'fun foo() return 0;' }
```
`test` is an input file that provides it's contents and there's no need to read it.

Contrary to:

```
var test2 = { file : 'test.vult' }
```

when `test2` is given as input, the file `'test.vult'` will be read from the file system.

The generated code is returned in the same form:
```
{
   file : "foo.cpp",      // File name or extension
   code : ".. my code .." // Generated code
}
```

#### Code generation options

This object is expected by all function that generate code (Js, C++, or Lua).
```
options =
   {
      output   : String,
      real     : String,
      template : String,
      includes : String Array
   }
```
All the fields are optional. For example:
```
var options = { output : 'Main', real : 'fixed', includes = ['/home/leonardo'] }
```

These options correspond to the command line options shown in [Command Line Options](https://github.com/modlfo/vult/wiki/Command-Line-Options).

#### Errors
When errors in the code are found they are returned in the following form:
```
error =
   {
      msg  : String,
      file : String,
      line : Number,
      col  : Number
   }
```

For example:


### Methods

#### version()
Returns a string with the version of the compiler

#### generateC(files, options)
#### generateJs(files, options)
#### generateLua(files, options)

Takes an array of input files and options and generates the corresponding code.

The input and output `files` are in the form described above (`{ file : 'name'} ` or `{ file : 'name', code : '...' }`).

The `options` are described above as well.

IMPORTANT: if no special options are required you need to pass an empty object `{}`.

The output will be either an array of output `file` objects or an array of errors.

### Examples
```
// Read the file 'test.vult' and generate C code
var result1 = vult.generateC([{ file : 'test.vult'}], {});

// Generate C code out of the given input vult code
var result2 = vult.generateC([{ file : 'test.vult', code : "fun foo() return "}], {});

// Printing 'result2' will show:
console.log(result2);
/*
[ { file: '.h',
    code: '#ifndef VULT_H\n#define VULT_H\n#include <stdint.h>\n#include <math.h>\n#include "vultin.h"\n\nint Live_foo();\n\n\n\n#endif // VULT_H\n' },
 { file: '.cpp',
    code: '#include "Vult.h"\n\nint Live_foo(){\n   return 0;\n}\n\n\n' } ]
*/

// The input code has an error
var errors = vult.generateC([ { code : "fun foo() { return 0;", file : "test.vult" } ], {})

// The result will be instead:
console.log(errors);
/*
[ { msg: 'Expecting a \'}\' but the file ended\nfun foo() { return 0; \n                     ^\n',
    file: '',
    line: 1,
    col: 21 } ]
*/

```

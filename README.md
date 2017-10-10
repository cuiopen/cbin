cbin
====

[![npm version](https://img.shields.io/npm/v/cbin.svg)](https://www.npmjs.com/package/cbin)

`cbin` provides a simple interface for reading and writing binary data in JavaScript.

Usage
-----

There are two classes, `Reader` and `Writer`, with 3 constructor parameters:

- `data` (required), a `Uint8Array` accessed through the class.
- `pos` (optional), the current byte position (default is 0).
- `endian` (optional), either `Endian.little` (0, the default) or `Endian.big` (1).

The parameters become correspondingly named class members.

Accessing the members directly is fine, and the preferred way to access individual bytes.
More convenient methods are provided for longer data types:

- `u32` for unsigned 32-bit integer.
- `f64` for 64-bit IEEE double precision float.

The methods on `Reader` take no parameters. On `Writer` the only parameter is the value to write.

Both methods update the `pos` member. The methods on `Writer` return `this`, which allows chaining them.

Example:

```TypeScript
const { Reader, Writer, Endian } = require('cbin');

const writer = new Writer(new Uint8Array(13), Endian.big);

writer.data[writer.pos++] = 0;
writer.u32(0x12345678).f64(-1);

// Prints: 0012345678bff0000000000000
console.log(Buffer.from(writer.data).toString('hex'));

const reader = new Reader(writer.data, Endian.big, 5);

// Prints: -1
console.log(reader.f64());
```

License
=======

[The MIT License](https://raw.githubusercontent.com/charto/cbin/master/LICENSE)

Copyright (c) 2017 BusFaster Ltd

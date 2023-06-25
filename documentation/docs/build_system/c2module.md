
For code to insert information like the git version, compiler version etc,
there is a special module called `c2`. This pseudo-module exports symbols that contain
this information and can be easily used like:

```c
module hello;

import stdio as io;
import c2;

public func i32 main(i32 argc, char** argv) {
    io.printf("c2c version is %s\n", c2.version);
    return 0;
}
```
No need for dirty macros, code-generation scripts etc...

In addition, the C2 module also holds some commonly used symbols. This way, no symbols
have to be placed at the global namespace and thus a naming conflict can always be
resolved by the programmer.

See [Basic types](../language/basic_types.md) for a list of symbols.

The C2 module also contains several C types used to map C functions to C2's interface files.
See [External Libraries](libraries/) for information on the types and interface files.


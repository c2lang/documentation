
For code to insert information like the git version, build time, compiler version etc,
there is a special module called `c2`. This pseudo-module exports symbols that contain
this information and can be easily used like:

```c
module hello;

import stdio as io;
import c2;

public func int32 main(int32 argc, char*[] argv) {
    io.printf("buildtime is %s\n", c2.buildtime);
    io.printf("c2c version is %s\n", c2.version);
    return 0;
}
```
No need for dirty macros, code-generation scripts etc...

In addition, the C2 module holds some commonly used symbols. This way, no symbols
have to be placed at the global namespace and thus naming conflict can always be
resolved by a programmer.

See [Basic types](../language/basic_types.md) for a list of symbols.



Enough philosophy, let's talk code, let's talk Hello world...

`hello.c2`
```c
module hello_world;

import stdio as io;

func int32 main(int32 argc, char*[] argv) {
    io.printf("Hello World!\n");
    return 0;
}
```

Spot the __six__ differences.. (with C)
Scroll down for the answer

.

.

.

.

.

.

.

.

Here they are:

1. __module__ keyword, see [modules](../language/modules.md)
2. __import__ replaced #include, see [Import](../language/modules.md#import)
3. __func__ keyword precedes all functions
4. __int32__ instead of int. In C2 you always specify the size
5. __char*[] argv__ instead of __char* argv[]__. Types are continuous.
6. __io.printf__, symbols are inside modules.


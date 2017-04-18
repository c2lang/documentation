## modules

Like many other modern languages, C2 uses the concept of `modules`. Modules are
used to create logical groups of functions, types and variables. Each __.c2__
file has to start with the __module__ keyword, specifying which module it belongs
to. A module can consist of several files, for example:

`file1.c2`
```c
module foo;

// ..
```

`file2.c2`
```c
module foo;

// ..
```

`file3.c2`
```c
module bar;

// ..
```

file1.c2 and file2.c2 belong to the same module, `foo`, while file3.c2 belongs to
the module `bar`.

Modules allow the developer to split functionality into several files, *without*
sacrificing speed or introducing extra complexity.

## import
For a file to use declarations from another module, it must __import__ said module.
The __import__ keyword has a file scope. Therefore, if `file1.c2` imports module `stdio`,
`file2.c2` still cannot use `stdio`'s symbols, unless it imports `stdio` as well.

`file1.c2`
```c
module foo;

//import file and storage
import file;
import storage;
// ..
```

`file2.c2`
```c
module foo;

//file and net imported, but not storage
import file;
import net;
// ..
```

So `file1.c2` can use external symbols from `file` and `storage`, while `file2.c2`
can only use symbols from `file` and `net`.

One big advantage of not using C-style header-file includes is that no filenames will
ever appear in the code. This means renaming/moving the files of modules requires
**NO** change to other code whatsoever, even if said code uses the module itself!! Powerful stuff.

## symbol visibility

All files of the same module have access to all the declarations of that module. Other
modules can only use the __public__ ones.

`file.c2`
```c
module foo;

public func void init() { .. }

func void open() { .. }
```

In this example, the other modules can use the `init()` function after importing foo, but
only files in the `foo` module can use `open()`.

## symbol resolving
To access symbols of other modules, the module prefix is added, followed by a dot:

```c
module foo;

import one;
import two;

func void test() {
    one.test();
    two.test();
    open();
}
```
A global symbol is only allowed ONCE per module, whether __public__ or not. This makes
`module.symbol` unique. In the example above, all three modules have the
`test` symbol, but no clash occurs, since it is always apparent from which module which symbol comes.

### import as
When importing a module with a long name, C2 allows aliasing the module name (applied to the
current file only!), like this:

```c
module foo;

import extended_filesystems_io as fs;
```
external symbols can now use the shorter `fs` prefix. Even after aliasing, using the full name is still allowed!

### import local
When using a lot of external symbols, the constant prefixing can be cumbersome. C2
allows using the external symbols of a module directly (without any prefix) when the
import statement is followed by the keyword __local__. This can also be combined with
aliasing (the __as__ keyword), making the prefix completely optional.
```c
import networking as net local;
import filesystem local;
```
Symbols of both `networking` and `filesystem` can be used without prefix. If a symbol
can be resolved unambiguously (for the current module and set of imports), the module
prefix is optional. So if both `networking` and `filesystem` have a function called `open()`,
it will still have to be prefixed, since C2 does not allow usage of ambiguous symbols.

The result of __modules__ and the __import .. (as ..) (local)__ is that there is no
need to constantly prefix the __definition__ part of the code anymore. In C it is common
for a library to prefix all symbols to avoid name clashes, for example:

`networking.h`
```c
void net_open();

void net_close();

struct net_data {
  ..
};
```

In C2 this would simply be:
```c
module networking;

public func void open() { .. }

public func void close() { .. }

public type NetData struct {
    ..
}

```
Developers can then choose the appropriate prefix they want to use in their code using

`import networking as ..`


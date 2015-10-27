## modules

Like many other modern languages, C2 has the concept of `modules`. Modules are
used to create a logical group for functions, types and variables. Each __.c2__
file has to start with the __module__ keyword, specifying which modules it belongs
to. A module can consist of several files, like

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

file1.c2 and file2.c2 belong to the same module `foo`, while file3.c2 belongs to
module `bar`.

Modules allow a developer to split functionality into several files, *without*
sacrificing speed or adding complexity.

## import
To have a file use declarations from another module, it must __import__ that module.
The __import__ keyword has a file scope. So if `file1.c2` imports module `io`,
`file2.c2` still cannot use `io`'s symbols, unless it imports `io` as well.

`file1.c2`
```c
module foo;

import file;
import storage;
// ..
```

`file2.c2`
```c
module foo;

import file;
import net;
// ..
```

So `file1.c2` can use external symbols from `file` and `storage`, while `file2.c2`
can use symbols from `file` and `net`.

One big advantage of not using c-style header-file includes is that no filenames will
ever appear in the code. So renaming/moving the files for some modules requires NO
change in other code, even it it uses that module!! Powerfull stuff.

## symbol visibility

All files of the same module have access to all the declarations of that module. Other
modules can only use the __public__ ones.

`file.c2`
```c
module foo;

public func void init() { .. }

func void open() { .. }
```

So other modules can use the `init()` function after importing foo, but only files in the
`foo` module can use `open()`.

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
`module.symbol` unique. In the example above, both modules `one` and `two` have the
`test` symbol.

### import as
When importing a module with a long name, C2 allows aliasing the module name (for the
current file only!), like:

```c
module foo;

import extended_filesystems_io as fs;
```
external symbols can now use the shorter `fs` prefix. Using the fullname is even now
allowed anymore!

### import local
When using a lot of external symbols, the constant prefixing can be cumbersome. C2
allows using the external symbols of some module directly (without prefix) when the
import statement is followed by the keyword __local__. This can also be combined with
the __as__ keyword.

```c
import networking as net local;
import filesystem local;
```
Symbols of both `networking` and `filesystem` can be used without prefix. If a symbols
can be resolved unambiguously (for the current module and set of imports), the module
prefix is optional. So if both `networking` and `filesystem` have some function `open()`,
it will still have to be prefixed, since C2 allows no ambiguous symbol use.

The result of __modules__ and the __import .. (as ..) (local)__, is that there is no
need to constantly prefix the __definition__ part of the code anymore. In C it is common
for a library to prefix all symbols, to avoid name-clashes, for example:

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


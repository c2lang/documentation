## modules

Like many other modern languages, C2 utilises the concept of `modules`. Modules are
used to create logical groups of functions, types and variables. Each `.c2` file must
begin with the __module__ keyword, specifying which module it belongs to. A module can
consist of several files, for example:

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

`file1.c2` and `file2.c2` belong to the same module, `foo`, while `file3.c2` belongs to
the module `bar`.

Modules allow the developer to split functionality into several files without
sacrificing speed or introducing extra complexity.

Modules only add a single layer of namespacing, so they are not nested like in some
languages (eg. Java). Nested namespaces aren't necessary - just look at C - it went by
with only the global layer. In the case of very big codebases, longer module names are
advised (eg. *network\_utils*).

## import
For a file to use declarations from another module, it must __import__ the said module.
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

One advantage of not using C-style header-file includes is that filenames will never
appear in the code. This means renaming/moving the files of modules requires
**NO** change to other code whatsoever, even if said code uses the module itself! How convenient!

## symbol visibility

All files of the same module have access to all the declarations in the module. Other
modules can only use declarations specified as `public`.

`file.c2`
```c
module foo;

public func void init() { .. }

func void open() { .. }
```

In this example, the other modules can use the `init()` function after importing foo, but
only files in the `foo` module can use `open()`, as it isn't specified as `public`.

## symbol resolving
To access symbols contained in other modules, dot notation is used:

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
Global symbols __must__ have unique names within their module, whether `public` or not. This ensures that
`module.symbol` is unique. In the example above, all three modules have contain a `test` symbol, but no clash
occurs, as the module which the symbol comes from is always specified.

### import as
When importing a module with a long name, C2 allows aliasing the module name (in the
current file only), like this:

```c
module foo;

import extended_filesystems_io as fs;
```
External symbols can now use the shorter `fs` alias. Even after aliasing, using the full name of the module is still allowed!

### import local
When using many external symbols, the constant prefixing can be cumbersome. C2
allows using the external symbols of a module directly (without any prefix) when the
import statement is followed by the keyword `local`. This may also be used with
aliasing (the __as__ keyword), making the prefix completely optional.
```c
import networking as net local;
import filesystem local;

// Equivalent
filesystem.doSomething();
doSomething();

// Equivalent
net.connect();
networking.connect();
connect();
```
The symbols of both `networking` and `filesystem` can be used without prefixing. If a symbol
can be resolved unambiguously (for the current module and set of imports), the module
prefix is optional. So if both `networking` and `filesystem` have a function called `open()`,
it will still have to be prefixed, since C2 does not allow usage of ambiguous symbols.

The result of __modules__ and the __import .. (as ..) (local)__ is that there is no
need to constantly prefix the definitions of the module anymore. In C it's common
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


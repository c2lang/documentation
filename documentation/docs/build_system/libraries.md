
C2 is an *evolution* of C. So one design criteria would be easy integration with
existing C libraries. This section explains how C2 integrates external C libraries.

## C/C++ library support
Let's start this section by first looking at existing library support in C (orC++).
The first thing to realize is:

__C does not have true library support!__

This may sound weird, considering that there a thousands of C libraries out there.
But the C-*language* doesn't know the concept of libraries. Using libraries in C consists
of several *separate* steps:

* including a header file that's just copy-pasted into your source file, like any other
*#include*
* passing some extra flags to the linker to tell it to include a dependency or extra
object files.

Both steps also have some supporting features like setting search-paths for headers/libraries etc
You can forget either of the steps. For example, forgetting the second step will usually result in some
cryptic *undefined reference to X* errors during linking. So it's up to the programmer to
do both steps.

## C2 library design
In C2, the compiler does have a concept of a library. So using a library is more an
atomic action; either you use it or not. Also every used library needs to be specified
in the recipe file. There is one exception to this rule: use of *libc* is opt-out. So by
default a program can use it. This is a convenience choice.

c2c uses an environment variable called *$C2_LIBDIR* indicating the directory where
libraries can be found. In this directory, each library as its own subdirectory, as
shown below.

NOTE: in the near future, extra paths can be given in the recipe file.

```bash
c2libs/
└── libc
    ├── manifest
    ├── stdio.c2i
    ├── stdlib.c2i
    ├── string.c2i
    └── strings.c2i
```

To be valid, a library-directory has to contain a *manifest* file and one or
more *interface* files.

###manifest file
Since C2 doesn't use *#include* headers, a mechanism is needed to map C header files
to C2 modules. This mechanism is the *manifest* file.

```toml
# C2 wrapper for the standard C library

[library]
language = "C"

[[modules]]
name = "stdio"
header = "stdio.h"

[[modules]]
name = "stdlib"
header = "stdlib.h"

[[modules]]
name = "string"
header = "string.h"

[[modules]]
name = "strings"
header = "strings.h"
```

The manifest file uses the [TOML-format](https://github.com/toml-lang/toml). The
key-values under *library* describe the generic setting for that library. For *langugage*
there can be two options: C or C2. Using C indicates that symbol-generation should not
prefix the declaration name with the module name. For example: *printf* simply becomes
the symbol *printf*, not *stdio_printf*.

The rest of the file maps specific C headers files to C2 modules. Optionally, the *header*
line describes which c-header should be included by the C-generation back-end.

###interface files
While C2 does not have header files or an *#include* mechanism, it does have *interface*
files. These are similar to regular .c2 files, except for:

* they have the .c2i extension (c2 inteface, get it?)w
* function can have no bodies
* every declaration must be public
* the filename (except .c2i) must match the module name inside (eg foo.c2i -> module foo; )
* there can be only one file per module

Part of *stdio.c2i* looks like this:

```c2
module stdio;

// .. (stuff left out)

public FILE* stdin;
public FILE* stdout;
public FILE* stderr;

public func int32 fclose(FILE* __stream);
public func int32 fflush(FILE* __stream);
public func int32 fprintf(FILE* __stream, const char* __format, ...);
public func int32 printf(const char* __format, ...);
public func int32 sprintf(char* __s, const char* __format, ...);

```

##advantages of integrated library-support

There are several additional advantages of integrating library support in the language:

###Library finding
c2c has an option *--showlibs* that searches the library paths and simply prints all
available libraries on a system. Handy for those tiny libs that always go missing.

###Dependency checking
The compiler can actually check if you really need some library and give a warning
otherwise. This avoids any unnecessary dependencies.

###Full symbol mapping
The compiler can generate a full dependency map of all symbols, including external
ones. This allows better insight into the code structure.

###c2tags
*c2tags* is the tool that allows editors to 'jump to definition' of any symbol. Because
of the interface files, that describe external symbols, editors have a place to jump
to for all symbols. This is a very powerful feature. Never mess with ctags files
ever again.



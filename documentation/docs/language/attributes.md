## Attributes

C2 incorporates standardized __attributes__. There can also be compiler-specific attributes
to do all sorts of funky things the compilers do.

The currently supported attributes are:

* __export__ (type, func, var)
* __packed__ (type)
* __unused__ (type, func, var)
* __unused_params__ (func)
* __section__ (func, var), requires argument
* __noreturn__ (func)
* __printf_format__ (func)
* __inline__ (func)
* __aligned__ (type, func, var), requires argument
* __weak__ (func, var)
* __opaque__ (public struct/union types)
* __cname__ (type, func, var), interface
* __no_typedef__ (interface struct/union types)

The standard syntax for all attributes is `@(  )`  (get it?!, @, at, attributes... ;) )

Take a look at the following example showing their usage in various declarations:

```c
// variables
i32 counter @(unused);
i32[1024] bigdata @(section="data") = {};

// types
type Point struct {
    i32 x;
    i32 y;
} @(packed, aligned=16)

type Weird enum u32 {
    FOO,
    BAR,
    FAA
} @(unused)

// functions
public func void init() @(export) {
    // ..
}
```

`NOTE: compiler-specific attributes will be required to start with an underscore,
like _c2_my_attribute_, so other compilers can recognize and ignore them`

### Printf_format

Printf_format is the C2 equivalent of C:
```__attribute__((format=(printf, 1, 2)));``` and is used like:

```c
func void log(const char* format, ...) @(printf_format=1) {
 // ..
}

```

Where the argument points to the (1-based) index of the format argument.

Any call to this function can than have its format checked and possibly give errors like:

```c
func void test() {
    log("%s", 10);  // error: "format '%s' expects a string argument"
              ^
}

```

See also [Printf specifiers](../language/printf_specifiers/)


### Opaque pointers

The __opaque__ attribute deserves some special attention. It is used to implement
the *opaque pointer* pattern in C2. See the Wikipedia article
[Opaque Pointer](https://en.wikipedia.org/wiki/Opaque_pointer) for more background info.

In short, opaque pointers are used to hide the implementation while giving the users
a *typed handle* to
pass to your library, maintaining type safety. The __opaque__ attribute can only
be used on *public struct/union types* and tells the compiler that *other*
modules can only use that type *by pointer* and are not allowed to dereference it.

```c
public type Handle struct {
    ..   // members are not visible outside module
} @(opaque)
```

When c2c generates an *interface file* (eg. module.c2i), it will only generate:
```c
type Handle struct {} @(opaque)
```

Note that it is allowed to put other non-public types as full members inside
a public opaque struct, since the members are not visible outside the module.


### Cname / No\_typedef
Some legacy C types/functions don't map really well to the C2 style. An example
of this is _stat.h_:

```c
struct stat {
    // ...
};

int stat(const char *pathname, struct stat *statbuf);
```

So both the struct and the function are called _stat_.

To solve this situation and offer a nice way to embed these calls into a C2 application,
C2 offers the attributes *cname* and *no_typedef*. In the C2 version of sys\_stat.h:

```c
type Stat struct {
    // ...
} @(cname="stat", no_typedef)

func c_int stat(const c_char* pathname, Stat* buf);
```

This means C2 code can use 'Stat' instead of 'struct stat', so the spelling conventions
stay intact (types start with capital case). Also for the C-backend, we cannot generate:
```c
typedef struct stat_ stat;

struct stat_ {
    // ...
};
```
...since that would clash with the function `stat`. So the attribute *no_typedef* tells c2c not
to generate the typedef, instead simply:
```c
struct stat {
    // ...
};
```


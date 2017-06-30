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
* __inline__ (func)
* __aligned__ (type, func, var), requires argument
* __weak__ (func, var)
* __opaque__ (public struct/union types)

The standard syntax for all attributes is `@(  )`  (get it?!, @, at, attributes... ;) )

Please, take a look at the following example showing how to add them to various declarations:

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
like _c3_my_attribute_, so other compilers can recognize and ignore them`

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

when c2c generates an *interface file* (eg. module.c2i), it will only generate
```c
type Handle struct {} @(opaque)
```

Note that it is allowed to put other non-public types as full members inside
a public opaque struct, since the members are not visible outside the module.



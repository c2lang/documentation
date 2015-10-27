
C2 incorporates standardized __attributes__. There can also be compiled-specific attributes
to do all sort of funky things compilers do.

Currently supported attributes are:

* __export__ (type, func, var)
* __packed__ (type)
* __unused__ (type, func, var)
* __unused_params__ (func)
* __section__ (func, var), requires argument
* __noreturn__ (func)
* __inline__ (func)
* __aligned__ (type, func, var), requires argument
* __weak__ (func, var)

The standard syntax for all attributes is `@(  )`  (get it?!, @, at, attributes... ;) )

see below how to add them to various declarations

```c
// variables
int32 counter @(unused);
int32[1024] bigdata @(section="data") = {};

// types
type Point struct {
    int32 x;
    int32 y;
} @(packed, aligned=16)

type Weird enum uint32 {
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


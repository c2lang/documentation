
## Primitive types

C2 has the following built-in primitive types:

* __bool__: Either __true__ or __false__.
* __i8__, __i16__, __i32__, __i64__: Signed integral types.
* __u8__, __u16__, __u32__, __u64__: Unsigned integral types.
* __f32__, __f64__. Single and double precision floating point types, respectively.

There is also a built-in void pointer type (__void*__), not to be confused with the
keyword __void__, which denotes a function that returns nothing.

For convenience, the __char__ keyword is also available and is identical to the __i8__ type.

Note that C2 does __not__ have any type specifiers like __signed__, __unsigned__, __long__ or __short__.

### C2 pseudo-module ###
The C2 compiler always has a pseudo module called __c2__. This module is used to
store some language symbols such as min/max values and things like build time, etc.
For each integral type there exists a minimum and maximum value:

* __min_i8__, __max_i8__
* __min_i16__, __max_i16__
* __min_i32__, __max_i32__
* __min_i64__, __max_i64__
* __min_u8__, __max_u8__
* __min_u16__, __max_u16__
* __min_u32__, __max_u32__
* __min_u64__, __max_u64__

```c
module foo;
import c2;

i32 highest = c2.max_i32;
```

It also includes some C types for mapping C declarations in libraries to C2 interface types.
See [External Libraries](../build_system/libraries/) for more information.

## Pointer types

Pointer types are created by adding an asterix (`*`) after the type they refer to, like

```c
void* a;
i8* b;
Point* c;
char**
```

## Array types

Arrays in C2 differ from C arrays in that `[]` always comes right after the element type, e.g:

```c
void*[]  a;
Point[4] b;
```

For array types, C2 introduces a new operator, namely `elemsof()`. This returns the number
of elements in an array and avoids C macros like:
```c
#define ARRAY_SIZE(x) ( sizeof(x) / sizeof(x[0]) )
```
The `sizeof()` operator is also still available.


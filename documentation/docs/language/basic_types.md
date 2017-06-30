
## primitive types

C2 has the following built-in primitive types:

* __bool__. can be either __true__ or __false__
* __i8__, __i16__, __i32__, __i64__. The signed types
* __u8__, __u16__, __u32__, __u64__. The unsigned types
* __f32__, __f64__. Floating point types

There is also a  builtin __void__ type, used for pointers (eg __void*__)
For convenience, the __char__ keyword is also available and is identical to the __i8__ type.

Note that C2 does __not__ have any type specifiers like __signed__, __unsigned__, __long__, __short__, etc.

### c2 pseudo-module ###
The c2compiler always has a pseudo module called __c2__. This module is used to
store some language symbols such as min/max values and things like build time, etc.
For each integer type there exists a min/max value:

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

It also includes some C type for mapping C declarations in libraries to C2 interface types.
See [External Libraries](../build_system/libraries/) for more information.

## pointer types

Pointer types are created by adding an asterix (*) after the type they refer to, like

```c
void* a;
i8* b;
Point* c;
char**
```

## array types

Arrays in C2 differ from C arrays in that the [] always come right after the element type, so

```c
void*[]  a;
Point[4] b;
```

For array types, C2 introduces a new operator, namely __elemsof()__. This returns the number
of elements in an array and avoids C macros like:
```c
#define ARRAY_SIZE(x) ( sizeof(x) / sizeof(x[0]) )
```
The __sizeof()__ operator is also still available.


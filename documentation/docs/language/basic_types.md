
## Primitive types

C2 has the following built-in primitive types:

* `bool`: Either `true` or `false`.
* `i8`, `i16`, `i32`, `i64`: Signed integral types.
* `u8`, `u16`, `u32`, `u64`: Unsigned integral types.
* `isize`, `usize`: architecture dependent, either i32/i64 or u32/u64.
* `f32`, `f64`: Single and double precision floating point types, respectively.
* `reg8`, `reg16`, `reg32`, `reg64`: Register types (volatile, unsigned).
* `void`: Same as in C.

For convenience, the __char__ keyword is also available and is identical to the __u8__ type.
The idea is to only use this for strings.

Note that C2 does __not__ have any type specifiers like __signed__, __unsigned__, __long__ or __short__.

### C2 pseudo-module ###
The C2 compiler always has a pseudo module called __c2__. This module is used to
store some language symbols such as min/max values and things like build time, etc.
For each integral type there exists a minimum and maximum value:

* `min_i8`, `max_i8`
* `min_i16`, `max_i16`
* `min_i32`, `max_i32`
* `min_i64`, `max_i64`
* `min_isize`, `max_isize`
* `min_u8`, `max_u8`
* `min_u16`, `max_u16`
* `min_u32`, `max_u32`
* `min_u64`, `max_u64`
* `min_usize`, `max_usize`

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
char** d; // Pointer to a pointer to a char
```

The `nil` keyword may be used to assign a null value to a pointer:
```c
char* name = nil;
```

## Array types

Arrays in C2 differ from C arrays in that `[]` always comes right after the element type, e.g:

```c
void*[]  a;
Point[4] b;
```

For array types, C2 introduces a new operator, namely [elemsof](../language/builtin_functions/#elemsof) This returns the number
of elements in an array and avoids C macros like:
```c
#define ARRAY_SIZE(x) ( sizeof(x) / sizeof(x[0]) )
```
The `sizeof()` operator is also still available.


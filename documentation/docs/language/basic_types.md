
## primitive types

C2 has the following builtin primitive types:

* __bool__. can be __true__ or __false__
* __int8__, __int16__, __int32__, __int64__. The signed types
* __uint8__, __uint16__, __uint32__, __uint64__. The unsigned types
* __float32__, __float64__. Floating point types

There is also a  builtin __void__ type, used for pointers (eg __void*__)
For convenience, the __char__ keyword is also available and is identical to the __int8__ type.

Note that C2 does __not__ have any type specifiers like __signed__, __unsigned__, __long__, __short__, etc

### c2 pseudo-module ###
The c2compiler always has a pseudo module called __c2__. This module is used to
store some language symbols like min/max values and things like build-time, etc.
For each integer type there exists a min/max value:

* __min_int8__, __max_int8__
* __min_int16__, __max_int16__
* __min_int32__, __max_int32__
* __min_int64__, __max_int64__
* __min_uint8__, __max_uint8__
* __min_uint16__, __max_uint16__
* __min_uint32__, __max_uint32__
* __min_uint64__, __max_uint64__

```c
module foo;
import c2;

int32 highest = c2.max_int32;
```

## pointer types

Pointer types are created by adding an asterix (*) after the type they refer to, like

```c
void* a;
int8* b;
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
of elements in an array and avoid C macros like:
```c
#define ARRAY_SIZE(x) ( sizeof(x) / sizeof(x[0]) )
```
The __sizeof()__ operator is also still available.


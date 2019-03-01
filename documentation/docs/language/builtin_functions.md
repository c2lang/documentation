
## Builtin functions

C2 has the following built-in functions:

* sizeof
* elemsof
* offsetof

### sizeof ###
`sizeof` is the same as in C, it returns a 'u32' with the size of the type/variable.

```c
u32 size = sizeof(void*);
```

### elemsof ###
For array types, C2 introduces a new operator, namely `elemsof`. This returns the number
of elements in an array and avoids C macros like:
```c
#define ARRAY_SIZE(x) ( sizeof(x) / sizeof(x[0]) )
```

So

```c
char[15] name;
const u32 len = elemsof(name];   // len will be 15
```
Note that this also works for [incremental arrays](variables/#incremental-arrays).

### offsetof
`offsetof` is the same as in C, only it comes with the language (no need to include something).
The syntax is offsetof(type, member). For example:

```c
type Struct struct {
    i32 a;
    struct sub {
        i32 b;
    }
}

u32 o1 = offsetof(Struct, a);
u32 o2 = offsetof(Struct, sub.b);
```


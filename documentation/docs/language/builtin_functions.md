
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
const u32 len = elemsof(name);   // len will be 15
```
Note that this also works for [incremental arrays](variables/#incremental-arrays).

The `elemsof` function can also be used on Enum types, to return the number of elements
in the Enum.


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

### to_container

`to_container` replaces a much used C macro, namely:
```c
#define to_container(type, member, ptr) \
    ((type *)((char *)(ptr)-(unsigned long)(&((type *)0)->member)))
```

Essentially, this macro converts a pointer to a member of a struct to
a pointer of the struct itself. Examples where this macro is used is the Linux kernel
linked list mechanism. This works by embedded a 'List' node inside the Element you want
in the list. The list then points to this member. When iterating, you need to
convert the list member to the containing struct.

The syntax is `to_container`(type, member, pointer), where:

- Type is a struct/union type
- member is a member of that struct/union
- pointer is a pointer to that member (and of that same type)

For example:

```c
type Struct struct {
    i32 a;
    i32 b;
}

func void example(i32* i) {
    Struct* s = to_container(Struct, b, i); // so i points to member b
}

func void example(const i32* i) {
    const Struct* s = to_container(Struct, b, i); // const status is maintained
}
```

Unline the macro version, the _const_ status of the original pointer is maintained.


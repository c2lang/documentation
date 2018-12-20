# User defined types

Just like C, C2 allows the programmer to define new types.

The syntax to define new types is very standard and uniform: `type <name> <definition>`.
Like all global declarations, they may also be specified as `public`. Note that the `public`
specifier is required when the type is used as a parameter, return type or a member of a public symbol.

The type can define:

* an `enum`
* a `struct`
* a `union`
* an alias for another type
* a `func`

## Enum types

Enum (enumerated) types use the following syntax:
```c
type State enum i8 {
    BEGIN = 0,
    MIDDLE,
    END
}
```

Note that all enum constants are in only available through the enum type's namespace
(eg. State.BEGIN, not BEGIN).

## Struct types

Structs are defined like so:
```c
type Person struct {
    u8 age;
    char* name;
}
```

A struct's members may be accessed using dot notation:
```c
Person p;
p.age = 21;
p.name - "John Doe";

io.printf("%s is %d years old.", p.age, p.name);
```


## Union types

Union types are defined just like structs:
```c
type Integral union {
    u8 as_u8;
    u16 as_u16;
    u32 as_u32;
    u64 as_u64;
}
```

Unions, unlike structs, may only have one active member at a given time. See below:
```c
Integral i;
i.as_u8 = 40; // Setting the active member to as_u8

i.as_u32 = 500; // Changing the active member to as_u16

io.printf("%d", i.as_u8); // Undefined behaviour: as_u8 is not the active member, so this will probably print garbage.
```

Note that unions only take up as much space as their largest member, so `sizeof(Integral)` is equivalent to `sizeof(u64)`.

C2 also features (anonymous) sub-structs/unions:
```c
type Person struct {
    u8 age;
    char* name;
    union {
        i32 employee_nr;
        u32 other_nr;
    }
    union subname {
        bool b;
        Callback cb;
    }
```


## Alias types
Alias types are used to give an alias to a different type, like:

```c
type CharPtr char*;
type Numbers i32[10];
```

Instead of the obscure C syntax used to alias a function pointer type, C2 uses the
following:
```c
public type Callback func void(i32 a, bool b);
```
This defines an alias to function pointer type of a function that returns nothing and requires two
arguments: an `i32` and a `bool`.



## Function types
Function types are used to pass a function as an argument to another function or to store
it in a variable:

```c
type Callback func i32(i32, void*);
```

A usage example is given below:
```c
// in some function body
Callback cb = my_callback
cb(10, nil);
```


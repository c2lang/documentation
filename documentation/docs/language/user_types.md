
The syntax to define new types is very standard and uniform: `type <name> <definition>`
Like all global declarations, they may also have the __public__ specifier in front.
Note that the __public__ specifier is required when the type is used as a parameter or return type 
of a public symbol

The definition can be:

* __enum__
* __struct__ or __union__
* __func__
* another type (for alias types)

## enum types

enum types use the following syntax:
```c
type State enum int8 {
    BEGIN = 0,
    MIDDLE,
    END
}
```

## function types

Instead of the obscure C syntax to defined function-pointer types, C2 uses the
following:
```c
public type Callback func void(int32 a, bool b);
```
This defines a function pointer type to functions that return nothing and need 2
arguments.

## struct/union types

```c
type Person struct {
    uint8 age;
    char* name;
    union {
        int32 employee_nr;
        uint32 other_nr;
    }
    union subname {
        bool b;
        Callback cb;
    }
}
```

## alias types
Alias types are used to give a name to other types, like:

```c
type CharPtr char*;
type Numbers int32[10];
```




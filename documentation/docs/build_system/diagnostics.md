
One of the perks of telling the compiler everything is that it can do much more
for you. This page describes some of the special ones.

## Unused warnings
Most diagnostic messages will be errors, but out of the warnings, warnings for unused
symbols are the most common.

### unused variables

```c
module test;

public i32 y;     // warning: unused variable 'test.y'

public fn void foo() {
    i32 x;          // warning: unused variable 'x'
}
```

### functions
All functions of the entire program can be checked for usage.

```c
module test;

fn foo() {}    // warning: unused function 'test.foo'
```

### unused imports
c2c will tell you when imports are not used

```c
module test;
import stdlib;      // warning: unused module 'stdlib'
```

### unused public

```c
module test;

public fn void foo() {}   // warning: function 'test.foo' is not used public

public fn i32 main() {
    foo();
    return 0;
}
```

### unused types
```c
module test;

type Object struct {        // warning: unused type 'test.Object'
    i32 x;
}
```

### unused fields
c2c can even warn when a specific field of a struct is not used!

```c
type Object struct {
    i32 x;
    Object* next;       // warning: unused struct member 'next'
}

fn Object foo() {
    Object o;
    o.x = 10;
}
```


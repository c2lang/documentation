
Variables in C2 look a lot like variables in C (on purpose), so:

```c
i32 counter = 0;
public bool hasBool = false;
Point* p = nil;
```

The __public__ keyword can only be used for `global` variables. The __local__ keyword
only on `non-global` ones. Easy.

### local keyword

The __local__ keyword has the same meaning as the __static__ keyword when used on local
(as in non-global) variables in C; their lifetime is bigger then that of the function.
For example calling the function below 3 times:

```c
func void increment() {
    local i32 counter = 0;
    counter++;
    printf("%d\n", counter);
}
```
will result in:
```
1
2
3
```

## initialization

C2 has some powerful variable initialization features.
Initialization also looks very similar to C, so

```c
type Data struct {
    i32 a;
    char* text;
    f32 f;
}

Data[] mydata = {
    { 1, "first",  1.11 },
    { 2, "second", 2.22 },
    { 3, "third",  3.33 },
}
```

In C2, all global variables are automatically initialized with a default value
if no explicit initialization is done.

The examples below show some C2 initialization options.

### array index designators
```c
i32[] array = {
    [10] = 0,
    [11] = 3,
}

// mixing index designators with default (incremental) initialization
i32[4] array2 = {
    0,
    [3] = 3,
    4,          // error: access elements in array initializer
}

// using enum constant as index designator value
type Enum enum i8 {
    FOO = 2,
    BAR = 5,
}

i32[] array = {
    [BAR] = 5,
    0,          // index 6
    [FOO] = 2
    3,          // index 3
    4,
    5,          // error: duplicate initialization of array index
}

// using non-compile-time constant as index value is not allowed
i32 a = 1;
const i32 b = 2;

i32 array2 = {
    [a] = 1,    // error: initializer element is not a compile-time constant
    [b] = 2,
}
```

### field designators
Field designators initialize struct members by name.
```c
// basic struct fields

type Point struct {
    i32 x;
    const u8* name;
}

Point[] array = {
    { 1, "one" },   // basic struct initialization

    { .x = 3, .name = "three" },    // using field designators

    { 4, .name = "four" },  // error: mixing field designator with non-field designators
}
```


## incremental arrays
A special feature in C2 are incremental arrays. These can be used to avoid messy macros when
it is required to have some elements of the array present depending on some external condition (__#ifdef__'ed).

To define an incremental array, use the `[+]` array subscript. Entries can then be added from
different points in the code.
```c
type Point struct {
    i32 x;
    i32 y;
}

Point[+] points;

points += { 10, 11 }

// ... other code

points += { 20, 22 }

// ... other code

points += { 30, 31 }
```

the __sizeof()__ and __elemsof()__ operators will keep track if this of course.

NOTE: Incremental arrays can only be used at __global__ level (not inside functions).


### nil
Instead of having to define `NULL` somewhere, C2 have the __nil__ keyword. This can only
be used for pointer types.


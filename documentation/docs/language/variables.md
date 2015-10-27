
variables in C2 look a lot like variables in C (on purpose), so:

```c
int32 counter = 0;
public bool hasBool = false;
Point* p = nil;
```

the __public__ keyword can only be used for `global` variables. The __local__ keyword
only on `non-global` ones. Easy.

### local keyword

The __local__ keyword has the same meaning as the __static__ keyword when used on local
variables in C; their lifetime is bigger then the function. for example calling the function below
3 times
```c
func void increment() {
    local int32 counter = 0;
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

Variable initialization also looks very similar to C, so
```c
type Data struct {
    int32 a;
    char* text;
    float f;
}

Data[] mydata = {
    { 1, "first",  1.11 },
    { 2, "second", 2.22 },
    { 3, "third",  3.33 },
}
```

In C2, all global variables are automatically initialized with a default value
if no explicit initialization is done.

## incremental arrays
A special feature in C2 are incremental arrays. These can be used to avoid messy macros when
have some elements of the array depending on some external condition (#ifdef'ed).

To define an incremental array, use the `[+]` array subscript. Entries can then be added from
different points in the code.
```c
type Point struct {
    int32 x;
    int32 y;
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


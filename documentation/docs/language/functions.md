# Functions

Functions declarations always start with the `func` keyword; otherwise they look very similar
to C. Since there are no forward declarations of any kind in C2, there is just one form
of a function declaration, which is the definition:

```c
public func i32 main(i32 argc, char** argv) {
    return 0;
}
```

Functions may also have attributes. More information on attributes can be found [here](attributes.md).

## Arrays
In C2 arrays cannot be used as function arguments, so pointers must be used instead. This is done
because in C passing 'int numbers[20]' is not a copy, but a pointer to an array, which is confusing
and could lead to bugs.


## Arguments
Default arguments (like: func void test(i32 a = 10) {} ) are not allowed in C2.



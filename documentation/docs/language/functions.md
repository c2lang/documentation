## Functions

Functions declarations start with the __func__ keyword; otherwise they look very similar
to C. Since there are no forward declarations of any kind in C2, there is just one form
of a function declaration, which is the definition:

```c
public func i32 main(i32 argc, char*[] argv) {
    return 0;
}
```

Functions may also have attributes. More information on attributes can be found [here](attributes.md).

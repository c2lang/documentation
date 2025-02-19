
A language is implemented in a _compiler_. For C2 this is C2C; the C2 Compiler.

### Implementation

Since 2024, the C2 compiler is written in C2 itself, so it is self hosting.
This requires _bootstrapping_ as described in the build section.

### Performance

C2C currently parses between 2-4 million lines of code per second, depending on the hardware.
Analysing that same code is roughly twice as fast.

The current implementation of _c2c_ is around 40.000 lines of code. Parsing that with _c2c_ takes:

```bash
parsing took 11215 usec
analysis took 7345 usec
```

So it is _fast_.


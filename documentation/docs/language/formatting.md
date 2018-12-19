# Formatting

## Comments

C2 comments work in the same way as C's. Thus both forms are available:

```c
// a one line comment
/* A
   multi-line
   comment
*/
```

## Semicolons

Every statement in C2 is followed by a semicolon, __except__ when it ends with
a __right-hand brace__.
Trailing attributes never change the rule above.

This means that in C2, you'll never see `};`.  It's easier on the eyes ;)

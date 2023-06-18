# Loops + Flow control

C2 has the same loop constructs as C, namely: for, while and do..while.

## For

```c
for (u32 i=0; i<10; i++) printf("%d\n", i);
```

## While

```c
while (i<10) i++;

while (Point* p = get_point()) { .. }
```

Note that C2 does allow a declaration to be used as condition (see example above)


## Do-while

Do-while statements are exactly the same as C.

```c
do {
    i++;
} while (i<10);
```

## If

If statements are exactly the same as C.

```c
if (x < 10 && y >= 10 && ptr != nil) { .. }
```


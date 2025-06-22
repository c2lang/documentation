## Init-list assignment

Init-list assignment is a syntatic sugar feature.


In C and C2, *init-lists* are used during *initialization*, like:

```c

type Point struct {
    i32 x, y;
}

Point p1 = { 1, 2 }
Point p2 = { .x=1, .y=2 }
```

For assigning a point happens like:

```c
fn void test1(Point p) {
    p.x = 1;
    p.y = 2;
}
```

This syntax would also be handy during *assignment*, since it is unambiguous and
takes up less code. This is allowed in C2, so the code then looks like:

```c
fn void test1(Point p) {
    p = { 1, 2 };
    p = { .x=3, .y=4 };
}
```

// NOTE: Please notice the ';' after '}', this is required for statements!

When a Type is expected by value (not by pointer), this syntax can also be used:

```c
fn void set(Point p) {
    // ...
}

fn void test() {
    set({ 3, 4 });
}
```


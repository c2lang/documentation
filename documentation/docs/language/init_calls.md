## Init calls

Init calls are a *syntatic sugar* feature that allows combining the local variable's *definition*
with its *initialization* using a type-function.

```c

type Point struct {
    i32 x, y;
}

fn void Point.init(Point* p) {
    p.x = p.y = 0;
}

fn void test1() {

   // without init call
   Point p;
   p.init();

   // with init call
   Point p.init();

   // ...
}
```


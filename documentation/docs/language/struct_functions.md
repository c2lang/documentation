
Another new feature in C2 are the __struct-functions__.

The struct-functions are a _syntatic-sugar_ feature that makes code more readable.

The example below shows how it works:

```c
type Point struct {
    i32 x;
    i32 y;
}

func void Point.add(Point* p, i32 x) {
    p.x = x;
}

func void example() {
    Point p = { 1, 2 }

    // with struct-functions
    p.add(10);

    // without struct-functions (if defined as non struct-function)
    /// point_add(&p, 10);
}
```

## rules
* struct-functions only work on struct/union types
* struct-functions are defined in the same module as the struct (not necessary the same file!)
* for a type type named _Foo_, struct functions must start with _Foo._ prefix.
* there cannot be a regular member _x_ and a struct function _foo.x_.
* a static struct-function is called on the type itself: Foo.myfunc(); It's not allowed
    to call this on a variable
* a static struct-function has no argument requirements and can only be called on the type:
    _Foo.add()_. _f.add()_ is not allowed.
* a (non-static) struct-function is required to have 'Type\*' or 'const Type\*' as the first argument
* struct-function can also be assigned to variables of type Function with the correct proto-type:
    _callback = Foo.add;_
* sub-structs cannot have struct functions

Extra notes:

* it's possible to have public or non-public struct-functions for a public struct.
* a struct-function itself is a regular function and can also be used as such
* it's also possible to use a struct-function call if the variable of struct type is a member of
    another struct
```c
type Outer struct {
    Inner inner;
}

type Inner struct {
    // ...
}

func void Inner.modify(Inner* inner, /* ... */) {
    // ...
}

func void example() {
    Outer outer;
    outer.inner.modify(/* ... */);
    // translates to Inner.modify(&outer.inner);
}
```

* the following is ok, since a struct-function is not a real dereference
```c
    Type* t = nil;
    t.init();
```

for more examples, see the tests in _c2compiler/test/Functions/struct_functions/_


###bigger example

Or another example, which also uses the opaque pointers:
Module __inner__ offers an API:

```c
module inner;

import stdlib;
import stdio;

public type Shape struct {
    u8 sides;
    // ..
} @(opaque)

// a non-public struct-function
func void Shape.init(Shape* shape, u8 sides) {
   shape.sides = sides;
}

// a static struct-function, called as Shape.create(..)
public func Shape* Shape.create(u8 sides) {
    Shape* shape = stdlib.malloc(sizeof(Shape));
    shape.init(sides);
    return shape;
}

// a public, const struct-function, first argument: const Shape*
public func void Shape.print(const Shape* shape) {
    stdio.printf("shape with %d sides\n", shape.sides);
}

// a public, struct-function, first argument: Shape*
public func void Shape.free(Shape* shape) {
    stdlib.free(shape);
}
```

which is used by module outer:

```c
module outer;

import inner local;

func void example() {
    Shape* s = Shape.create(3);
    s.print();
    s.free();
}
```

_NOTE_: outer is not allowed to access Shape's regular members directly.


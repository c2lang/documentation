
Another new feature in C2 are the __struct-functions__.

The struct-functions are a _syntatic-sugar_ feature that makes code more readable.

The example below shows how it works:

```c
type Point struct {
    int32 x;
    int32 y;
}

func void point_add(Point* p, int32 x) {
    p.x = x;
}

func void example() {
    Point p = { 1, 2 }

    // the 2 statements below are equal

    // without struct-functions
    point_add(&p, 10);

    // with struct-functions
    p.add(10);
}
```

## rules
* struct-functions only work on struct/union types
* struct-functions are defined in the same module as the struct (not necessary the same file!)
* for a type type named _Foo_, struct functions must start with _foo\__ prefix. The initial character is
    changed to lower case and the name is followed by an underscore
* there cannot be a regular member _x_ and a struct function _foo\_x_.
* a static struct-function is called on the type itself: Foo.myfunc(); It's not allowed
    to call this on a variable
* a static struct-function has no argument requirements
* a (non-static) struct-function is required to have 'Type\*' or 'const Type\*' as the first argument
* it's not allowed to use a struct-function for other uses than to be called. So 'Ptr p = var.init'
    will result in an error
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

func void inner_modify(Inner* inner, /* ... */) {
    // ...
}

func void example() {
    Outer outer;
    outer.inner.modify(/* ... */);
    // translates to inner_modify(&outer.inner);
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
    uint8 sides;
    // ..
} @(opaque)

// a non-public struct-function
func void shape_init(Shape* shape, uint8 sides) {
   shape.sides = sides;
}

// a static struct-function, called as Shape.create(..)
public func Shape* shape_create(uint8 sides) {
    Shape* shape = stdlib.malloc(sizeof(Shape));
    shape.init(sides);
    return shape;
}

// a public, const struct-function, first argument: const Shape*
public func void shape_print(const Shape* shape) {
    stdio.printf("shape with %d sides\n", shape.sides);
}

// a public, struct-function, first argument: Shape*
public func void shape_free(Shape* shape) {
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

Since struct-functions are _syntactic-sugar_, the same example can also be done
without them:

```c
module outer;

import inner local;

func void example() {
    Shape* s = shape_create(3);
    shape_print(s);
    shape_free(s);
}
```



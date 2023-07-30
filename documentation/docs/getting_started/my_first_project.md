
Creating your first project in C2 is easy, just run:

```bash
~$ mkdir hello
~$ cd hello
~/hello$ c2c --create hello
~/hello$ ls
   main.c2
   recipe.txt
~/hello$ c2c
```

and use the following skeleton files:

`hello.c2`
```c
module hello;

import stdio as io;

public fn i32 main(i32 argc, char** argv) {
    io.printf("Hello C2!\n");
    return 0;
}
```

`recipe.txt`
```ini
executable hello
  $warnings no-unused
  $backend c
  hello.c2
end
```

Calling `tree` will show the output:
```bash
.
├── hello.c2
├── output
│   └── hello
│       ├── build
│       │   ├── Makefile
│       │   ├── build.log
│       │   ├── hello.c
│       │   ├── hello.h
│       │   └── hello.o
│       └── hello
└── recipe.txt
```

As you can see, c2c will generate all required .c and .h files and the *Makefile* to build.
Depending on the options in the *recipe*, c2c will generate a single .c/.h pair per *module* or
per *target*.

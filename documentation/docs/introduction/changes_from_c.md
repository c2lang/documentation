
Because C2 is an _evolution_ of C, this page summarizes the
_changes_ made and the design philosophy behind it. The main
goal of C2 is to get rid of or improve things that were the 
weak points of C and to adopt some modern concepts (e.g. modules)
found across many different programming languages.

## no header files
Header files are one of such weak points in C. There is many
reasons why:

1. Forces a lot of extra work on the parser
2. Keeping track of more file types means extra work
3. You need to make sure a header doesn't get included twice or do #ifndef protection
4. Computed includes are pain, you know it
5. You need to think what to have in headers and what in .c files, slows down development,
albeit just a bit.

For this reason, C2 follows the trend set by many different languages
and that is one file type only. The only file extension used by C2 is
 '.c2'.  That allows for way simpler and cleaner organization of code, 
twice as much for newbies.

So now that now that headers are gone what can compensate for their 
functionality? Good question, this is where modules get to speak.
C2 adopts the concept of modules, which do a similar things as 
modules, packages or namespaces in other languages. Now code is no
longer divided to files, you can use symbols from one file in another,
as long as they belong to the same module, also module system removes
the need to worry about name conflicts, thus forcing the developer to
use some sort of a prefix for their functions (you know how each and 
every gtk function starts with "gtk_", right?). Note that contents of 
each file do need to belong to a module and that the module statement
has to be the first statement of each c2 file:


```c
//file1.c2
module example;
int32 importantnumber = 5;
```
```c
//file2.c2
module example;

import stdio as io;
public func int32 main(int32 argc, char*[] args)
{
    io.printf("importantnumber = %d\n", importantnumber);
    return 0;
} 
```

Now, look at the second example once again. Do you see that import
statement? When we have modules, we need a way to organize the act
of importing of them. The import keyword provides full and better
replacement for the obsolete #include statement. It has several
advantages:

1. Imports whole modules, not just a single file. With one import
statement you can import for example 500 files at once, provided
all belong to just one module.
2. Provides protection against ambiguity and name nameclashes.
3. Allows to abbreviate long module names.

For more information on importing and modules, see [modules](../language/modules.md)

## no forward declarations
Forward declarations were another thing slowing down development
in C. It is a technique in used in languages that requires symbols 
to be declared before use. Again, C2 has followed the way modern
programming languages work and does not depend on the order of
declarations and calls. The compiler automatically sorts this 
itself at compile-time. Thanks to that, the following example is 
completely ok in C2:

```c
module example;

import stdio as io;
public func int32 main(int32 argc, char*[] args)
{
    io.printf("factorial of five is %d", factorial(5));
    return 0;
}
int64 factorial(int32 n)
{
  int32 c;
  int64 result = 1;
 
  for (c = 1; c <= n; c++)
    result = result * c;
 
  return result;
}
```

In C, you can't do such thing, because the compiler reads the file
sequentially, line by line. Therefore at the line where is the only
call, it wouldn't know what factorial is yet, and forward declaration
would be mandatory. Because of the need to use forward declarions
you have to type many things twice, which reduces development speed.

And how is this possible? Through the use of multi-pass compiler.
The C2 compiler is no longer a compiler which compiles line by line.
C2 builds an AST (Abstract Syntax Tree) in several steps. Thanks
to the AST, the compiler has a lot more information about the program
and doesn't require declarations to precede usage. After an AST
has been generated, the compiler just generates code accordingly 
for the backend selected.

##member access always through '.' punctuator, '->' for struct pointers
In C, you have to use the '->' punctuator if you want to access members
of a struct pointer and '.' if you are accessing members of a struct variable.
C2 simplifies this and turns this:

```c
struct mystruct *pointer;
struct mystruct var;

pointer->field = ...
var.field = ...
```

Into this:

```c
mystruct* pointer
mystruct var;

pointer.field = ...;
var.field = ...;
```

This helps remove some clutter and makes the code a bit cleaner. Also,
the efforts you need to utilize when performing changes is lowered.

## unified type definitions
Type definitions in C are not very good, they sure work fine, but are
not very unified. C2 tries to address to this issue by providing a unified,
easily readable way of type definitions. Now you no longer need to use the
struct keyword when declaring variables of a struct type without typedef.
Also, all type definitions are defined with the type keyword, followed
by the name of the type and the kind of a type ( enum, Callback, struct )
and their content:

```c
type string char*;
type Dog struct 
{
    uint8 age;
    string name;
    string favouriteToy;
    uint13 height;
    Mood mood;
}

type Mood enum int8 
{
    HAPPY,
    ANGRY,
    PLAYFUL,
    HUNGRY
}
```
For more information, see [User-defined types](../language/user_types.md)

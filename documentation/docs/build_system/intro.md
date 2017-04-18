
The scope of C2 language is larger than just the code itself; it also includes the
build system. Including this allows doing many nice things easily, like LTO (link
time optimization), etc.

## recipe
A C2 project requires a `recipe` file. This file describes:

* the target type (executable, shared library or static library)
* which options are used (compile time flags)
* which source files are used

For big projects, the recipe file would be generated (for example by `make menuconfig`),
especially if there are many different configurations.

The exact format of the recipe file is still in progress, but it currently looks like:

```ini
# an example recipe file

executable hello
  $warnings no-unused
  sources/hello.c2
end

lib graphics static
  $export api
  sources/file1.c2
  file2.c2
  api/file3.c2
end
```
Note that the path to each c2 file is relative to the position of the __recipe file__, not
to that of the c2c binary.

### calling directory
When building targets, `c2c` is only called __once__. This way the compiler knows the
target type and all required files. Since you might be working in some subdir of your
project, c2c will iteratively search dirs to root until *recipe.txt* is found - no
need to change dirs to run it!

So these two calls are equal if *my_project/* contains the *recipe.txt*:
```
~my_project$ c2c
~my_project/some_subdir$ c2c
```


### output directory
Any output will be placed in the *output/* directory generated in the root directory of the project.
Inside the output directory, generated files of each target will have their own folder.
Therefore, to *make clean* just means to remove this directory.

## Options

Inside a target, the following options are available:

 * *config [names]* - specify definitions for the preprocessor, like #define feature1
 * *export [modules]* - export the modules in libraries and generated headers
 * *generate-c* - enable generation of C code
 * *generate-ir* - enable generation of LLVM IR code
 * *warnings [list]* - enable/disable specific warnings during compilation
 * *deps [options]* - enable generation of dependency file with certain options
 * *refs* - generate reference file (used for jumping to definition)
 * *use [libs]* - depend on given libraries



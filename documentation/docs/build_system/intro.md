
The scope of C2 language is larger than just the code itself; it also includes the
build system. Including this allows doing many nice things easily, like LTO (link
time optimization, etc).

## recipe
A C2 project has a `recipe` file. This file describes:

* the target type (executable, shared library or static library)
* which options are used (compile time flags)
* which source files are used

For big projects, the recipe file would be generated (for example by `make menuconfig`),
especially if there are many different configurations.

The exact format of the recipe file is stil in progress, but it currently looks like:

```ini
# an example recipe file

target hello
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

### calling directory
When building these targets, `c2c` is only called __once__. So the compiler knows the
target type and all files required. Since you might be working in some subdir of your
project, c2c will iteratively search dirs to root until *recipe.txt* is found, no
need to change dirs to run it!

So these 2 calls are equal if *my_project/* contains the *recipe.txt*
```
~my_project$ c2c
~my_project/some_subdir$ c2c
```


### output directory
Any output will be put in the *output/* directory in the root of the project.
So a *make clean* equivalent just means removing this directory.


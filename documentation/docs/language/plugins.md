## Plugins

The C2 compiler is more than a tool to convert c2 code into binaries. It aims to speed up
development and help developers get things done.

For example:

As such in projects using Make/CMake/Ninja, every project wastes time configuring the build
system  to insert a GIT version into the code, so that every build automatically contains
the version. The C2 compiler now has a plugin call _git_version_ that inserts the git version
into a (generated) module named _git_version_. It can be used as a normal symbol, so:

```c
module main;

import stdio;
import git_version;

fn i32 main(i32 argc, char** argv) {
    io.printf("Git version: %s\n", git_version.describe);
    return 0;
}
```

The required plugins are specified in the _recipe.txt_ file or in the _build-file_.
Since both may be auto-generated, duplicate plugins are filtered out.


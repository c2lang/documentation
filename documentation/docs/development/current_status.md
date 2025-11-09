
The C2 language is still under development. However, since it is an *evolution*
of C, no major changes are expected. Still, some new features might break current code.

Component | Status | Notes
--- | --- | ---
Parser | 100% | pretty much parses everything. Could do a bit better for *syntax errors*
Analyser | 100% | fully working
Plugins | 100% | c2c supports plugins (refs, deps, unit_test, load_file, shell_cmd, etc)
C generator | 100% | some corner cases still need to be implemented
IR generator | 20% | Generating IR, some optimizations, no lowering yet
Embedded | 10 % | This means generating images for embedded targets, .S files, linker scripts, etc
C2Format | 0 % | A 'style' tool (like astyle, clang-format)

Supported Platforms:

* Linux x86_64
* Darwin x86_64, arm64
* FreeBSD amd64
* OpenBSD amd64


So the short status would be that for using c2c with C backend, it's usable for real projects.
The IR backend is targetted for very fast builds, but cannot self-compile c2c yet.




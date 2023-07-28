
The C2 language is still under development. However, since it is an *evolution*
of C, no major changes are expected. Still, some new features might break current code.

Component | Status | Notes
--- | --- | ---
Parser | 100% | pretty much parses everything. Could do a bit better for *syntax errors*
Analyser | 100% | fully working
Plugins | 100% | c2c supports plugins (refs, deps, unit_test, load_file, shell_cmd, etc)
C generator | 99% | some corner cases still need to be implemented
IR generator | 5% | basically only *Hello World* works (functions, strings, function calls)
Templates | 10 % | Function templates work, but Struct templates (with functions) are not in yet.
Macros | 0 % | Still in the design phase, maybe not needed?
Embedded | 10 % | This means generating images for embedded targets, .S files, linker scripts, etc
QBE backend | 5 % | Hello world works currently, but not much more
C2Format | 0 % | A 'style' tool (like astyle, clang-format)


So the short status would be that for playing around with the language using the C-generating
back-end, it's very usable for real projects


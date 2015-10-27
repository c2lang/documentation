
The C2 language is still under development. However, since it is an *evolution*
of C, no major changes are expected. Still, some new features might break current code.

Component | Status | Notes
--- | --- | ---
Parser | 90% | pretty much parses everything. Needs better handling of *syntax errors*
Analyser | 60% | needs improved *type checking*, *un-initialized* diagonostics
C generator | 95% | some corner cases still need to be implemented
IR generator | 5% | basically only *Hello World* works (functions, strings, function calls)

So the short status would be that for playing around with the language using the C-generation
back-end, it's pretty usable.


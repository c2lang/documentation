# C2 Documentation

This is the source code for generating the C2 documenation.
They are generated with [MkDocs](http://www.mkdocs.org).

To install mkdocs:

```
$ pip install mkdocs
```

To work on the docs locally, `cd` to the repo directory and:

```
$ mkdocs serve
```

This will give you a live-reloading local version of the docs. When you are ready to publish your changes, make sure you are sync'd with the repo and then:

```
$ mkdocs gh-deploy
```

To build
```
mkdocs build --clean
````

To install, copy the generated site/ to the webserver.

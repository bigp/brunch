# Brunch docs

Sections:

* [FAQ](./faq.md)
* [Commands](./commands.md)
* [Config](./config.md)
* [Plugins](./plugins.md)
* [Upgrading](./upgrading.md)

## Basics

Example application structure:

```
root/         # Main brunch application directory.
|-app/        # Your code may reside here.
|--assets/    # Static files that shall not be compiled
|---images/   # will be just copied to `public` dir.
|--views/     # Create any subdirectories inside `app` dir.
|---file-1.js # JS files will be automatically concatenated.
|--git.coffee # They will be also usable as modules, like require('git').
|--file-1.css # CSS files will be automatically concatenated too.
|--file-2.css # JS and CSS files may be linted before concatenation.
|--file.sass  # Compiled-to-css files will be concatenated to style file too.
|--tpl.jade   # You may have pre-compiled to JS templates. Also with `require`.

|-bower_components/ # Libraries, installed by Bower package manager
|--...stuff...      # will reside here if you use it.

|-vendor/     # All third-party libraries that are not handled by Bower
              # should be put here. JS, CSS, etc.
|--scripts/   # They will be included BEFORE your scripts automatically.
|---backbone.js
|---underscore.js

|-public/      # The directory with static files which is generated by brunch.
|--app.js      # Ready to be served via webserver.
|--app.css     # Don’t change it directly, just run `brunch watch --server`.
|--assets/     # And then all changed files in your app dir will be compiled.
|---images/

|-bower.json    # Contains info about Bower packages.
|-package.json  # Contains all brunch plugins, like jshint-brunch, css-brunch.
|-config.coffee # All params (you can concat to 2, 5, 10 files etc.)
                # are set via this config. Just simple, 15 lines-of-code config
```

### Concatenation

Brunch concatenates all your scripts in directories specified in
`config.paths.watched`. You set concatenation config, number of
output files in `config.files[type]`.

We suggest to have two files:

* `app.js` contains your application code.
* `vendor.js` contains code of libraries you depend on (e.g. jQuery).

This is better solution for browser caching than using one file,
because you change dependencies not as often as you change
your application code.

Order of file concatenation is:

1. Bower components ordered automatically.
2. Files in `config.files[type].order.before` in order you specify.
3. Files in `vendor/` directories in alphabetic order.
4. All other files in alphabetic order.
5. Files in `config.files[type].order.after` in order you specify.

All this stuff (conventions, name of out files etc) can be changed
via modifying config file.

### Conventions

Brunch also has conventions. Conventions are filters for files with special meaning. They can be changed via config.

* Files whose name start with `_` (underscore)
  are ignored by compiler. They're useful for languages like sass / stylus,
  where you import all substyles in main style file.
* Files in `assets/` dirs are copied directly to
  `public/`.
* Files in `vendor/` dirs aren't wrapped in modules.
  Module is a Common.JS / AMD abstraction that allows to simply
  get rid of global vars. For example, you have file `app/views/user_view` —
  you can load this in browser by using `require('views/user_view')`.
* Files named as `-test.<extension>` are considered as tests.
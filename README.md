# origami-build-tools

Standardised build tools for Origami modules.

## Installation

    npm install -g https://github.com/Financial-Times/origami-build-tools/tarball/master

## Usage

In your Origami module directory run:

    origami-build-tools <command>

Where `<command>` is one of:

### install

Install all dependencies:

* __Install the SASS Ruby gem__ (if your module has a `main.scss` in its root)
* __Install Bower__ globally
* __Run npm install__ (if your module has a `package.json` in its root)
* __Run bower install__ (using both the Origami Registry and the default Bower registry to resolve dependencies)

### build

* Compile `main.scss` (if it exists and is listed in `bower.json` main)
* Browserify `main.js` (if it exists and is listed in `bower.json` main)
 
Generated files are placed in a /build folder. These files should not be committed to the module repo.

### test

Runs `build`, and then also runs basic standard tests on the module:

* __SASS__ If your SASS contains a `$<module-name>-is-silent` variable, then also:
    * Check the SASS outputs no CSS by default
    * Check the SASS outputs some CSS with `$<module-name>-is-silent` set to false

### demo

Parameters:

* `<config file>`: The path to the demo config file.

Switches:

* `--local` asset paths will be for the local filesystem
* `--buildservice` assets paths will be for the Origami build service

Build static demo pages from demo source files, according to a spec in a config JSON file.

Example:

    origami-build-tool demo demos/config.json --local

Config file format:

    {
        "options": {
            "sass": "demos/src/demo.scss",
            "data": "demos/src/data.json"
        },
        "demos": {
            "demo1": {
                "template": "demos/src/demo1.mustache",
                "js": "demos/src/demo1.js"
            },
            "demo2": {
                "template": "demos/src/demo2.mustache",
                "js": "demos/src/demo2.js"
            }
        }
    }

The `demos` property of your `origami.json` file will be updated - either to list the demo files that have been created (if built for the buildservice), or emptied (if built for local).

## Watching

The commands `build`, `test` and `demo` can be run with the switch `--watch` to automatically re-run when files change.

Example:

    origami-build-tools demo /demos/src/config.json --local --watch

## Travis CI build configuration

If your module has no other tests, then your Travis build can configured with just one file:

`.travis.yml`:

    language: node_js
    node_js:
      - "0.10"
    before_install:
      - npm install -g https://github.com/Financial-Times/origami-build-tools/tarball/master
      - origami-build-tools install
    script:
      - origami-build-tools test

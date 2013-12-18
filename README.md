esri-jsapi-rjs-example
======================
This repository is an example of 2 different ways to use [r.js](http://requirejs.org/docs/optimization.html) to build the source code of applications that use the [ArcGIS API for JavaScript](https://developers.arcgis.com/en/javascript/)*:
  1. **Multi-file**: create minified copies of each of the JavaScript files and related resources (templates, CSS, images, etc)
  2. **Single-file**: create a single file which contains the minified JavaScript along with templates inlined as strings


*Only the application's local files will be inlcuded in the build output. No Esri or Dojo AMD modules will be included. The output application will still need a script tag that references the Dojo loader inlcuded in Esri's CDN copy of the JSAPI.

**NOTE: this is still more of an experiment than a boilerplate that can be used in production applications. See the [Potential Issues](#potential-issues) section below**

## Running the Examples
1. [Fork and clone the repo](https://help.github.com/articles/fork-a-repo)
2. `cd` into the `esri-jsapi-rjs-example` folder
4. Run `git submodule init` and `git submodule update`
5. Instal the dependancies with `npm install`
6. Run a Grunt build with one of the following options:
  * `grunt compile` to minify each AMD module into it's own file
  * `grunt single` to build a single file that contains all the minified AMD modules and their templates as inlined text
7. Inspect and/or browse to the contents to of the `dist` folder

## How It Works
[r.js](http://requirejs.org/docs/optimization.html) is the AMD optimizer for [RequireJS](http://requirejs.org/), and @robertd got it working for basic Dojo AMD modules, but there's always been issues with modules that use plugins (such as dojo/text to pull in template files).

@odoe figured out how to use the [RequireJS](http://requirejs.org/) plugins during the build process but use the Dojo plugins at runtime by:
  1. Including the requireJS text/domReady plugins in the project (as git submodules in this case)
  2. Referencing the above plugins in all modules by replacing `dojo/text` and `dojo/domReady` with `text` and `domReady`
  3. Adding aliases to the Dojo config to point the above reference back to the Dojo plugins as follows:
```
aliases: [['text', 'dojo/text'], ['domReady', 'dojo/domReady']]
```

The above worked for a multi-file build, but there were still issues with inlining text (templates) when creating a single-file build. @odoe figured out how to do a quick find and replace on the built file using `grunt-contrib-text-replace` that enabled the Dojo loader to load the built templates.

## Potential Issues
Other Dojo plugins like `i18n` and `has` have not been tested and it is likely that they will cause build issues.

Description
---
Ngify is a [Browserify](https://github.com/substack/node-browserify#usage)
transform for converting Angular templates to javascript using
[$templateCache](https://docs.angularjs.org/api/ng/service/$templateCache).

The file name, with extension, is used for the template name.

Either ngify.moduleName or name in package.json is used for the module name.

If you don't have a package.json, but do have an index.js and import it directly,
the folder name will be used for the module name.

Usage
---
Install ngify locally:


    npm install ngify --save-dev


Configure browserify to use this transform in package.json:


      "browserify": {
        "transform": [
          "ngify"
        ]
      }

When you require html files, they will be processed by ngify:


    require('angularTemplate.html')

The output bundle will contain a minified version of the following:


    angular.module('{{moduleName}}')
        .run([
            '$templateCache',
            function($templateCache){
                $templateCache.put('{{templateName}}','{{html}}')
            }
         ])


Where the these values are filled in:

* {{moduleName}}
    * ngify.moduleName in package.json
    * name in package.json
    * folder name

* {{templateName}} - The file name with extension

* {{html}} - The contents of the file minimized using html-minifier, here are the
    default arguments:


        {
            collapseWhitespace: true,
            conservativeCollapse: true
        }

Configuration
---
The following configuration can be set in package.json:


        "ngify": {
            "minifyArgs": {
                collapseWhitespace: true,
                conservativeCollapse: true
            },
            "moduleName":"MyModuleName"
        }

The default for minifyArgs is shown.  Anything you set will completely
override this default.

The moduleName setting will override the value for name in package.json.

Hack
---
There is one hackish feature that allows you to not define a package.json
but still use this transform.

This transform recognizes index.js as a module and will use the folder name for
the moduleName if package.json is missing.

Browserify will not apply the transform if the package is imported, but if you
import the index file, it will work:


    require("./myModule/index")

Contribute
---
Execute tests for a single run:


    npm test

Execute tests for continuous testing with changes:


    npm run-script autotest

License
---
The MIT License (MIT)

Copyright (c) 2014 majgis

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
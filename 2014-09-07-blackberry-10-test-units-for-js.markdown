Title: BlackBerry 10: Creating unit tests for JS.
Date: 2014-09-07 15:00
Tags: BB10, Blackberry 10, Grunt, QUnit, QML, JS

As you can see at [Native roadmap page](https://developer.blackberry.com/native/downloads/roadmap/) the M3 release (2.1.1 Beta) of the Momentics IDE will contain support for unit tests.
It is interesting to know what features and constraints Blackberry's solution will have.

Meanwhile, we can easily make unit tests for our JavaScript source codes now by using grunt and qunit. If you are not familiar with grunt, please visit [Getting started](http://gruntjs.com/getting-started) page on the official site.

Assume we've created new Cascades project with the following structure:

    CascadesProject/
    ├── assets/
    │   ├── main.qml
    │   ├── js/
    │   │   └── Commons.js
    ├── src/
    │   ├── applicationui.cpp
    │   ├── applicationui.hpp
    │   └── main.cpp
    ├── tests/
    │   ├── js/
    │   │   ├── Common.js
    │   │   └── about.md
    │   ├── Gruntfile.js
    │   └── package.json
    ├── bar-descriptor.xml
    ├── CascadesProject.pro
    └── icon.png

As you see extra files were added: *assets/js/Common.js* and *tests/\**.

Lets declare in the assets/js/Common.js

```
function getSum(a, b) {
	return a + b;
}
```

function we will use in assets/main.qml

```
import bb.cascades 1.2
import 'js/Common.js' as Common

Page {
    Container {
        Label {
            text: Common.getSum(10, 20)
        }
    }
}
```

Now we need to create needed test dependencies in tests/package.json according to grunt tutorial and create tests for getSum(a,b) in the tests/js/Common.js by using [syntax and assertions](http://api.qunitjs.com/category/assert/) of qunit library:

```
QUnit.test('getSum(1, 2)', function(assert) {
	var result = getSum(1, 2);
	assert.equal(result, 1 + 2);
});

QUnit.test('getSum(5, 4)', function(assert) {
	var result = getSum(5, 4);
	assert.equal(result, 5 + 4);
});
```

To run tests we should to go to the tests directory and install needed packages described in the tests/package.json file (downloaded modules will be located in the tests/node_modules directory):

```bash
[xnike@localhost tests]$ npm install grunt grunt-contrib-qunit qunitjs
npm WARN package.json cascadesproject-js-tests@0.0.1 No description
npm WARN package.json cascadesproject-js-tests@0.0.1 No repository field.
npm WARN package.json cascadesproject-js-tests@0.0.1 No README data
npm http GET https://registry.npmjs.org/grunt
npm http GET https://registry.npmjs.org/grunt-contrib-qunit
npm http GET https://registry.npmjs.org/qunitjs
npm http 304 https://registry.npmjs.org/qunitjs
npm http 200 https://registry.npmjs.org/grunt
<<<<< skipped >>>>>
qunitjs@1.15.0 node_modules/qunitjs

grunt@0.4.5 node_modules/grunt
├── which@1.0.5
├── dateformat@1.0.2-1.2.3
├── getobject@0.1.0
├── rimraf@2.2.8
├── colors@0.6.2
├── hooker@0.2.3
├── async@0.1.22
├── eventemitter2@0.4.14
├── grunt-legacy-util@0.2.0
├── exit@0.1.2
├── nopt@1.0.10 (abbrev@1.0.5)
├── lodash@0.9.2
├── coffee-script@1.3.3
├── underscore.string@2.2.1
├── iconv-lite@0.2.11
├── minimatch@0.2.14 (sigmund@1.0.0, lru-cache@2.5.0)
├── glob@3.1.21 (inherits@1.0.0, graceful-fs@1.2.3)
├── findup-sync@0.1.3 (glob@3.2.11, lodash@2.4.1)
├── grunt-legacy-log@0.1.1 (underscore.string@2.3.3, lodash@2.4.1)
└── js-yaml@2.0.5 (esprima@1.0.4, argparse@0.1.15)

grunt-contrib-qunit@0.5.2 node_modules/grunt-contrib-qunit
└── grunt-lib-phantomjs@0.6.0 (eventemitter2@0.4.14, semver@1.0.14, temporary@0.0.8, phantomjs@1.9.7-15)
```

Now we can edit our source and test code in JavaScript and check that all is ok by running grunt qunit task from the tests directory:

```bash
[xnike@localhost tests]$ grunt qunit
Running "qunit:all" (qunit) task
Testing js/Common.html ..OK
>> 2 assertions passed (17ms)
```

Full sources of the example are available [as a zip archive]({filename}/files/CascadesProject.zip).
# npmdoc-patrun

#### api documentation for  [patrun (v0.5.1)](https://github.com/rjrodger/patrun)  [![npm package](https://img.shields.io/npm/v/npmdoc-patrun.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-patrun) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-patrun.svg)](https://travis-ci.org/npmdoc/node-npmdoc-patrun)

#### A fast pattern matcher on JavaScript object properties.

[![NPM](https://nodei.co/npm/patrun.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/patrun)

- [https://npmdoc.github.io/node-npmdoc-patrun/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-patrun/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-patrun/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-patrun/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-patrun/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-patrun/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "name": "patrun",
    "description": "A fast pattern matcher on JavaScript object properties.",
    "homepage": "https://github.com/rjrodger/patrun",
    "keywords": [
        "pattern",
        "matcher",
        "object",
        "property",
        "json"
    ],
    "author": "Richard Rodger (http://richardrodger.com)",
    "repository": {
        "type": "git",
        "url": "git://github.com/rjrodger/patrun.git"
    },
    "main": "patrun.js",
    "version": "0.5.1",
    "scripts": {
        "test": "jasmine-node test",
        "browser-test": "./test.sh",
        "build": "./build.sh"
    },
    "license": "MIT",
    "files": [
        "patrun.js",
        "patrun-min.js",
        "patrun-min.map",
        "LICENSE"
    ],
    "dependencies": {
        "lodash": "4.15.0",
        "gex": "0.2.2"
    },
    "devDependencies": {
        "jasmine-node": "1.14.5",
        "serve": "1.4.0"
    }
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)

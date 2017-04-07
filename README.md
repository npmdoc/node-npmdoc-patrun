# api documentation for  [patrun (v0.5.1)](https://github.com/rjrodger/patrun)  [![npm package](https://img.shields.io/npm/v/npmdoc-patrun.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-patrun) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-patrun.svg)](https://travis-ci.org/npmdoc/node-npmdoc-patrun)
#### A fast pattern matcher on JavaScript object properties.

[![NPM](https://nodei.co/npm/patrun.png?downloads=true)](https://www.npmjs.com/package/patrun)

[![apidoc](https://npmdoc.github.io/node-npmdoc-patrun/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-patrun_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-patrun/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-patrun/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-patrun/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Richard Rodger",
        "url": "http://richardrodger.com"
    },
    "bugs": {
        "url": "https://github.com/rjrodger/patrun/issues"
    },
    "dependencies": {
        "gex": "0.2.2",
        "lodash": "4.15.0"
    },
    "description": "A fast pattern matcher on JavaScript object properties.",
    "devDependencies": {
        "jasmine-node": "1.14.5",
        "serve": "1.4.0"
    },
    "directories": {},
    "dist": {
        "shasum": "5bff3757f4f3fd2bdfb6a04d27942cbec3e12f1c",
        "tarball": "https://registry.npmjs.org/patrun/-/patrun-0.5.1.tgz"
    },
    "files": [
        "patrun.js",
        "patrun-min.js",
        "patrun-min.map",
        "LICENSE"
    ],
    "gitHead": "6ff7117f93b486bf48be8be9e79ca9ad5ab54a7b",
    "homepage": "https://github.com/rjrodger/patrun",
    "keywords": [
        "pattern",
        "matcher",
        "object",
        "property",
        "json"
    ],
    "license": "MIT",
    "main": "patrun.js",
    "maintainers": [
        {
            "name": "rjrodger",
            "email": "richard.rodger@nearform.com"
        }
    ],
    "name": "patrun",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/rjrodger/patrun.git"
    },
    "scripts": {
        "browser-test": "./test.sh",
        "build": "./build.sh",
        "test": "jasmine-node test"
    },
    "version": "0.5.1"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module patrun](#apidoc.module.patrun)
1.  [function <span class="apidocSignatureSpan"></span>patrun ( custom )](#apidoc.element.patrun.patrun)



# <a name="apidoc.module.patrun"></a>[module patrun](#apidoc.module.patrun)

#### <a name="apidoc.element.patrun.patrun"></a>[function <span class="apidocSignatureSpan"></span>patrun ( custom )](#apidoc.element.patrun.patrun)
- description and source-code
```javascript
patrun = function ( custom ) {
  custom = custom || {}

  var self = {}
  var top  = {}


  self.noConflict = function() {
    root.patrun = previous_patrun;
    return self;
  }


  self.add = function( pat, data ) {
    pat = _.clone(pat)

    var customizer = _.isFunction(custom) ? custom.call(self,pat,data) : null


    var keys = _.keys(pat), plains = [], gexers = []

    keys.forEach(function(key){
      var val = pat[key]
      if( null == val ) return;

      val = String(val)
      pat[key] = val;

      (( custom.gex && val.match(/[\*\?]/) ) ? gexers : plains ).push(key)
    })

    plains = plains.sort()
    gexers = gexers.sort()

    keys = plains.concat(gexers)


    var keymap = top, valmap

    for( var i = 0; i < keys.length; i++ ) {
      var key = keys[i]
      var val = pat[key]

      var gexer = ( custom.gex && val.match(/[\*\?]/) ) ? gex(val) : null
      if( gexer ) gexer.val$ = val

      var sort_prefix = (gexer?'1':'0')+'~'
      var sort_key = sort_prefix+key

      valmap = keymap.v

      if( valmap && sort_key == keymap.sk ) {
        add_gexer( keymap, key, gexer )
        keymap = valmap[val] || (valmap[val]={})
      }
      else if( !keymap.k ) {
        add_gexer( keymap, key, gexer )
        keymap.k = key
        keymap.sk = sort_key
        keymap.v = {}
        keymap = keymap.v[val] = {}
      }
      else if( sort_key < keymap.sk ) {
        var s = keymap.s, g = keymap.g
        keymap.s = {k:keymap.k,sk:keymap.sk,v:keymap.v}
        if( s ) keymap.s.s = s
        if( g ) keymap.s.g = g

        if( keymap.g ) keymap.g = {}
        add_gexer( keymap, key, gexer )

        keymap.k = key
        keymap.sk = sort_key
        keymap.v = {}

        keymap = keymap.v[val] = {}
      }
      else {
        valmap = keymap.v
        keymap = keymap.s || (keymap.s = {})
        i--
      }
    }

    if( void 0 !== data && keymap ) {
      keymap.d = data
      if( customizer ) {
        keymap.f = _.isFunction(customizer) ? customizer : customizer.find
        keymap.r = _.isFunction(customizer.remove) ? customizer.remove : void 0
      }
    }

    return self
  }


  function add_gexer( keymap, key, gexer ) {
    if( !gexer ) return;

    var g = (keymap.g = keymap.g || {})
    var ga = (g[key] = g[key] || [])
    ga.push( gexer )
    ga.sort( function(a,b) {
      return a.val$ < b.val$
    })
  }


  self.findexact = function( pat ) {
    return self.find( pat, true )
  }


  self.find = function( pat, exact ) {
    if( null == pat ) return null;

    var keymap    = top
    var data      = void 0 === top.d ? null : top.d
    var finalfind = top.f
    var key       = null
    var stars     = []
    var foundkeys = {}
    var patlen    = _.keys(pat).length

    do {
      key = keymap.k

      if( keymap.v ) {
        var val = pat[key]
        var nextkeymap = keymap.v[val]

        if( !nextkeymap && custom.gex && keymap.g && keymap.g[key] ) {
          var ga = keymap.g[key]
          for( var gi = 0; gi < ga.length; gi++ ) {
            if( null != ga[gi].on(val) ) {
              nextkeymap = keymap.v[ga[gi].val$]
              break;
            }
          }
        }

        if( nextkeymap ) {
          foundkeys[key]=true

          if( keymap.s ) {
            stars.push(keymap.s)
          }

          data      = void 0 === nextkeymap.d ? null : nextkeymap.d
          finalfind = nextkeymap.f
          keymap    = nextkeymap
        }
        else {
          keymap = keymap.s
        }
      }
      else {
        keymap = null
      }

      if( null == keymap && null == data && 0 < stars.length ) {
        keymap = stars.pop()
      }
    }
    while( keymap )

    if( exact ) {
      if( _.keys(foundkeys).length !== patlen ) {
        data = null
      }
    }
    else {
      // If there's root data, return as a catch all
      if( null == data && void 0 !== top.d ) {
        data = top.d
      }
    }

    if( finalfind ) {
      data = finalfind.call(self,pat,data)
    }

    return data
  }




  self.remove = function( pat ) {
    var keymap = top
    var dat ...
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)

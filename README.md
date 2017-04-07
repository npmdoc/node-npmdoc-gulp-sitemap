# api documentation for  [gulp-sitemap (v4.2.0)](https://github.com/pgilad/gulp-sitemap#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-sitemap.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-sitemap) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-sitemap.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-sitemap)
#### Generate a search engine friendly sitemap.xml using a Gulp stream

[![NPM](https://nodei.co/npm/gulp-sitemap.png?downloads=true)](https://www.npmjs.com/package/gulp-sitemap)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-sitemap/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-sitemap_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-sitemap/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-sitemap/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-sitemap/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Gilad Peleg",
        "email": "giladp007@gmail.com",
        "url": "http://giladpeleg.com"
    },
    "bugs": {
        "url": "https://github.com/pgilad/gulp-sitemap/issues"
    },
    "dependencies": {
        "chalk": "^1.1.1",
        "gulp-util": "^3.0.7",
        "lodash": "^4.6.1",
        "multimatch": "^2.0.0",
        "slash": "^1.0.0",
        "through2": "^2.0.0"
    },
    "description": "Generate a search engine friendly sitemap.xml using a Gulp stream",
    "devDependencies": {
        "gulp": "^3.8.11",
        "gulp-rename": "^1.2.2",
        "mocha": "^3.0.2",
        "should": "^11.1.0"
    },
    "directories": {},
    "dist": {
        "shasum": "eb51c28e4524329b64c2d7567aeb2f58c9931b3e",
        "tarball": "https://registry.npmjs.org/gulp-sitemap/-/gulp-sitemap-4.2.0.tgz"
    },
    "engines": {
        "node": ">=4.0.0"
    },
    "files": [
        "index.js",
        "lib"
    ],
    "gitHead": "aa196fd942b17874984cbf952c841ecc5143e683",
    "homepage": "https://github.com/pgilad/gulp-sitemap#readme",
    "keywords": [
        "gulpplugin",
        "gulp",
        "js",
        "javascript",
        "SEO",
        "sitemap",
        "sitemap.xml",
        "Google",
        "search-engine",
        "xml"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "pgilad",
            "email": "giladp007@gmail.com"
        }
    ],
    "name": "gulp-sitemap",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/pgilad/gulp-sitemap.git"
    },
    "scripts": {
        "test": "mocha -R spec test/*.spec.js",
        "watchTest": "mocha -R spec --watch test/*.spec.js"
    },
    "version": "4.2.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-sitemap](#apidoc.module.gulp-sitemap)
1.  object <span class="apidocSignatureSpan">gulp-sitemap.</span>sitemap

#### [module gulp-sitemap.sitemap](#apidoc.module.gulp-sitemap.sitemap)
1.  [function <span class="apidocSignatureSpan">gulp-sitemap.sitemap.</span>getEntryConfig (file, siteConfig)](#apidoc.element.gulp-sitemap.sitemap.getEntryConfig)
1.  [function <span class="apidocSignatureSpan">gulp-sitemap.sitemap.</span>isChangefreqValid (changefreq)](#apidoc.element.gulp-sitemap.sitemap.isChangefreqValid)
1.  [function <span class="apidocSignatureSpan">gulp-sitemap.sitemap.</span>prepareSitemap (entries, config)](#apidoc.element.gulp-sitemap.sitemap.prepareSitemap)
1.  [function <span class="apidocSignatureSpan">gulp-sitemap.sitemap.</span>processEntry (entry, siteConfig)](#apidoc.element.gulp-sitemap.sitemap.processEntry)



# <a name="apidoc.module.gulp-sitemap"></a>[module gulp-sitemap](#apidoc.module.gulp-sitemap)



# <a name="apidoc.module.gulp-sitemap.sitemap"></a>[module gulp-sitemap.sitemap](#apidoc.module.gulp-sitemap.sitemap)

#### <a name="apidoc.element.gulp-sitemap.sitemap.getEntryConfig"></a>[function <span class="apidocSignatureSpan">gulp-sitemap.sitemap.</span>getEntryConfig (file, siteConfig)](#apidoc.element.gulp-sitemap.sitemap.getEntryConfig)
- description and source-code
```javascript
function getEntryConfig(file, siteConfig) {
    var relativePath = file.relative;
    var mappingsForFile = find(siteConfig.mappings, function(item) {
        return multimatch(relativePath, item.pages).length > 0;
    }) || {};

    var entry = defaults(
        {},
        pick(mappingsForFile, allowedProperties),
        pick(siteConfig, allowedProperties)
    );

    if (entry.lastmod === null) {
        // calculate mtime manually
        entry.lastmod = file.stat && file.stat.mtime || Date.now();
    } else if (typeof entry.lastmod === 'function') {
        entry.lastmod = entry.lastmod(file);
    }

    var indexRegex = /(\/|\\|^)index\.html?$/;
    //turn index.html into -> /
    var relativeFile = relativePath.replace(indexRegex, function(string, match) {
        return match.slice(0, 1) === '/' ? '/' : '';
    }, 'i');
    //url location. Use slash to convert windows \\ or \ to /
    var adjustedFile = slash(relativeFile);
    entry.loc = siteConfig.siteUrl + adjustedFile;
    entry.file = adjustedFile;

    return entry;
}
```
- example usage
```shell
...
        return callback();
    }

    if (!firstFile) {
        firstFile = file;
    }

    var entry = sitemap.getEntryConfig(file, config);
    entries.push(entry);
    callback();
},
function (callback) {
    if (!firstFile) {
        return callback();
    }
...
```

#### <a name="apidoc.element.gulp-sitemap.sitemap.isChangefreqValid"></a>[function <span class="apidocSignatureSpan">gulp-sitemap.sitemap.</span>isChangefreqValid (changefreq)](#apidoc.element.gulp-sitemap.sitemap.isChangefreqValid)
- description and source-code
```javascript
function isChangefreqValid(changefreq) {
    // empty changefreq is valid
    if (!changefreq) {
        return true;
    }
    return includes(validChangefreqs, changefreq.toLowerCase());
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.gulp-sitemap.sitemap.prepareSitemap"></a>[function <span class="apidocSignatureSpan">gulp-sitemap.sitemap.</span>prepareSitemap (entries, config)](#apidoc.element.gulp-sitemap.sitemap.prepareSitemap)
- description and source-code
```javascript
function prepareSitemap(entries, config) {
    var entriesHref = entries.some(function(entry) {
        return entry && entry.hreflang && entry.hreflang.length;
    });
    return (entriesHref ? headerHref : header)
        .concat(entries.map(function(entry) {
            return processEntry(entry, config);
        }))
        .concat(footer)
        .join(config.newLine)
        .toString();
}
```
- example usage
```shell
...
    entries.push(entry);
    callback();
},
function (callback) {
    if (!firstFile) {
        return callback();
    }
    var contents = sitemap.prepareSitemap(entries, config);
    if (options.verbose) {
        msg = 'Files in sitemap: ' + entries.length;
        gutil.log(pluginName, msg);
    }
    //create and push new vinyl file for sitemap
    this.push(new gutil.File({
        cwd: firstFile.cwd,
...
```

#### <a name="apidoc.element.gulp-sitemap.sitemap.processEntry"></a>[function <span class="apidocSignatureSpan">gulp-sitemap.sitemap.</span>processEntry (entry, siteConfig)](#apidoc.element.gulp-sitemap.sitemap.processEntry)
- description and source-code
```javascript
function processEntry(entry, siteConfig) {
    var returnArr = [siteConfig.spacing + '<url>'];

    var loc = entry.getLoc ? entry.getLoc(siteConfig.siteUrl, entry.loc, entry) : entry.loc;
    returnArr.push(siteConfig.spacing + siteConfig.spacing + wrapTag('loc', loc));

    var lastmod = entry.lastmod;
    if (lastmod) {
        //format mtime to ISO (same as +00:00)
        lastmod = new Date(lastmod).toISOString();
        returnArr.push(siteConfig.spacing + siteConfig.spacing + wrapTag('lastmod', lastmod));
    }

    var changefreq = entry.changefreq;
    if (changefreq) {
        returnArr.push(siteConfig.spacing + siteConfig.spacing + wrapTag('changefreq', changefreq));
    }

    var priority = entry.priority;
    if (priority || priority === 0) {
        returnArr.push(siteConfig.spacing + siteConfig.spacing + wrapTag('priority', priority));
    }

    var hreflang = entry.hreflang;
    if (hreflang && hreflang.length > 0) {
        var file = entry.file;
        hreflang.forEach(function(item) {
            var href = item.getHref(siteConfig.siteUrl, file, item.lang, loc);
            var tag = '<xhtml:link rel="alternate" hreflang="' + item.lang + '" href="' + href + '" />';
            returnArr.push(siteConfig.spacing + siteConfig.spacing + tag);
        });
    }

    returnArr.push(siteConfig.spacing + '</url>');
    return returnArr.join(siteConfig.newLine);
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)

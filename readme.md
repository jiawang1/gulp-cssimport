gulp-cssimport
==============
Parses a CSS file, finds imports, grabs the content of the linked file and replaces the import statement with it.

INSTALL
-------
```sh
npm install gulp-cssimport
```

USAGE
-----
```js
var gulp = require("gulp");
var cssimport = require("gulp-cssimport");
var options = {};
gulp.task("import", function() {
	gulp.src("src/style.css")
		.pipe(cssimport(options))
		.pipe(gulp.dest("dist/"));
}); 
```

OPTIONS
-------
#### filter
RegExp, default: null (no filter).  
Process only files which match to regexp.
Any other non-matched lines will be leaved as is.  
Example:
```js
var options = {
	filter: /^http:\/\//gi // process only http urls
};
```

#### matchPattern  
String, glob pattern string. See [minimatch](https://www.npmjs.com/package/minimatch) for more details.
```js
var options = {
	matchPattern: "*.css" // process only css
};
var options2 = {
	matchPattern: "!*.{less,sass}" // all except less and sass
};
```
**Note:**
`matchPattern` will not be applied to urls (remote files, e.g. `http://fonts.googleapis.com/css?family=Tangerine`), only files.  
Urls are matched by default. If you do not want include them, use `filter` option (it is applicable to all).

#### matchOptions
Object, [minimatch](https://www.npmjs.com/package/minimatch) options for `matchPattern`.

#### limit
Number, default 5000.  
Defence from infinite recursive import.

#### extensions  
Deprecated, use `matchPattern` instead.  
String or Array, default: null (process all).
Case insensitive list of extension allowed to process.
Any other non-matched lines will be leaved as is.  
Examples:
```js
var options = {
	extensions: ["css"] // process only css
};
var options = {
	extensions: ["!less", "!sass"] // all except less and sass
};
```

#### directory
It doesn't matter if you are using gulp.

TIPS AND TRICKS
---------------
**Be more precise and do not add to src importing file without necessary:**  
```css
// main.css
@import "a.css";
@import "b.css";
```
If you will do `gulp.src("*.css")` gulp will read `a.css` and `b.css`,
and plugin also will try to read these files. It is double job.  
Do instead: `gulp.src("main.css")`

**Use filter option:**  
If you need exclude files from import, try use `filter` only option (it is faster) and avoid others.


POSTCSS
-------
There are plugins for [PostCSS](https://github.com/postcss/postcss) which do same job or even better:
* [postcss-import](https://github.com/postcss/postcss-import) inlines the stylesheets referred to by `@import` rules
* [postcss-import-url](https://github.com/unlight/postcss-import-url) inlines remote files.


SIMILAR PROJECTS
----------------
https://npmjs.org/package/gulp-coimport/  
https://npmjs.org/package/gulp-concat-css/  
https://github.com/yuguo/gulp-import-css/  
https://github.com/postcss/postcss-import  
https://www.npmjs.com/package/combine-css/  
https://github.com/suisho/gulp-cssjoin  
https://github.com/jfromaniello/css-import  
https://github.com/mariocasciaro/gulp-concat-css  


KNOWN ISSUES
------------
- Cannot resolve `@import 'foo.css' (min-width: 25em);`

CHANGELOG
---------
1.0 [12 Feb 2014]
- first release

1.1 [15 Feb 2014]
- switched to through2
- process files asynchronously

1.2 [15 Feb 2014]
- fixed processing urls

1.3 [14 Nov 2014]
- added option 'extensions'
- added option 'filter'

2.0 [30 Jun 2015]
- changed parse algorithm
- can handle recursive import
- can handle minified css files
- added option 'matchPattern'

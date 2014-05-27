[![Build Status](https://travis-ci.org/dlmanning/gulp-sass.png?branch=master)](https://travis-ci.org/dlmanning/gulp-sass)

gulp-sass
=========

SASS plugin for [gulp](https://github.com/wearefractal/gulp).

#Install

```
npm install gulp-sass
```

#Basic Usage

Something like this:

```javascript
var gulp = require('gulp');
var sass = require('gulp-sass')

gulp.task('sass', function () {
	gulp.src('./scss/*.scss')
		.pipe(sass())
		.pipe(gulp.dest('./css'));
});
```

Options passed as a hash into `sass()` will be passed along to [`node-sass`](https://github.com/andrew/node-sass)

## gulp-sass specific options

#### `errLogToConsole: true`

If you pass `errLogToConsole: true` into the options hash, sass errors will be logged to the console instead of generating a `gutil.PluginError` object. Use this option with `gulp.watch` to keep gulp from stopping every time you mess up your sass.

#### `onSuccess: callback`

Pass in your own callback to be called upon successful compilaton by node-sass. The callback has the form `callback(css)`, and is passed the compiled css as a string. Note: This *does not* prevent gulp-sass's default behavior of writing the output css file.

#### `onError: callback`

Pass in your own callback to be called upon a sass error from node-sass. The callback has the form `callback(err)`, where err is the error string generated by libsass. Note: this *does* prevent an actual `gulpPluginError` object from being created.

## Source Maps

gulp-sass can be used in tandem with [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) to generate source maps for the SASS to CSS compilation. You will need to initialize [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) prior to running the gulp-sass compiler and write the source maps after.

```javascript
var sourcemaps = require('gulp-sourcemaps');

gulp.src('./scss/*.scss')
  .pipe(sourcemaps.init());
    .pipe(sass())
  .pipe(sourcemaps.write())
  .pipe('./css');

// will write the source maps inline in the compiled CSS files
```

By default, [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) writes the source maps inline in the compiled CSS files. To write them to a separate file, specify a relative file path in the `sourcemaps.write()` function.

```javascript
var sourcemaps = require('gulp-sourcemaps');

gulp.src('./scss/*.scss')
  .pipe(sourcemaps.init());
    .pipe(sass())
  .pipe(sourcemaps.write('./maps'))
  .pipe('./css');

// will write the source maps to ./dest/css/maps
```

#Imports and Partials

gulp-sass now automatically passes along the directory of every scss file it parses as an include path for node-sass. This means that as long as you specify your includes relative to path of your scss file, everything will just work.

scss/includes/_settings.scss:

```scss
$blue: #3bbfce;
$margin: 16px;
```

scss/style.scss:

```scss
@import "includes/settings";

.content-navigation {
  border-color: $blue;
  color:
    darken($blue, 9%);
}

.border {
  padding: $margin / 2;
  margin: $margin / 2;
  border-color: $blue;
}
```


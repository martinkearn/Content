```
var gulp = require('gulp'),
    cssmin = require('gulp-minify-css'),
	jsmin = require('gulp-uglify'),
    concat = require('gulp-concat'),
    gzip = require('gulp-gzip');

gulp.task('task-cssmin', function() {
  gulp.src('css/*.css')
  .pipe(cssmin())
  .pipe(concat('bundle.css'))
  .pipe(gulp.dest("wwwroot"))
});

gulp.task('task-jsmin', function() {
  gulp.src('js/*.js')
  .pipe(jsmin())
  .pipe(concat('bundle.js')) 
  .pipe(gulp.dest("wwwroot"))
});

gulp.task("default", ['task-cssmin','task-jsmin'])
```

***

```<link href="wwwroot/bundle.css" rel="stylesheet">```

***

```<script src="wwwroot/bundle.js"></script>```

***

https://ninjacatgallery.blob.core.windows.net/static/
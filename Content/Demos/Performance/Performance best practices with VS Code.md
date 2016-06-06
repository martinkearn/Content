
# Performance best practices with VS Code
This is a summarised version of [Performance Optimisation and Tuning Lab](https://github.com/martinkearn/Web-Performance-and-Compatibility-Lab/blob/master/Performance/Performance.md); great for demos.

### Pre-reqs
* Clone https://github.com/martinkearn/Web-Performance-and-Compatibility-Lab/blob/master/Performance/Performance.md
* Chrome with YSlow plug-in

## Analyse with YSlow
1.Open Google Chrome

2.Navigate to your URL.

3.Open the YSlow extension and click Run Test

## Move CSS to the top of the file
1.Open _/performance/begin/Index.html_ in Visual Studio Code

2.Locate the CSS links at the bottom of the document

```
<!-- Bootstrap core CSS -->
<link href="css/bootstrap.css" rel="stylesheet">

<!-- Font Awesome CSS -->
<link href="css/font-awesome.css" rel="stylesheet">

<!-- Custom styles for this template -->
<link href="css/jumbotron.css" rel="stylesheet">
<link href="css/site.css" rel="stylesheet">
```  
3.Move this code to HEAD section (towards the top of the HTML file), just before the closing `</head>` line

4.Commit your changes

## Move Javascript to the bottom of the file
1.Open _/performance/begin/Index.html_ in Visual Studio Code

2.Locate the following JavaScript code in the HEAD of the document

```
<!-- Modernizr core CSS -->
<script src="js/modernizr-custom.js"></script>

<!-- Bootstrap core JavaScript-->
<script src="js/jquery-2.1.4.min.js"></script>
<script src="js/bootstrap.js"></script>
<script src="js/site.js"></script>
``` 
3.Move this code to be located at the bottom of the BODY section, just before the closing `</body>` line 

4.Commit your changes

## Replace the Twitter icon with a font
1.Open _/performance/begin/Index.html_ in Visual Studio Code

2.Find the line that looks like this at the bottom of the file

```
<a href="http://twitter.com/martinkearn"><img class="icon" src="images/twitter.png" /> @MartinKearn</a>
```

3.Replace the IMG element with this `<i class="fa fa-twitter fa-lg"></i>`. The finished line should look like ```<a href="http://twitter.com/martinkearn"><i class="fa fa-twitter fa-lg"></i> @MartinKearn</a>```

4.Zoom in on the new icon in your browser and see that it remain crisp and high quality even when fully zoomed in.

## Analyse with Google Page Speed Insights
1.Visit [https://developers.google.com/speed/pagespeed/insights/](https://developers.google.com/speed/pagespeed/insights/) in any browser

2.Enter your url and click analyse

3.Take a moment to look through the report both for Mobile and Desktop. The site should score 57/100 for mobile and 54/100 for desktop.

4.Leave the report open as we'll refer back to it throughout the lab

## Optimise images
There are many tools available to help with image optimisation including [Image Optimizer Visual Studio Extension](https://visualstudiogallery.msdn.microsoft.com/a56eddd3-d79b-48ac-8c8f-2db06ade77c3/), [Optimizilla](http://optimizilla.com/) and [SmushIt](http://imgopt.com/).

To save time, all the referenced images have been optimised, resized and stored in _/performance/begin/optimisedimages_, lets use them.

1.Copy the contents of _/performance/begin/optimisedimages_ to _/performance/begin/images_, overwriting existing files

2.Commit your changes

## Minify CSS and Javascript files with Gulp
1.Open a command prompt with administrative priviledge and navigate to your working folder ... _{some local path}/performance/begin_

2.Run `npm install gulp`.

3.Run `npm install gulp-minify-css`.

3.Run `npm install gulp-uglify`.

4.Take a look at the existing gulpfile.js

5.Run `gulp`.

6.Take a look at your _/performance/begin/_ folder structure. You'll notice a new folder called wwwroot which contains minified versions of your JS and CSS files.

7.Open _/performance/begin/Index.html_ in Visual Studio Code

8.Replace all references to `css/` to `wwwroot/css/`.

9.Replace all references to `js/` to `wwwroot/js/`.

10.Commit your changes

## Bundle CSS and Javascript files with Gulp
1.Open a command prompt with administrative priviledges and navigate to your working folder ... _{some local path}/performance/end_

2.Run `npm install gulp-concat`.

3.Run `npm install gulp-gzip`.

4.Open /performance/begin/gulpfile.js in Visual Studio Code

5.Replace the entire contents of gulpfile.js with the code below. This adds the gulp-concat plug in to the exitsing tasks to handle the bundling process.

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

6.Run `gulp`.

7.Open _/performance/begin/Index.html_ in Visual Studio Code

8.Replace all `link` elements that point to a CSS file in the HEAD and replace them with 
```
<link href="wwwroot/bundle.css" rel="stylesheet">
```

9.Remove all `link` elements that point to a JS file at the bottom of the document and replace them with 
```
<script src="wwwroot/bundle.js"></script>
```

10.Commit your changes

## Serve static files from Azure Storage
1.Open _/performance/begin/Index.html_ in Visual Studio Code

2.Replace all references to `wwwroot/` with `https://ninjacatgallery.blob.core.windows.net/static/`

3.Replace all references to `images/` with `https://ninjacatgallery.blob.core.windows.net/static/`

4.Commit your changes

## Snippets
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
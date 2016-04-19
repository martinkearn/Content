
# Gulp with ASP.NET Core 1.0
This demo installs gulp into an ASP.NET Core 1.0 project, creates a gulpfile.js, minifies css and deploy to wwwroot on build

### Pre-reqs
* Visual Studio 2015 with ASP.NET Core 1.0 RC1 or newer
* Make sure Visual Studio is using the latest NPM install, which it will not do by default. If this is not done, Autoprefixer will error in the Task Runner Explorer. See how at https://buli.waw.pl/update-node-js-integrated-with-visual-studio-2015/

## Create a new project
Visual Studio 2015 > File > New > Project > Web > ASP.NET Web Application

ASP.NET Core 1.0 Templates > Empty

## Add Packages.json (NPM)
*We need Package.json to be able to add NPM packages to the solution. Package.json is the NPM configuration file.*

Solution Explorer > Right-click on Project > Add > New Item

NPM Configuration file (search "NPM")
* Leave the default name, "package.json"

## Add Gulp NPM package
Add `"gulp": "^3.9.1"` to package.json in the `devdependencies` section

Save the file and look for the `Dependencies` "restoring" in Solution Explorer

Navigate the `Dependencies` in Solution Explorer and show that we have Gulp and all its dependencies

## Add GulpFile.js
*This is the main configuration file for Gulp. By convention it is call GulpFile.js*

Solution Explorer > Right-click on Project > Add > New Item

Gulp Configuration file (search "Gulp")
* Leave the default name, "gulpfile.js"

## Add Hello World default task
Add a default task which logs "hello world"
```
gulp.task('default', function () {
    console.log('Hello world');
});
```

## Add Binding so gulp runs on build
Solution Explorer > Right-click gulpfile.js > Task Runner Explorer

Right-click 'default' > Bindings > After build
* Note the comment at the top of gulpfile.js

Build the solution and show "hello world" in console (shown in task runner explorer)

## Add a CSS file to the project
New folder called 'CSS

New css file called 'stylesheet.css'

Add this basic CSS

```
body {
    font-family: Arial;
    font-size: 14pt;
}

h1 {
    font-size:4rem;
    font-weight: bold;
    text-decoration: underline;
}

h2 {
    font-size:3rem;
    font-weight: bold;   
}

p
{
    hyphens: auto;
}
```

## Minify css
Add 'gulp-cssmin' to packages.json and wait for it to restore
```
"devDependencies": {
"gulp": "^3.9.1",
"gulp-cssmin": "^0.1.7"
}
```
	
Add gulp-cssmin plugin to gulpfile.js below `var gulp = require('gulp');` as follows
```
var cssmin = require("gulp-cssmin");
```
	
Add a task for CSS that pipes to .wwwroot/css
```
gulp.task("cssmin_task", function () {
    gulp.src("css/*.css")
    .pipe(cssmin())
    .pipe(gulp.dest("wwwroot/css"));
});
```
	
Modify default task to include the css task
```
gulp.task('default', [ 'cssmin_task' ]);
```
	
Show the minified css file in wwwroot directory

## Add Autoprefixer
*Autoprefixer is a plugin that automates the use of CSS vendor prefixes. In this section, we'll look at hyphens which requires prefixes for some IE, Edge and Safari but is not supported on Chrome.*

Explain about the `hyphens` css property using http://caniuse.com/#feat=css-hyphens

Add 'gulp-autoprefixer' to packages.json and wait for it to restore
```
"devDependencies": {
"gulp": "^3.9.1",
"gulp-cssmin": "^0.1.7",
"gulp-autoprefixer": "^3.1.0"
}
```
	
Add gulp-cssmin plugin to gulpfile.js below `var cssmin = require("gulp-cssmin");` like this
```
var autoprefixer = require('gulp-autoprefixer');
```

Add this line beneath `gulp.src("css/*.css")` in the css_task
```
.pipe(autoprefixer())
```

Build the project

Show that the minified CSS in wwwroot now includes the various required vendor prefixes

## Add a watch so it minifies when the css changes
Add a watch task to gulpfile.js
```
gulp.task('csswatch_task', function () {
    gulp.watch('css/*.css', ['cssmin_task']);
});
```
	
Update the default task to include the watch
```
gulp.task('default', ['cssmin_task', 'csswatch_task']);
```
	
Build once so that the watch is running

Modify the css (make the body red: `color:red;`)

Show the updated minified css in wwwroot

## Snippets
```
gulp.task('default', function () {
    console.log('Hello world');
});
```

***

```
body {
    font-family: Arial;
    font-size: 14pt;
}

h1 {
    font-size:4rem;
    font-weight: bold;
    text-decoration: underline;
}

h2 {
    font-size:3rem;
    font-weight: bold;   
}

p
{
    hyphens: auto;
}
```

***

```
var gulp = require('gulp');
var cssmin = require("gulp-cssmin");
var autoprefixer = require('gulp-autoprefixer');

gulp.task("cssmin_task", function () {
    gulp.src("css/*.css")
    .pipe(autoprefixer())
    .pipe(cssmin())
    .pipe(gulp.dest("wwwroot/css"));
});

gulp.task('csswatch_task', function () {
    gulp.watch('css/*.css', ['cssmin_task']);
});

gulp.task('default', ['cssmin_task', 'csswatch_task']);
```

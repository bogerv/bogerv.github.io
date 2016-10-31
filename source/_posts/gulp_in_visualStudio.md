---
title: 在 Visual Studio 中使用 gulp
date: 2016-10-28 17:37:22
tags: [gulp, Visual Stduio]
---

## What is Gulp?
Gulp calls itself *the streaming build system*. No, this isn’t a replacement for build systems like msbuild or nant. In this case, we are talking about building the client side parts of our applications like JavaScript files, StyleSheets (CSS, SASS or LESS) and HTML files.

The basic idea with Gulp is that you use pipes to stream a set of data (usually files) through some kind of processing. As it turns out, it is pretty easy to use and is probably best described using an example.

### Installing Node and Gulp
If you don’t already have it installed, [download and install node.js](http://nodejs.org/).

> If you are using VS 2015 and ASP.NET 5, Visual Studio will install npm and gulp for you automatically.

Once node is installed, we need to install gulp using the node package manager (npm). From the command line, run
```
npm install gulp -g
```

<!--more-->

### Setting up your Visual Studio project
Rather than create a new sample project here, I’m going to use the [Hot Towel SPA template](http://www.johnpapa.net/hot-towel-angular/) from [John Papa](http://www.johnpapa.net/). First, I will create an new empty web application and then install the HotTowel.Angular nuget package.

```
Install-Package HotTowel.Angular
```

I wanted to use this template as an example because it is a perfect candidate for Gulp. The application is written using AngularJS and the code is split across 14 different JS files. From a code maintenance / readability standpoint, it is definitely good to split the code into files like this. From an application loading performance standpoint however, loading 14 separate JS files is generally not a great idea.

### Initializing our project for Gulp
First, we need to create a package.json file in the root directory of the your project. We can do this by running npm init on the command line or simply creating a file with the following contents:

```
{

   "name": "YourProjectName",

   "version": "1.0.0"

}
```

Next, we will install a few packages that we will use for this project. Run the following commands from the same folder that you added the `package.json` file.

```
npm install gulp --save-dev
npm install gulp-concat --save-dev
npm install gulp-uglify --save-dev
npm install del --save-dev
```

Note, the --save-dev option here is telling node to add these packages to a devDependencies section in the package.json file and install the packages in a node_modules folder in the current folder . At any time, you or another developer on your team can re-install all the devDependencies by simply running **`npm install`**.

> the gulpfile.js content can change whatever you want

Finally, create a gulpfile.js file in the same folder.

```
// include plug-ins
var gulp = require('gulp');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var del = require('del');

var config = {
    //Include all js files but exclude any min.js files
    src: ['app/**/*.js', '!app/**/*.min.js']
}

//delete the output file(s)
gulp.task('clean', function () {
    //del is an async function and not a gulp plugin (just standard nodejs)
    //It returns a promise, so make sure you return that from this task function
    //  so gulp knows when the delete is complete
    return del(['app/all.min.js']);
});

// Combine and minify all files from the app folder
// This tasks depends on the clean task which means gulp will ensure that the 
// Clean task is completed before running the scripts task.
gulp.task('scripts', ['clean'], function () {

    return gulp.src(config.src)
      .pipe(uglify())
      .pipe(concat('all.min.js'))
      .pipe(gulp.dest('app/'));
});

//Set a default tasks
gulp.task('default', ['scripts'], function () { });
```

Now, run gulp from the command line and you should see some output stating that scripts task is completed successfully.

This will have created an all.min.js file that contains minified JavaScript from all the js files in the app folder.

Now we include all.min.js in the Visual Studio project and replace the 14 separate script includes with a single include to the new script.

```
<script src="app/all.min.js"></script>
```

### Watching for changes
Wouldn’t it be nice gulp could automatically re-run the scripts task whenever we make a change any of our js files? Sure, we can do that!

Add the following to our `gulpfile.js`:

```
gulp.task('watch', function(){
    return gulp.watch(config.src, ['scripts']);
});
```

Now, if we run `gulp watch` from the command line, our new `watch` task will watch for changes to any of our js files. When a change is detected it will trigger the `scripts` task to execute, regenerating the all.min.js file.

You can learn more about gulp.watch [here](https://github.com/gulpjs/gulp/blob/master/docs/API.md#gulpwatchglob--opts-tasks-or-gulpwatchglob--opts-cb). Note that at this time, the built in gulp.watch has some bugs that stop it from watching new files. These should be fixed soon but in the mean time you might want to use use the popular [gulp-watch plugin](https://github.com/floatdrop/gulp-watch/blob/master/docs/readme.md#starting-tasks-on-events).

### Integrating with Visual Studio

So far, we have been working primarily in the command line. It would be nice if we could integrate with our existing Visual Studio experience.

Luckily, we can with the new [Task Runner Explorer](http://visualstudiogallery.msdn.microsoft.com/8e1b4368-4afb-467a-bc13-9650572db708) plugin. Once the plugin is installed, open the Task Runner Explorer from the View –> Other Windows –> Task Runner Explorer menu.

This window will show you all the tasks in the gulp file and allow you to bind those tasks to certain Visual Studio events. This way, we don’t need to remember to run the gulp tasks from the command line. The IDE can handle it for us.

What I like to do is bind the `watch` task to the Solution Open event and bind the `scripts` task to the *Before Build* event.

With these bindings, we can make sure that all.min.js is correctly generated when we build the application and also regenerated anytime I make changes to the js files.

The Task Runner Explorer also shows the output of any running tasks. Here you can see the output from the watch task, which is always running in the background in this configuration.

### Development vs Production
To simplify debugging, you may want to include all your individual scripts in a development build and only include the single concatenated/minified script in your production build. Take a look at my post on [Web Optimization in ASP.NET Core MVC](http://www.davepaquette.com/archive/2015/05/05/web-optimization-development-and-production-in-asp-net-mvc6.aspx) for some ideas on how to this can be accomplished. That post covers ASP.NET Core MVC but a similar approach could be used in MVC 5. With the following in your cshtml file, the individual files would be included when debug=”true” is set in your web.config. If debug=”false”, then only the single concatenated/minified file is included.

```
@if (HttpContext.Current.IsDebuggingEnabled)
{
    <!-- Bootstrapping -->
    <script src="app/app.js"></script>
    <script src="app/config.js"></script>
    <script src="app/config.exceptionHandler.js"></script>
    <script src="app/config.route.js"></script>

    <!-- common Modules -->
    <script src="app/common/common.js"></script>
    <script src="app/common/logger.js"></script>
    <script src="app/common/spinner.js"></script>

    <!-- common.bootstrap Modules -->
    <script src="app/common/bootstrap/bootstrap.dialog.js"></script>

    <!-- app -->
    <script src="app/admin/admin.js"></script>
    <script src="app/dashboard/dashboard.js"></script>
    <script src="app/layout/shell.js"></script>
    <script src="app/layout/sidebar.js"></script>

    <!-- app Services -->
    <script src="app/services/datacontext.js"></script>
    <script src="app/services/directives.js"></script>
}
else
{
    <script src="app/all.min.js"></script>
}
```

Note that this only works for cshtml files in an MVC application as it requires the Razor view engine.
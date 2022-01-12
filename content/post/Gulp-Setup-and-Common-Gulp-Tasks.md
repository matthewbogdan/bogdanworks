---
title: "GulpJS4"
date: 2022-01-11T16:30:15-05:00
featured_image: "/images/logo_gulp.png"
description: "Gulp Setup and Common Gulp Tasks"
categories: [development]
tags: [gulp]
draft: false
---
The benefit of using [Gulp](https://gulpjs.com) is to automate the creation and manage redundant tasks and workflows. These tasks and workflows may include running a local server, minifying code, optimizing images, preprocessing CSS and more. I’m going to go through setting up a project with gulp and some common gulp tasks.


## Prerequisites and Assumptions
* [Node and npm](https://nodejs.org/en/) is already installed in Mac environment 
* Assuming a project has an *src/* file folder with html, css and js files in it

## Initializing Gulp in a project Project
Moving to my project folder named _gulpjs4_, a package.json can be created. A package.json file allows you to save all of the different modules and other properties about your project. To create a package.json file you can use a couple of commands:

{{< highlight bash "" >}}npm init 	# Asks a number of initialization questions {{< / highlight >}}
{{< highlight bash "" >}}npm init -y 	# The -y allows a default initialization package file {{< / highlight >}}

Rather than just running npm init, which runs a utility that asks a number of setup questions to create a package.json file, the -y will allow us to create a default package.json file. Depending on your level of experience with Gulp, you’ll prefer one over the other. Either way you can modify the values in the package.json file after it’s creation.
 
Initial package.json file includes the following:
{{< highlight json "" >}}
{
 "name": "gulpjs4",
 "version": "1.0.0",
 "description": "This is a repository...",
 "main": "index.js",
 "scripts": {
   "test": "echo \"Error: no test specified\" && exit 1"
 },
 "repository": {
   "type": "git",
   "url": ""
 },
 "keywords": [],
 "author": "",
 "license": "MIT",
 "bugs": {
   "url": ""
 },
 "homepage": ""
}
{{< / highlight >}}

We will trim data down to the following:
{{< highlight json "" >}}
{
 "name": "gulpjs4",
 "version": "1.0.0",
 "description": "This is a repository...",
 "author": "BogdanWorks",
 "license": "MIT"
}
{{< / highlight >}}

The next step is to install the gulp command line interface so that gulp can be executed from the command line:
{{< highlight bash "" >}}npm install gulp-cli -g # gulp-cli (command line interface) -g = install globally {{< / highlight >}}


Gulp relies on packages or plug-ins. The first package you'll want to install the gulp node module package. The following command will do this:

{{< highlight bash "" >}}npm install gulp -D # Deletes any old versions of gulp and reinstalled the latest version {{< / highlight >}}

After running this command you'll notice a _node_modules_ folder has been added to your project. All Plugins and their dependencies are installed to this node_modules folder. 

You can install any gulp or nodejs specific packages as we just did installing the gulp plug-in. All Gulp.js plug-ins can be found at https://gulpjs.com/plugins

## Gulp Tasks
As mentioned above gulp is used to automate many development tasks. Gulp tasks are defined in a gulpfile.js file. Or gulp tasks can split up gulp tasks into separate files. You can create gulpjs directory with an index.js file referencing other .js files containing gulp tasks.

Tasks can com in a number of formats:
* Stream,
* Promise,
* Event emitter,
* Shield process,
* Observable,
* Callback (most popular)

To create a task, create:
src, and dest (src - where the file is, and dest - where it’s going to go)
Inside that you create a series of pipe statements (so the results of the src will be piped into some sort of module and sent to some destination of where you want the files to go)
Everyone of the tasks can also be exported (this is optional because you can create internal tasks that don’t get exported)
Can specify globs which is just a way to refer to files with wildcards, *.


## Simple Src Dest Pipe Gulp Task

{{< highlight js "" >}}
const {src, dest} = require('gulp');
 
function html(cb) {
   src('src/index.html').pipe(dest('build'));
   cb();
}
 
exports.default = html;
{{< / highlight >}}


Running the “gulp” command:

{{< highlight bash "" >}}
bworks@BWorkss-iMac gulpjs4 % gulp
[14:26:00] Using gulpfile ~/Documents/Development/gulp/CourseFiles/gulpjs4/gulpfile.js
[14:26:00] Starting 'default'...
[14:26:00] Finished 'default' after 11 ms
{{< / highlight >}}

Index.html file copied to a newly created “build” folder
{{< figure src="/images/20220112_Gulp-Common-Tasks_01-src-dest.png" link="/images/20220112_Gulp-Common-Tasks_01-src-dest.png" title="Gulp pipe source to build destination" alt="Screenshot of Visual Studio Code showing file has been copied from src folder to dest folder" >}}

## Multiple Gulp Tasks in Series
It would be more convenient to create variables for the src and build folders and we can create tasks for html files, css files and js files in the project. 

{{< highlight js "linenos=table,hl_lines=29-35,linenostart=1" >}}
const {src, dest, series} = require('gulp');
 
const origin = 'src';
const destination = 'build';
 
//find all html files at the origin and pipe to destination
function html(cb) {
   src(`${origin}/**/*.html`).pipe(dest(destination));
   cb();
}
 
//find all css files at the origin and pipe to destination
function css(cb) {
   src(`${origin}/css/**/*.css`).pipe(dest(`${destination}/css`));
   cb();
}
 
// getting each js file in multiple loactions from an array and pipe to single destination
function js(cb) {
   src([
       `${origin}/js/lib/bootstrap.bundle.min.js`,
       `${origin}/js/lib/fontawesome-all.min.js`,
       `${origin}/js/lib/jquery.min.js`,
       `${origin}/js/script.js`
   ]).pipe(dest(`${destination}/js`));
   cb();
}
 
// calling each method individually
// exports.html = html;
// exports.css = css;
// exports.js = js;
 
// calling each method in order in series
exports.default = series(html, css, js);
 
{{< / highlight >}}

You can see it's possible to call each method individually in the commented lines above and the terminal calls below:

{{< highlight bash "" >}}
bworks@BWorkss-iMac gulpjs4 % gulp html
[16:48:04] Using gulpfile ~/gulpjs4/gulpfile.js
[16:48:04] Starting 'html'...
[16:48:04] Finished 'html' after 6.49 ms
bworks@BWorkss-iMac gulpjs4 % gulp css
[16:47:52] Using gulpfile ~/gulpjs4/gulpfile.js
[16:47:52] Starting 'css'...
[16:47:52] Finished 'css' after 6.36 ms
bworks@BWorkss-iMac gulpjs4 % gulp js  
[16:55:20] Using gulpfile ~/gulpjs4/gulpfile.js
[16:55:20] Starting 'js'...
[16:55:20] Finished 'js' after 6.54 ms

{{< / highlight >}}

But with the methods in series, each will run after each other using only the default ‘gulp’ command:

{{< highlight bash "" >}}

bworks@BWorkss-iMac gulpjs4 % gulp
[17:18:37] Using gulpfile ~/gulpjs4/gulpfile.js
[17:18:37] Starting 'default'...
[17:18:37] Starting 'html'...
[17:18:37] Finished 'html' after 5.46 ms
[17:18:37] Starting 'css'...
[17:18:37] Finished 'css' after 1.68 ms
[17:18:37] Starting 'js'...
[17:18:37] Finished 'js' after 2.89 ms
[17:18:37] Finished 'default' after 12 ms
bworks@BWorkss-iMac gulpjs4 % 

{{< / highlight >}}

## Del Gulp Task
Rather than manually deleting the Build folder after each time running this command to get a fresh build we’re going to install the promise based “del” node module from npm. In the terminal run the following command

{{< highlight bash "" >}}
bworks@BWorkss-iMac gulpjs4 % npm install --save-dev del
{{< / highlight >}}

Adding to the gulpfile.js a reference to del, 

{{< highlight js "" >}}
const {src, dest, series} = require('gulp');
const del = require('del');
{{< / highlight >}}

a task to "clean" the build folder,

{{< highlight js "" >}}
async function clean(cb) {
   await del(destination);
   cb();
}
{{< / highlight >}}

and calling the method in our default,

{{< highlight js "" >}}
exports.default = series(clean, html, css, js);
{{< / highlight >}}

will clean out the folder before piping the files to the build folder

## Parallel task execution

It doesn’t really matter which one of the tasks get completed after the clean task. The html, css, and js tasks can all be done at the same time. And there’s a way to do that with gulp. Adding the [gulp *parallel* keyword](https://gulpjs.com/docs/en/api/parallel/) to our list of constants which “Combines task functions and/or composed operations into larger operations that will be executed simultaneously.”

{{< highlight js "" >}}
const {src, dest, series, parallel} = require('gulp');
.
.
.
exports.default = series(clean, parallel(html, css, js));
{{< /highlight >}}


Now running gulp the tasks occur at the same time after the clean task:

{{< highlight bash "">}}
bworks@BWorkss-iMac gulpjs4 % gulp
[11:43:09] Using gulpfile ~/Documents/Development/gulp/CourseFiles/gulpjs4/gulpfile.js
[11:43:09] Starting 'default'...
[11:43:09] Starting 'clean'...
[11:43:09] Finished 'clean' after 8.48 ms
[11:43:09] Starting 'html'...
[11:43:09] Starting 'css'...
[11:43:09] Starting 'js'...
[11:43:09] Finished 'html' after 5.34 ms
[11:43:09] Finished 'css' after 6.45 ms
[11:43:09] Finished 'js' after 7.95 ms
[11:43:10] Finished 'default' after 25 ms
{{</highlight>}}



## Browser Preview using Browsersync
Install [Browsersync](www.browsersync.io) with npm and save the dependency 
{{<highlight bash "">}}
bworks@BWorkss-iMac gulpjs4 %   npm install --save-dev browser-sync
{{</highlight>}}

Adding the reference to the node module and providing a method and method withthe following attributes:

{{<highlight js "">}}
const browsersync = require('browser-sync').create();
 
function server(cb) {
   browserSync.init({
       server: {
           baseDir: destination //destination of files
       },
       notify: false, //turns off notification window
       open: false, //dont' open new browser window every time ran
      
   });
};
{{</highlight>}}

Running the gulp command will fire up browser sync

{{<highlight bash "">}}
bworks@BWorkss-iMac gulpjs4 % gulp                                 
[11:54:30] Using gulpfile ~/Documents/Development/gulp/CourseFiles/gulpjs4/gulpfile.js
[11:54:30] Starting 'default'...
[11:54:30] Starting 'clean'...
[11:54:30] Finished 'clean' after 14 ms
[11:54:30] Starting 'html'...
[11:54:30] Starting 'css'...
[11:54:30] Starting 'js'...
[11:54:30] Starting 'server'...
[11:54:30] Finished 'html' after 6.17 ms
[11:54:30] Finished 'css' after 7.65 ms
[11:54:30] Finished 'js' after 9.24 ms
[Browsersync] Access URLs:
 ----------------------------------
       Local: http://localhost:3000
    External: http://10.0.2.15:3000
 ----------------------------------
          UI: http://localhost:3001
 UI External: http://localhost:3001
 ----------------------------------
[Browsersync] Serving files from: build
{{</highlight>}}



Running gulp typically would open a browser window automatically but since we set open to false we’ll have to open a browser to http://localhost:3000

## GulpWatch
A common task using Gulp is to watch for any changes to files during launch. 

First step is to add the watch keyword in the reference to the gulp and we’ll develop a task named watcher with a series of globs to watch folders 

Adding a reference to the watch method of the gulp library, a gulpWatch task and adding the gulpWatch task to the exported default tasks. You can see this depicted below:


{{<highlight js "" >}}
const {src, dest, series, parallel, watch} = require('gulp');
.
.
.
// looks for file changes and reloads changes
function gulpWatch(cb) {
   watch(`${origin}/**/*.html`).on('change', series(html, browsersync.reload))
   watch(`${origin}/**/*.css`).on('change', series(css, browsersync.reload))
   watch(`${origin}/**/*.js`).on('change', series(js, browsersync.reload))
   cb();
}
 
exports.default = series(clean, parallel(html, css, js, server, gulpWatch));
{{</highlight>}}

After adding the watch task to the gulpfile, you can run the gulp command and make changes to a html, css or js file and you’ll see the following update in your terminal:
{{<highlight bash "">}}
bworks@BWorkss-iMac gulpjs4 % gulp
[15:46:40] Using gulpfile ~/Documents/Development/gulp/CourseFiles/gulpjs4/gulpfile.js
[15:46:40] Starting 'default'...
[15:46:40] Starting 'clean'...
[15:46:40] Finished 'clean' after 10 ms
[15:46:40] Starting 'html'...
[15:46:40] Starting 'css'...
[15:46:40] Starting 'js'...
[15:46:40] Starting 'server'...
[15:46:40] Starting 'gulpWatch'...
[15:46:40] Finished 'html' after 6.89 ms
[15:46:40] Finished 'css' after 8.37 ms
[15:46:40] Finished 'js' after 10 ms
[15:46:40] Finished 'gulpWatch' after 32 ms
[Browsersync] Access URLs:
 ----------------------------------
       Local: http://localhost:3000
    External: http://10.0.2.15:3000
 ----------------------------------
          UI: http://localhost:3001
 UI External: http://localhost:3001
 ----------------------------------
[Browsersync] Serving files from: build
[15:47:26] Starting 'html'...
[15:47:26] Finished 'html' after 4.25 ms
[15:47:26] Starting 'browserSyncReload'...
{{</highlight>}}

You can see after Browsersync is running a change happened to an html file so the html task executes again. The benefit of doing this is to be able to update your html, css and js files and see a live update of the website immediately after you save a file.

## Next Steps

Really what I would like to do next is add some image processing and some SCSS processing. Perhaps it would be better to use [Webpack](https://webpack.js.org) and [Rollup](https://rollupjs.org/guide/en/). But in due time I'll be glad to add any of these efforts. But with these simple examples its easy to see why gulp is a great tool to do repetitive tasks.

For more information on Gulp check out https://gulpjs.com 

<!-- ## Adding image processing
It’s a good idea to standardize file names, create responsive and optimized file sizes. I’m going to creat some tasks to take care of these.

Install the following gulp libraries
gulp-rename

bworks@BWorkss-iMac gulpjs4 % npm i gulp-rename
 
+ gulp-rename@2.0.0
added 1 package from 1 contributor and audited 533 packages in 3.227s
 
19 packages are looking for funding
  run `npm fund` for details
 
found 2 high severity vulnerabilities
  run `npm audit fix` to fix them, or `npm audit` for details
bworks@BWorkss-iMac gulpjs4 % npm i change-case
-
+ change-case@4.1.2
added 16 packages from 2 contributors and audited 549 packages in 5.182s
 
19 packages are looking for funding
  run `npm fund` for details
 
found 2 high severity vulnerabilities
  run `npm audit fix` to fix them, or `npm audit` for details

## Using Sass with Gulp
In order to use [Syntactically awesome stylesheets (Sass​​)](https://sass-lang.com) there are a couple npm installations that need to happen.

Although the node processor, [node-sass](https://www.npmjs.com/package/node-sass), is currently deprecated and projects should move on to using [dart-sass](https://sass-lang.com/dart-sass), I’m going to run through the steps of how it has been used.

bworks@BWorkss-iMac gulpjs4 % npm i --save-dev gulp-sass node-sass


Syntactically awesome stylesheets (Sass) is a css preprocessor (it builds css files). Why? It helps manage a css file with many classes that may be syntactically redundant. Think of a div element that has an unordered list and list items in it. In CSS you may write

div {attributes}
div ul {attributes}
div ul li {attributes}

Already this css file has class names repeated multiple times enlarging the file and gumming up the readability of it. Sass can help with these issues allowing for the use of variables, nesting, partials, operators, modules and mixins, and you can also extend and inherit class attributes.

Sass, originally written in Ruby, now has a number of Sass libraries or wrappers. LibSass was an officially-supported implementation in C/C++ however [LibSass was deprecated](https://sass-lang.com/blog/libsass-is-deprecated) and now [Dart Sass is the primary implementation of Sass](https://sass-lang.com/dart-sass).

So let’s dive in and see how to use Dart Sass with Gulp! -->


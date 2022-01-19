---
title: "US Web Design System Hugo Template"
date: 2022-01-19T01:09:07-05:00
featured_image: '/images/sketchbook-wireframes.png'
description: ""
menu: none
categories: ["webdesign", "SSG"]
tags: [uswds, nodejs, npm, hugo, gulp]
draft: true
---

The U.S. Web Design System (USWDS) is a design system for the federal government to build accessible, mobile friendly government websites. But that doesn’t mean it can’t be used outside of government. In fact, I’m going to create a hugo theme that utilizes this design system to use on this website here. I initially created this website with the [gohugo-theme-ananke theme](https://github.com/theNewDynamic/gohugo-theme-ananke) and I liked the hero images and two column layout. But developing a USWDS theme, I can eventually  change my website to use the USWDS. The effort will help me get familiar with this client-side system and familiarize myself with changing Hugo themes.

## Assumptions
I’m assuming you have a grasp on how the hugo site generator works. If you don’t yet, go check out the [Hugo documentation](https://gohugo.io/documentation/)
I'm also assuming [Hugo](https://gohugo.io), [Nodejs and npm](https://nodejs.org/en/download/) have been installed. 

## Creating New Hugo Site and Theme
The first step in my creation of my United States Web Design System Hugo theme is to create a new hugo site.
I’m going to go to the directory where I will be creating and working on the template. In my case I created a folder titled "uswdsTheme" and I will change directory to that location:

{{<highlight bash "">}}
cd uswdsTheme
{{</highlight>}}

I will then create a new hugo website in this directory signified by the "."

{{<highlight bash "">}}
hugo new site .
{{</highlight>}}

If you have [tree](https://formulae.brew.sh/formula/tree) installed you can look at the directory structure of your newly created site

{{<highlight bash "">}}
bworks@BWorkss-iMac uswdsTheme % tree
.
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
{{</highlight>}}

These are the standard files and directories you will have starting up a new hugo website. But next we will be adding a new theme to this site with the following command:

{{<highlight bash "">}}
hugo new theme uswdsTheme
{{</highlight>}}

And using tree to check out the directory structure you can see we have a themes folder added along with some additional directories and files.

{{<highlight bash "">}}
tree                     
.
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── resources
│   └── _gen
│       ├── assets
│       └── images
├── static
└── themes
    └── uswdsTheme
        ├── LICENSE
        ├── archetypes
        │   └── default.md
        ├── layouts
        │   ├── 404.html
        │   ├── _default
        │   │   ├── baseof.html
        │   │   ├── list.html
        │   │   └── single.html
        │   ├── index.html
        │   └── partials
        │       ├── footer.html
        │       ├── head.html
        │       └── header.html
        ├── static
        │   ├── css
        │   └── js
        └── theme.toml
{{</highlight>}}

You can delete the css and js folders in the static folder as we will be moving these to an assets folder.

{{<highlight bash "">}}
rm -r themes/testTheme/static/css
rm -r themes/testTheme/static/js
{{</highlight>}}

## Installing USWDS
Ensure that you have the latest version of node and npm installed. If you do not, you can [install nodejs and npm from the web](https://nodejs.org/en/download/). 
Confirm they are installed checking your versions:
{{<highlight bash "">}}
% node -v
v16.10.0
% npm -v
8.3.0
{{</highlight>}}


The next step is to create a package.json file which will allow us to save all of the different modules and other properties about this project. Running npm init command with -y. Rather than just running npm init, which runs a utility that asks a number of setup questions to create a package.json file, the -y will allow us to create a default package.json file. Either way you can modify the values in the file after it’s creation.


{{<highlight bash "">}}
% npm init -y
Wrote to /uswdsTheme/package.json:

{
  "name": "uswdsTheme",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
{{</highlight>}}


The next step is to install USWDS with npm.


{{<highlight bash "">}}
% npm install uswds --save

added 9 packages, and audited 10 packages in 9s

found 0 vulnerabilities
npm notices …
{{</highlight>}}


Within the package.json file there now is a dependency added that looks like so,

{{<highlight json "">}}
"dependencies": {
   "uswds": "^2.13.0"
 }
{{</highlight>}}


We can also see a node_modules folder has a uswds folder in it. Do not modify anything in this folder. But seeing this file folder lets you know that you have successfully installed the uswds node package into your project.


## Compiling USWDS SASS code
We will use uswds-gulp to set up a Sass entry point. 


{{<highlight bash "">}}
% npm i autoprefixer gulp gulp-replace sass del gulp-sass gulp-sourcemaps gulp-rename gulp-svg-sprite gulp-postcss postcss postcss-csso uswds uswds-gulp@github:uswds/uswds-gulp --save-dev
{{</highlight>}}


You will notice that all of these dependencies appear in the package.json file:


{{<highlight json "">}}
 "devDependencies": {
   "autoprefixer": "^10.4.2",
   "del": "^6.0.0",
   "gulp": "^4.0.2",
   "gulp-postcss": "^9.0.1",
   "gulp-rename": "^2.0.0",
   "gulp-replace": "^1.1.3",
   "gulp-sass": "^5.1.0",
   "gulp-sourcemaps": "^3.0.0",
   "gulp-svg-sprite": "^1.5.0",
   "postcss": "^8.4.5",
   "postcss-csso": "^5.0.1",
   "sass": "^1.47.0",
   "uswds": "^2.13.0",
   "uswds-gulp": "github:uswds/uswds-gulp"
 }
{{</highlight>}}



Next configuration files need to be added to the project. Two configuration files needed to compile USWDS. 

A _gulpfile.js_ file which is used to tell gulp where your files reside and tasks / workflows related to them.
A _.browserslistrc_ file tells Gulp how to build CSS so it works in USWDS supported browsers (note USWDS will no longer support Internet Explorer 11 in USWDS version 3, to be released March 2022)


Running the following commands will copy default files from the node_modules folder to your project’s root directory (assuming you’re currently in your project folder, hence the "."):


{{<highlight bash "">}}
cp node_modules/uswds-gulp/gulpfile.js .
cp node_modules/uswds-gulp/.browserslistrc .
{{</highlight>}}


You now have the two files in your root directory:

{{<highlight bash "">}}
ls 
total 1008
.
..
.browserslistrc
.git
.gitignore
archetypes
config.toml
content
data
gulpfile.js
layouts
node_modules
Package-lock.json
package.json
resources
static
themes
{{</highlight>}}


The next step is to update the gulpfile.js to specify where the USWDS SAS, image, fonts, javascript and css will go. I’m putting JavaScript and stylesheet files in an "assets" folder at the template directory. Putting the files there will allow us to use [Hugo Pipes](https://gohugo.io/hugo-pipes/introduction) to minify and cache files for faster loading times. The other files are going into the "static" folder. My update to my gulpfile.js looks like this:


{{<highlight js "">}}
// Project Sass source directory
const PROJECT_SASS_SRC = "./themes/uswdsTheme/src/sass";
 
// Images destination
const IMG_DEST = "./themes/uswdsTheme/static/uswds/img";
 
// Fonts destination
const FONTS_DEST = "./themes/uswdsTheme/static/uswds/fonts";
 
// Javascript destination
const JS_DEST = "./themes/uswdsTheme/assets/js";
 
// Compiled CSS destination
const CSS_DEST = "./themes/uswdsTheme/assets/css";
 
// Site CSS destination
// Like the _site/assets/css directory in Jekyll, if necessary.
// If using, uncomment line 106
//const SITE_CSS_DEST = "./path/to/site/css/destination";
{{</highlight>}}

Running the following command will initialize the project by putting default files at the directories noted in gulpfile.js

{{<highlight bash "">}}
npx gulp init
{{</highlight>}}

## Updating SCSS files
The USWDS out of the box assumes you’re putting all five of the destinations in the same directory. We are deviating from this assumption, so we need to update a couple of other files so hugo can find the USWDS resources, images and fonts to be exact.
 
You'll notice in your sass directory at themes/uswdsTheme/src/sass you'll have a number of sass files. We need to update two of them. 
But first, let’s make sure we have [gulp watching for changes](/post/gulp-setup-and-common-gulp-tasks/) when we update the scss files. To do this you go to your root directory in your terminal and you’ll run the following command:
 
{{<highlight bash "">}}
gulp watch
[05:46:12] Using gulpfile ~/gulpfile.js
[05:46:12] Starting 'watch'...
[05:46:12] Starting 'build-sass'...
{{</highlight>}}
 
While the gulp watch task is running, the *_uswds-theme-general.scss* file can be updated by changing the line that says:


{{<highlight scss "" >}}
$theme-image-path: "../img";
{{</highlight>}}

and update it to:

{{<highlight scss "" >}}
$theme-image-path: "../uswds/img";
{{</highlight>}}

similarly the *_uswds-theme-typography.scss* needs to be updated from:

{{<highlight scss "" >}}
$theme-font-path: "../fonts";
{{</highlight>}}
to:

{{<highlight scss "" >}}
$theme-font-path: "../uswds/fonts";
{{</highlight>}}

## Adding references to USWDS files
To get started using the USWDS in your hugo site, you will need to update the hugo layout files, baseof.html and head.html, to include a reference to the stylesheet, JavaScript library and the JavasScript initializer.

The baseof.html file is your basic template that all pages will use. The file has code init already when creating a new hugo template. We’re adding the highlight lines below:

{{<highlight html "hl_lines=9-12" >}}
<!DOCTYPE html>
<html>
   {{- partial "head.html" . -}}
   <body>
       {{- partial "header.html" . -}}
       <div id="content">
       {{- block "main" . }}{{- end }}
       </div>
       {{- partial "footer.html" . -}}
       {{ $uswdsJsLib := resources.Get "js/uswds.js"
       | minify | fingerprint}}
       <script src="{{ $uswdsJsLib.RelPermalink }}" async></script>
   </body>
</html>
 
{{</highlight>}}

If you notice there are references to html files in the baseof.html file that already exist in the layouts > partials folder. Let's update them so we can get the theme working.

Add the following to the head.html file:

{{<highlight html "" >}}


{{</highlight>}}



<!--  !!!!!!!!!!!! STEPS BEFORE!!!!!!!!!!!!!!! -->
To ensure the USWDS was installed correctly and working correctly in your hugo site and theme, you can paste a couple of the [USWDS components](https://designsystem.digital.gov/components/overview/) and put them on a page. Going through the directories Themes > uswdsTheme > layouts > _default to the index.html, I’m going to the paste the hero section from the [USWDS landing-page template](https://designsystem.digital.gov/templates/landing-page/) along with an [accordion component](https://designsystem.digital.gov/components/accordion/) onto the page.
{{<highlight html "">}}
<section class="usa-hero" aria-label="Introduction">
{{ define "main" }}  
 
<section class="usa-hero" aria-label="Introduction">
   <div class="grid-container">
       <div class="usa-hero__callout">
           <h1 class="usa-hero__heading">
               <span class="usa-hero__heading--alt">Hero callout:</span>Bring
               attention to a project priority
           </h1>
           <p>
               Support the callout with some short explanatory text. You don’t need
               more than a couple of sentences.
           </p>
           <a class="usa-button" href="javascript:void(0)">Call to action</a>
       </div>
   </div>
</section>
 
<h3 class="site-preview-heading">Borderless</h3>
 
<div class="usa-accordion">
   <!-- Use the accurate heading level to maintain the document outline -->
   <h4 class="usa-accordion__heading">
       <button class="usa-accordion__button" aria-expanded="true" aria-controls="a1">
           First Amendment
       </button>
   </h4>
   <div id="a1" class="usa-accordion__content usa-prose">
       <p>
           Congress shall make no law respecting an establishment of religion, or
           prohibiting the free exercise thereof; or abridging the freedom of speech,
           or of the press; or the right of the people peaceably to assemble, and to
           petition the Government for a redress of grievances.
       </p>
   </div>
 
   <!-- Use the accurate heading level to maintain the document outline -->
   <h4 class="usa-accordion__heading">
       <button class="usa-accordion__button" aria-expanded="false" aria-controls="m-a5">
           Fifth Amendment
       </button>
   </h4>
   <div id="m-a5" class="usa-accordion__content usa-prose">
       <p>
           No person shall be held to answer for a capital, or otherwise infamous
           crime, unless on a presentment or indictment of a Grand Jury, except in
           cases arising in the land or naval forces, or in the Militia, when in
           actual service in time of War or public danger; nor shall any person be
           subject for the same offence to be twice put in jeopardy of life or limb;
           nor shall be compelled in any criminal case to be a witness against
           himself, nor be deprived of life, liberty, or property, without due
           process of law; nor shall private property be taken for public use,
           without just compensation.
       </p>
   </div>
</div>
 
{{end}}
 
 
 
 

{{</highlight>}}

At this point, I should be able to see these components on screen if I run my hugo website using the following command in terminal

{{<highlight bash "">}}
Hugo server
{{</highlight>}}





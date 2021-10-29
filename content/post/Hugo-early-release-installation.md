---
title: "Hugo Early Release Installation"
date: 2021-10-18T23:07:06-04:00
featured_image: "/images/logo_Hugo.png"
description: "Installing old, legacy, earlier release of Hugo static site generator"
categories: ["SSG"]
tags: ["hugo"]
draft: false
---


# Installing Early Release of Hugo
Working on a new project that uses [Hugo static site generator](https://gohugo.io), I initially installed hugo via [homebrew](https://brew.sh) with the following command

{{< highlight bash >}}
brew install hugo
{{< / highlight >}}

Attempting to launch a website built with Hugo 0.79.0 after the installing the current version (at the time of this post Hugo version 0.88.1) on the my machine resulted in the following:

### Terminal Hugo Server command warning



{{< highlight bash "linenos=table,hl_lines=2,linenostart=1" >}}
bworks@BWorkss-iMac digitalgov.gov % hugo server
WARN 2021/10/19 14:19:31 markup.defaultMarkdownHandler=blackfriday is deprecated and will be removed in a future release. See https://gohugo.io//content-management/formats/#list-of-content-formats
Start building sites … 
hugo v0.88.1+extended darwin/amd64 BuildDate=unknown

                   |  EN   
-------------------+-------
  Pages            | 9058  
  Paginator pages  |  629  
  Non-page files   |   10  
  Static files     | 2606  
  Processed images |    0  
  Aliases          | 1646  
  Sitemaps         |    1  
  Cleaned          |    0  

Built in 60491 ms
Watching for changes in /Users/bworks/Documents/Development/digitalgov.gov/{content,data,package.json,static,themes}
Watching for config changes in /Users/bworks/Documents/Development/digitalgov.gov/config.yml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at //localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
{{< / highlight >}}

While the warning,

_WARN 2021/10/15 14:13:35 markup.defaultMarkdownHandler=blackfriday is deprecated and will be removed in a future release._

is not a detrimental issue, eventually we’ll want to fix this. Per the [v0.87.0 release notes](https://github.com/gohugoio/hugo/releases/tag/v0.87.0), [blackfriday was deprecated](https://github.com/gohugoio/hugo/issues/8792).

## What if I’d like to have the old and new Hugo version installed?

The first step is to go to the [Hugo github](https://github.com/gohugoio/hugo/), and select releases at the bottom right of the screen:

{{< figure src="/images/20211018_Hugo-Installation_01-github-releases.png" link="/images/20211018_Hugo-Installation_02-gitub-releases.png" title="Hugo github page, releases button bottom right of screen" alt="Screenshot of Hugo github page" >}}

On the [Hugo release page](https://github.com/gohugoio/hugo/releases) Scroll through the pages and select the version you desire, in my case 0.79.0. Scrolling down to the bottom of the specific release page under “Assets,” you’ll need to find the version you need. I clicked and downloaded hugo_0.79.0_macOS-64bit.tar.gz as I am using a macOS Big Sur.

Editing my login shell, ~/.zprofile, I added the environment variable: export PATH=$PATH:$HOME/bin

Using the “source” command, source ~/.zprofile, will allow us to reference the login shell this session.

We can go to the ~/bin folder and extract the tar archive here with the command:
{{< highlight bash "" >}}tar -xvzf ~/Downloads/hugo_0.79.0_macOS-64bit.tar {{< / highlight >}}

After extraction, you can change the name of the executable to something like "hugo79" which will allow you to specify which hugo to run. I.E.

{{< highlight bash "" >}}
hugo server #(current version){{< / highlight >}}

{{< highlight bash "" >}}
hugo79 server #(old version){{< / highlight >}}

Now running the old version of hugo...

{{< highlight bash "linenos=table,linenostart=1" >}}
bworks@BWorkss-iMac % hugo79 server
Start building sites … 

                   |  EN   
-------------------+-------
  Pages            | 9058  
  Paginator pages  |  629  
  Non-page files   |   10  
  Static files     | 2606  
  Processed images |    0  
  Aliases          | 1646  
  Sitemaps         |    1  
  Cleaned          |    0  

Built in 48282 ms
Watching for changes in /Users/.../{content,data,package.json,static,themes}
Watching for config changes in /Users/.../config.yml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at //localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
{{< / highlight >}}

The warning no longer displays. Perhaps I can post about fixing this warning in the future, and when I do I may reference the release notes above and [Hugo content formats](https://gohugo.io/content-management/formats/).
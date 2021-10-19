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
Working on a new project that uses Hugo static site generator, I initially installed hugo via homebrew with the following command

brew install hugo

Attempting to launch a website built with Hugo 0.79.0 after the installing the current version on the my machine resulted in the following:

{{< figure src="/images/20211018_Hugo-Installation_01-Terminal-Hugo-warning.png" link="/images/20211018_Hugo-Installation_01-Terminal-Hugo-warning.png" title="Terminal Hugo Server command warning" alt="macOS terminal window stating hugo warning" >}}

While the warning,

WARN 2021/10/15 14:13:35 markup.defaultMarkdownHandler=blackfriday is deprecated and will be removed in a future release. 

is not a detrimental issue, eventually we’ll want to fix this. 

## What if I’d like to have the old and new Hugo version installed?

The first step is to go to the hugo github, https://github.com/gohugoio/hugo/, and select releases at the bottom right of the screen:

{{< figure src="/images/20211018_Hugo-Installation_02-gitub-releases.png" link="/images/20211018_Hugo-Installation_02-gitub-releases.png" title="Hugo github page, releases button bottom right of screen" alt="Screenshot of Hugo github page" >}}


Scroll through the pages and select the version you desire, in my case 0.79.0. Scrolling down to the bottom under “Assets” you’ll need to find the version you need. I clicked and downloaded hugo_0.79.0_macOS-64bit.tar.gz as I am using a macOS Big Sur.

Editing my login shell, ~/.zprofile, I added the environment variable: export PATH=$PATH:$HOME/bin

Using the “source” command, source ~/.zprofile, will allow us to reference the login shell this session.

We can go to the ~/bin folder and extract the tar archive here with the command tar -xvzf ~/Downloads/hugo_0.79.0_macOS-64bit.tar

After extraction, you can change the name of the executable to something like hugo79 which will allow you to specify which hugo to run. I.E.
hugo server (current version)
hugo79 server (old version) 

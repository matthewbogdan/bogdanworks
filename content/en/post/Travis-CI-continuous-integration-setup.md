---
title: "Travis-CI"
date: 2021-09-28T12:45:50-04:00
featured_image: "/images/20210928_Travis-CI_logo.png"
description: "Setting up Travis-CI for automated deployment for this website, bogdanworks.com"
categories: ["CICD"]
tags: ["continuous integration", "CICD"]

---

#  Travis CI for continuous integration
A common term in software development is [CICD](https://en.wikipedia.org/wiki/CI/CD) or continuous integration, continuous delivery or deployment. [Travis CI](https://www.travis-ci.com) is a tool that makes automated testing and deployment easy. It provides free continuous integration to open source projects hosted on GitHub and BitBucket. I’m going to use it to build and deploy this website as I update the [GitHub for bogdanworks.com](https://github.com/mnbogdan/bogdanworks).  

## Set Up Account on Travis-CI with GitHub account
In order to configure a project using Travis CI you need a GitHub account and to sync your GitHub project with Travis CI. You can actually use your GitHub account to sign up for Travis CI by clicking the login with GitHub button at the top of the [Travis-CI sign up page](https://app.travis-ci.com/signup):

{{< figure src="/images/20210928_Travis-CI_01-Signup.png" link="/images/20210928_Travis-CI_01-Signup.png" title="Travis-CI sign-up page" alt="Travis-CI sign-up page" >}}

## Add New GitHub repository to Travis-CI
Clicking the + icon you can add a new repository:

{{< figure src="/images/20210928_Travis-CI_02-Add-New-Repository.png" link="/images/20210928_Travis-CI_02-Add-New-Repository.png" title="Travis-CI Add new repository from GitHub" alt="Travis-CI dashboard webpage clicking plus icon" >}}

## Sync Travis-CI with GitHub
If you can not see your GitHub repositories click the “Sync account” button. When your appropriate GitHub repository shows, click the “Settings” button next to that repository.

{{< figure src="/images/20210928_Travis-CI_03-Sync-And-Settings.png" link="/images/20210928_Travis-CI_03-Sync-And-Settings.png" title="Travis-CI syncing GitHub account and opening settings" alt="Travis-CI My account page, open setting of GitHub repository" >}}

## Build Only Master Branch with Travis-CI
Turn off the build pushed pull requests” as we will only be pushing the master branch: 

{{< figure src="/images/20210928_Travis-CI_04-Build-Settings.png" link="/images/20210928_Travis-CI_04-Build-Settings.png" title="Travis-CI repository build settings" alt="Travis-CI repository build settings" >}}

## Configuring Travis-CI
Once Travis CI is synched with your GitHub account, a yml file needs to be at the root of the project titled .travis.yml with the contents similar to what is listed below:

_Note:_ make sure you have the current version of Hugo from https://github.com/gohugoio/hugo/releases

{{< gist mnbogdan 397c65e2f283f05264f0271832b3027d>}}

## Push Project Updates to GitHub
You’ll need to add and commit any updates to your repository and push them up to your GitHub repository. 

git add .
git commit -m “First TravisCI build”
git push origin

For help with git commands see [my GitCheatsheet post](/en/post/gitcheatsheet/).

After your GitHub push, Travis CI will build your project. You can also sync up your deployment with Travis CI so your updates will go to your website. I used Netlify but there are also other options like, FTP and AWS deployment. 

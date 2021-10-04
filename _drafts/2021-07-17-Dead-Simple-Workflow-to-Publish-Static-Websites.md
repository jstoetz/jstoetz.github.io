---
layout: post
title:  “Dead Simple Methods to Post to a Static Website"
by: "Jake Stoetzner"
excerpt_separator: <!--more-->
---

## Overview/Summary

## <a name="toc"></a>TOC
- [The Goal: Coding From Anywhere](#h1)
- [Heading 2](#h2)
  - [Sub-Heading 2.1](#h2.1)
  - [Sub-Heading 2.2](#h2.2)
  - [Sub-Heading 2.3](#h2.3)
- [Heading 3](#h3)
  - [Sub-Heading 3.1](#h3.1)
  - [Sub-Heading 3.2](#h3.2)
  - [Sub-Heading 3.3](#h3.3)
- [Heading 4](#h4)
  - [Sub-Heading 4.1](#h4.1)
  - [Sub-Heading 4.2](#h4.2)
  - [Sub-Heading 4.3](#h4.3)
- [Heading 5](#h5)
  - [Sub-Heading 5.1](#h5.1)
  - [Sub-Heading 5.2](#h5.2)
  - [Sub-Heading 5.3](#h5.3)
- [Notes and Research](#notes)

[Link to all R Code](https://jstoetz.github.io/code/) |
[Link to this Post's R-Code on Github](https://github.com/jstoetz/R_Code/blob/master/Blog_Post_6.r)

## <a name="h1"></a>The Goal: Coding From Anywhere
<sub>[back to top](#toc)</sub>

Having the ability to work from anywhere at any moment on anything is a blessing and a curse. In a perfect world, you would only code from the serene comfort of your modern office, sipping [bulletproof coffee](https://www.bulletproof.com/) while muzak play in the background as you focus on each task with the force of a thousand suns. But only after you have risen at 5 am to complete your sacred morning ritual where you meditate-doyoga-write1000words-lift-run-journal etc.

![](/assets/img/kermit.jpg)

The reality is that life gets in the way and you may be reading this while hunched over your phone in a minivan waiting for your your kid's tumbling practice to end so you can go make dinner. And you have to push one last change to your website *from your iPhone* or you'll lose your job.

The bottom line is that coding mobility is both necessary and useful. And with a little preparation, and the right tools, you can code **static websites** to your heart's content from just about anywhere. 

## Workflow to Publish to a Static Site

Content creation usually takes some form of the following:

1. Idea
2. Research
3. Outline
4. Write
5. Edit
6. Publish

The typical process to publish [static websites](https://www.webceo.com/blog/static-website-vs-dynamic-website-which-is-better-for-seo/) involves either:

1. **Non-CMS Hosting** - Editing **html**, **css** and **js** files on a desktop or laptop computer and manually uploading them to a host via ftp.
2. **CMS Hosting** - Logging into a Content Management System (CMS) like Wordpress or Shopify on the web, making manual changes in a [WYSIWYG](https://blog.hubspot.com/website/best-wysiwyg-html-editor) editor and publishing the site. 

[Research](https://w3techs.com/technologies/overview/content_management) shows that about one-third of websites use a non-CMS solution while the other two-third use a CMS solution. 

The goal of this post is to review ways to simplify and automate as much as possible of Step 4:Writing and Step 6:Publishing of the content creation process while retaining the ability to publish and host on non-CMS platforms from any device (including mobile).

When possible, free and open-source solutions are used.

The main tools that are discussed are:

1. Text Editor
	- 	I won’t spend a lot of time trying to sway you to choose one text editor over another; use one you are comfortable with, can save different file types (**html**, **md**, **txt** etc) and that syncs with cloud storage. 
	- Good roundup review of iOS text editors: [iTextEditors](https://brettterpstra.com/ios-text-editors/)
	- 	My favorites so far: [Textastic](https://www.textasticapp.com/), [iAWriter](https://ia.net/writer) and [Drafts](https://getdrafts.com/). 
2. Cloud File Storage
	- 	I prefer [Dropbox](dropbox.com) but any should be fine (except iCloud).
3. Service to Sync With Host
	- [GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)
	- Netlify
	- [AWS CodePipeline](https://aws.amazon.com/codepipeline/) 
4. Hosting Service

The Workflows can be summarized as:

1. Dropbox > Zapier > Amazon S3
2. Dropbox > Netlify
3. Github > CodePipeline > Amazon S3
4. Github > Amazon Amplify/Github Pages

## Zapier Workflow

[Zapier](https://zapier.com/app/dashboard) is a web-based service that allows apps to communicate with each other.

The Zapier Workflow is as follows:

```
Text Editor > File Storage > Zapier > Host
```

An example of a one of these workflows is:

```
iA Writer > Dropbox > Zapier > Amazon S3
```

You would write on [iA Writer](https://ia.net/writer) (on iOS, Mac, Windows or Android), save files to [Dropbox](http://dropbox.com) and use Zapier to sync with [Amazon S3](https://aws.amazon.com/s3/) as your hosting provider. 

## Netlify Workflow

This workflow was inspired by the article [Combining Netlify with Dropbox For a One-Click Publishing Process | Netlify](https://www.netlify.com/blog/2018/10/15/combining-netlify-with-dropbox-for-a-one-click-publishing-process/). The author built [this example site](https://netlibox.netlify.app) using the process he outlines in the article.

As a broad overview, the process looks like this:

```
Text Editor > File Storage > Site Generator > Host
```

or more specifically:

```
Text Editor > Dropbox > Netlify (builds the site automatically from a GitHub Repo using Jekyll and Host on Netlify)
```

You can read the article and check out the [Netlibox GitHub repo](https://github.com/jimniels/netlibox), but the process is as follows:

1. Setup a new repository for [GitHub](https://docs.github.com/en/get-started/quickstart) to maintain your site files. 
	* GitHub is the code repository
	* [GitHub Pages](https://pages.github.com/) can also be used to host a static site. A [GitHub Page Site](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site) can be created for a personal or project site using [Jekyll](https://jekyllrb.com/docs/) as a static site generator.
2. Create a [Netlify site](https://app.netlify.com/start) that is connected to the repository you setup in Step #1.
3. Login to your [Dropbox App Console](https://www.dropbox.com/developers/reference/getting-started) and create a new app that is connected to a single folder. 
4. Create a Dropbox Access token. This can be found in the Developer App Console under ```Oauth2```; click the "Generate" access token button.
5. Setup a webhook that triggers each time there are changes in your Dropbox folder. Follow the [instructions in the Netlibox repo](https://github.com/jimniels/netlibox).
6. The key to this method is the custom script that builds a ```_posts``` folder from Dropbox **before** running the Jekyll build:

> rather than have a _posts folder with a bunch of markdown files (as dictated by Jekyll) directly in my Git repo, I create that folder and its contents at build time by pulling all my plain-text markdown files from Dropbox. This is done via a custom node script that gets run before Jekyll does its thing. For example, rather than your build command being jekyll build you would do something like node fetch-posts.js && jekyll build. [From the article](https://www.netlify.com/blog/2018/10/15/combining-netlify-with-dropbox-for-a-one-click-publishing-process/).

### <a name="h1.1"></a><!--Sub-Heading 1.1-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h1.2"></a><!--Sub-Heading 1.2-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h1.3"></a><!--Sub-Heading 1.3-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->

## <a name="h2"></a><!-- Heading 2 -->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h2.1"></a><!--Sub-Heading 2.1-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h2.2"></a><!--Sub-Heading 2.2-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h2.3"></a><!--Sub-Heading 2.3-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->

## <a name="h3"></a><!-- Heading 3 -->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h3.1"></a><!--Sub-Heading 3.1-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h3.2"></a><!--Sub-Heading 3.2-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h3.3"></a><!--Sub-Heading 3.3-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->

## <a name="h4"></a><!-- Heading 4 -->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h4.1"></a><!--Sub-Heading 4.1-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h4.2"></a><!--Sub-Heading 4.2-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h4.3"></a><!--Sub-Heading 4.3-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->

## <a name="h5"></a><!-- Heading 5 -->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h5.1"></a><!--Sub-Heading 5.1-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h5.2"></a><!--Sub-Heading 5.2-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->
### <a name="h5.3"></a><!--Sub-Heading 5.3-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->

## <a name="notes"></a>Notes and Research
<sub>[jump back to top](#toc)</sub>

  - [Scheduling Posts and Microblogs With Jekyll](https://joebuhlig.com/scheduling-posts-and-microblogs-with-jekyll/) - [DB](https://www.dropbox.com/s/kng9yy47lcztewp/Scheduling%20Posts%20and%20Microblogs%20With%20Jekyll.pdf?dl=0) 
  - [RClone](https://www.rclone.org)
	  - Rclone is a command line program to manage files on cloud storage.
  - [GitHub - timhodson/google-drive-file-download: A simple script to fetch files from a google drive folder and upload to S3](https://github.com/timhodson/google-drive-file-download)
	  - a python tool to download files from a google drive folder, and to then upload the files to S3.
	- 
	
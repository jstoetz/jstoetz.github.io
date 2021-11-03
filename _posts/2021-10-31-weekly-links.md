---
layout: post
title: Links for the Week of 2021-10-31
by: "Jake Stoetzner"
categories: [links]
---

# Links for the Week of 2021-10-31

Here are some links I covered in the past week.

## Table of Contents
<hr/>

- General
- Automating File Organization
- Exporting Apple Notes and The Plain Text Movement
- App to Post to Facebook
- R on AWS

## General
<hr />

### Hosting images with Dropbox

[Link](https://support.geekseller.com/knowledgebase/hosting-images-with-dropbox/) - [DB](https://www.dropbox.com/s/lgua88ejq7zu5fq/2021.10.25%20-%20Hosting%20images%20with%20Dropbox.pdf?dl=0)

A great trick to host images on Dropbox.

1. Get a shareable link to the image in Dropbox. It will have the following structure: ```www.dropbox.com/s/HOST_ID/filename.extension```
2. Replace ```dropbox.com``` with ```dl.dropboxusercontent.com```. This effectively removes the viewing frame around the image you see at ```dropbox.com```.
3. Insert the link in markdown like:

```
![](https://www.dl.dropboxusercontent.com/s/HOST_ID/filename.extension)
```

Dale and Brennan approve this method and [ride majestic, translucent steeds, shooting flaming arrows across the bridge of Hemdale](https://m.youtube.com/watch?v=T6-wCiXWaJU) while hosted on Dropbox:

![](https://www.dl.dropboxusercontent.com/s/dw3q221np98v3rj/Stepbrothers-Love-R.jpg?dl=0)

### PDF Newspaper - from fivefilters.org

[Link](https://pdf.fivefilters.org/)

A nifty little tool to make a **PDF** of your favorite RSS feeds appear like a printed newspaper.

### Learn to Trade Options Now: Rolling Options Out, Up and Down

[Link](https://www.schaeffersresearch.com/education/options-basics/options-trading-mechanics/rolling-options-out-up-and-down) - [DB](https://www.dropbox.com/s/xxf1826k356d12j/2021.10.28%20-%20Learn%20to%20Trade%20Options%20Now%3A%20Rolling%20Options%20Out%2C%20Up%20and%20Down.pdf?dl=0)

## Automating File Organization
<hr />

### Automating my workflow with Python #1: File Organizer

[Link](https://dev.to/gagangulyani/automating-my-workflow-with-python-1-file-organizer-40po) - [DB](https://www.dropbox.com/s/7bnch1dkpirjv9h/2021.10.28%20-%20Automating%20my%20workflow%20with%20Python%20%231%3A%20File%20Organizer.pdf?dl=0)

### organize - the file organization tool

[Link](https://github.com/tfeldmann/organize)

## Exporting Apple Notes and The Plain Text Movement
<hr />

Apple notes, for all its sleekness, syncing capabilities and ease of use, only allows you to [export notes on Mac](https://support.apple.com/guide/notes/import-and-export-notes-not201900c07/mac) in **PDF** format. Getting them out of the Apple ecosystem is like breaking Sean Connery out of Alcatraz with Nic Cage. Doable, but not without its challenges.

![](https://www.dl.dropboxusercontent.com/s/of5v54vmbxqtd43/Sean-approves.gif?dl=0)

A common battle cry in productivity writing is to warn of the dangers of merely working *on* your productivity system versus *actually being productive*. The line between the two can be razor thin at times. If re-evaluating your iOS note taking app for the 430th time saves you 5 minutes, then *by god you re-evaluate the app*. This has created a fervent push in the hearts and minds of productivity enthusiasts (myself included) to separate themselves from the shiny tools that cause them to deteriorate into self-serving, unproductive navel-gazers.

One such tactic is to move to plain text files for collecting, recording and creating notes. To the productivity-minimlist-elite, nothing could be as bourgeois as the humble **txt** file. The promise of perpetual compatibility combined with endless flexibility of expression is the software equivalent of everlasting life. And for those of us who have committed the cardinal sin of locking their productivity system into a proprietary structure, a plain text system feels like salvation. Freed from wearing our gold (Apple) or green (Evernote) letters like [Hester Prynne](https://en.m.wikipedia.org/wiki/Hester_Prynne), we can finally reach a Zen-like level of  effortless productivity with plain text. Or at least we can use it as a reason to procrastinate from actually doing the work.

### Why I use plain text files for my notes

[Link](https://www.chrissy.dev/notes/why-I-use-plain-text-files-for-my-notes/) - [DB](https://www.dropbox.com/s/tqi1sn823rwod0e/2021.10.25%20-%20Why%20I%20use%20plain%20text%20files%20for%20my%20notes.pdf?dl=0)

Sound reasoning to look past the hype of note taking apps and embrace the simplicity of the humble **txt** file since "[t]he main reasons I take notes are mainly about jotting down an idea, drafting something very roughly, or archiving content." Sacrificing flexibility and portability in the name of UI seems downright silly when you get right down to it.

### Where are Notes Stored on Mac?

[Link](https://osxdaily.com/2020/01/15/where-notes-stored-locally-mac/) - [DB](https://www.dropbox.com/s/7aopt1yfjhk5zcc/2021.10.25%20-%20Where%20are%20Notes%20Stored%20on%20Mac%3F.pdf?dl=0)

Finding where **your GD note** is stored on **your GD Mac computer** turns out to be harder than you think.

### Exporter for Notes.app - Export or Backup Notes from Apple's OSX Notes app to Plain Text .txt

[Link](http://writeapp.net/notesexporter/)

Never tried this but plan to.

### How to export Apple Notes as plain text files

[Link](https://ldstephens.net/2017/11/26/how-to-export-apple-notes-as-plain-text-files/) - [DB](https://www.dropbox.com/s/qeb5qzljujit84a/2021.10.25%20-%20How%20to%20export%20Apple%20Notes%20as%20plain%20text%20files.pdf?dl=0)

### Migrate from Apple Notes - Bear App

[Link](https://bear.app/faq/Import%20&%20export/Migrate%20from%20Apple%20Notes/) - [DB](https://www.dropbox.com/s/6x59r35l0rnor6z/2021.10.25%20-%20Migrate%20from%20Apple%20Notes%20%7C%20FAQ%20%26%20Support%20%7C%20Bear%20App.pdf?dl=0)

A nice little Automator script by the makers of Bear that you run on your Mac to export notes into **HTML** format. Theoretically, you could then convert those notes into **txt** format.

## App to Post to Facebook
<hr />

### How to Post on Your Facebook Page With the Graph API

[Link](https://medium.com/swlh/how-to-post-on-your-facebook-page-with-the-graph-api-701b2a79b056) - [DB](https://www.dropbox.com/s/sexycw05b59sg5x/2021.10.26%20-%20How%20to%20Post%20on%20Your%20Facebook%20Page%20With%20the%20Graph%20API.pdf?dl=0)

## R on AWS
<hr/>

### Getting started with R on Amazon Web Services | Amazon Web Services

[Link](https://aws.amazon.com/blogs/opensource/getting-started-with-r-on-amazon-web-services/) - [DB](https://www.dropbox.com/s/bol4nqcx2wak6gb/2021.10.28%20-%20Getting%20started%20with%20R%20on%20Amazon%20Web%20Services%20%7C%20Amazon%20Web%20Services.pdf?dl=0)

### Getting Started with Amazon EMR - Big Data Platform - Amazon Web Services

[Link](https://aws.amazon.com/emr/getting-started/) - [DB](https://www.dropbox.com/s/e32gxmmb5w1ulbu/2021.10.28%20-%20Getting%20Started%20with%20Amazon%20EMR%20-%20Big%20Data%20Platform%20-%20Amazon%20Web%20Services.pdf?dl=0)

### AWS EC2: How to process queue with 100 parallel ec2 instances? - Stack Overflow

[Link](https://stackoverflow.com/questions/23163308/aws-ec2-how-to-process-queue-with-100-parallel-ec2-instances) - [DB](https://www.dropbox.com/s/zywouatenncy9tc/2021.10.28%20-%20AWS%20EC2%3A%20How%20to%20process%20queue%20with%20100%20parallel%20ec2%20instances%3F%20-%20Stack%20Overflow.pdf?dl=0)

### downloading a file from Internet into S3 bucket - Stack Overflow

[Link](https://stackoverflow.com/questions/19241671/downloading-a-file-from-internet-into-s3-bucket) - [DB](https://www.dropbox.com/s/udiug5j1s20ldqs/2021.10.28%20-%20downloading%20a%20file%20from%20Internet%20into%20S3%20bucket%20-%20Stack%20Overflow.pdf?dl=0)

### AWS Articles

[Link](https://aws.amazon.com/articles/word-count-example/?tag=articles%23keywords%23elastic-mapreduce) - [DB](https://www.dropbox.com/s/owo1oys9d3mxcnj/2021.10.28%20-%20AWS%20Articles.pdf?dl=0)

### Deploying  Mozilla DeepSpeech models to AWS Lambda using Serverless

[Link](https://lukasgrasse.medium.com/deploying-mozilla-deepspeech-models-to-aws-lambda-using-serverless-b5405ccd546b) - [DB](https://www.dropbox.com/s/qgacs6xu2aemy0m/2021.10.28%20-%20Deploying%20%20Mozilla%20DeepSpeech%20models%20to%20AWS%20Lambda%20using%20Serverless.pdf?dl=0)

### CloudFormation Templates

[Link](https://aws.amazon.com/cloudformation/resources/templates/) - [DB](https://www.dropbox.com/s/awcvr7u89jnl3fm/2021.10.28%20-%20CloudFormation%20Templates.pdf?dl=0)

[Link](https://aws.amazon.com/articles/word-count-example/?tag=articles%23keywords%23elastic-mapreduce) - [DB](https://www.dropbox.com/s/owo1oys9d3mxcnj/2021.10.28%20-%20AWS%20Articles.pdf?dl=0)

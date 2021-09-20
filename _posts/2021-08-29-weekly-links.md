---
layout: post
title: Links for the Week of 2021-08-29
by: "Jake Stoetzner"
categories: [links]
---

# Links for the Week of 2021-08-29

Here are some links I covered in the past week.

## Table of Contents
<hr/>

- General
- Why I Hate Calendars
- Algo Trading
- Working With Audio Files and DeepSpeech in R and Python
- Developer Documentation as a Learning Tool
- AWS Tools: Amazon EMR

## General
<hr />

### Setting up a new MacBook Pro

[Link](https://www.garrickadenbuie.com/blog/setting-up-a-new-macbook-pro/) - [DB](https://www.dropbox.com/s/8y5ad1uidcuwct5/2021.08.23-Setting-up-a-new-MacBook-Pro.pdf?dl=0)

A complete setup of a MacBook Pro by an R data scientist.  Check out the notes he made while waiting for Big Sur to download:  [Link](https://gist.github.com/gadenbuie/a14cab3d075901d8b25cbaf9e1f1fa7d) - [DB](https://www.dropbox.com/s/efbzrob068rmasj/2021.08.23-a14cab3d075901d8b25cbaf9e1f1fa7d.pdf?dl=0).

### Setting up a Windows Machine - Scott Hanselmans 2021 Ultimate Developer and Power Users Tool List for Windows

[Link](https://www.hanselman.com/blog/scott-hanselmans-2021-ultimate-developer-and-power-users-tool-list-for-windows) - [DB](https://www.dropbox.com/s/z17c446ss2swgtd/2021.08.24-scott-hanselmans-2021-ultimate-developer-and-power-users-tool-list-for-windows.pdf?dl=0)

### R Stock Price Alert Tool

[Link](https://github.com/bradlindblad/R-stock-price-alert-tool/blob/master/stock%20price%20alert%20tool.R) - [DB](https://www.dropbox.com/s/nr5abuwtkcx14gw/2021.08.24-stock%2520price%2520alert%2520tool.R.pdf?dl=0)

Set-up windows task scheduler to run this script every night to get an email with price alerts for multiple stocks.

### rVest Webscraping Cheat Sheet

[Link](https://github.com/yusuzech/r-web-scraping-cheat-sheet) - [DB](https://www.dropbox.com/s/g3ftncz92z9u13s/2021.08.23-r-web-scraping-cheat-sheet.pdf?dl=0)

<small>tags: cheatsheet</small>

### R Markdown Websites

[Link](https://garrettgman.github.io/rmarkdown/rmarkdown_websites.html) - [DB](https://www.dropbox.com/s/ltmu019vh21hm1c/2021.08.24-rmarkdown_websites.html.pdf?dl=0)

See also the rmarkdown site generator section of the rmarkdown book:  [Link](https://bookdown.org/yihui/rmarkdown/rmarkdown-site.html#html-generation) - [DB](https://www.dropbox.com/s/o9vnnsk5uivk5bb/2021.08.24-rmarkdown-site.html%23html-generation.pdf?dl=0).

### 2020 Wage Data for the State of Kansas

[Link](https://www.bls.gov/oes/current/oes_ks.htm) - [DB](https://www.dropbox.com/s/t4z4kphc5cyrqk3/2021.08.24-oes_ks.htm.pdf?dl=0)

### 5 Metrics Every Dental Practice Should Be Tracking

[Link](https://www.dentistryiq.com/practice-management/financial/article/14035794/5-metrics-every-dental-practice-should-be-tracking) - [DB](https://www.dropbox.com/s/rxmguhs4k7wkyqn/2021.08.26%20-%20.pdf?dl=0)

### 58 Best Google Analytics Books of All Time

[Link](https://bookauthority.org/books/best-google-analytics-books) - [DB](https://www.dropbox.com/s/8wz5al9ju02urks/2021.08.26%20-%2058%20Best%20Google%20Analytics%20Books%20of%20All%20Time.pdf?dl=0)

## Why I Hate Calendars
<hr />

I am **terrible** at keeping a calendar (just ask my wife).  Calendars are obviously useful tools but something in my brain stops me from using them effectively.

![Barkley thinks I am turrible](https://media.giphy.com/media/Y4n2kBbEYjeGIvQSA1/giphy.gif "Charles Doesn't Approve of My Calendar Usage")

I have issues with calendars:

  1. **New Events Come At You Randomly and Are Hard to Process**. You don't know when or where you will be when you need to schedule an event on your calendar.  For example, you are dropping your kid off at school, and he tells you he has to be at a basketball practice tomorrow at 7 p.m. before fleeing the car.  You vaguely remember you have a fantasy football draft at the same time but you are not sure.

  If you don't have your beloved calendar in front of you, it's a challenge to decide how to process this information: Are you available? Do you have a conflict? Is it likely a conflict will arise between then and now? If you have a conflict, which event should you prioritize (goodbye, fantasy football)? Is there a *better* alternative option? Who is available to cover this new appointment? Can't your kid just play video games instead?

  2. **There is No Metric**.  I get that we [only have so many years/months/weeks/days/minutes/seconds in our lifetime](https://waitbutwhy.com/2015/12/the-tail-end.html), and that calendars exist to help organize, plan and prioritize those blocks of time.  But calendars are basically just a data table with a clean shirt on. They log the data and then just sit there smiling aimlessly, all the while looking clean and organized and pretty.  It's lipstick on a pig.  Calendars leave it to the user to decide what to do with all this data.  You can manually schedule, re-schedule or delete future events but most of the time there is no way to tell whether these actions are effective in meeting your goals.

![](https://i.redd.it/7c9c69ce9uf31.jpg)

  3. **Calendars Can't Predict the Future (Or Measure the Past)**.  Calendars don't remember where you have been, how you got where you are or forecast where you are going in the future.

  4.  **Calendars Are Bad at Categorization**.  Unless you adopt your own system, there is no way to tell where each entry should be categorized in relation to anything else.

In light of all of these problems, I am seeking a better way to plan my time blocks.  Below are some links that have helped me along the way.

### A Mini Calendar in Your R Console

[Link](https://www.garrickadenbuie.com/blog/r-console-calendar/) - [DB](https://www.dropbox.com/s/hvbp3gar3y2re99/2021.08.23-r-console-calendar.pdf?dl=0)

How to get a calendar for any range of ```month-years``` in the R console.

![](/img/Screen Shot 2021-08-23 at 3.09.43 PM.png)

### Why You Should Use a Calendar

[Link](https://simplyorganizedyou.com/why-should-you-use-a-calendar/) - [DB](https://www.dropbox.com/s/dnl23aig9tpm0m1/2021.08.23-why-should-you-use-a-calendar.pdf?dl=0)

## Algorithmic Trading
<hr />

### 24x5 AI Stock Trading agent to predict stock prices | Live trading

[Link](https://towardsdatascience.com/24x5-stock-trading-agent-to-predict-stock-prices-with-deep-learning-with-deployment-c15570720ae9) - [DB](https://www.dropbox.com/s/qfqu2h9fa92m9xu/2021.08.24%20-%2024x5%20AI%20Stock%20Trading%20agent%20to%20predict%20stock%20prices%20%7C%20Live%20trading.pdf?dl=0)

In-depth review of creating an algo trading bot hosted on AWS. Mostly python but really good information.

### Algorithmic Financial Trading with Deep Convolutional Neural Networks: Time Series to Image Conversion Approach

[Link](https://www.researchgate.net/profile/Omer-Sezer/publication/324802031_Algorithmic_Financial_Trading_with_Deep_Convolutional_Neural_Networks_Time_Series_to_Image_Conversion_Approach/links/5ae4ade9a6fdcc3bea95d2fd/Algorithmic-Financial-Trading-with-Deep-Convolutional-Neural-Networks-Time-Series-to-Image-Conversion-Approach.pdf?origin=publication_detail)

### Python: I have tested a Trading Mathematical Technic in RealTime.

[Link](https://medium.com/analytics-vidhya/python-i-have-tested-a-trading-mathematical-technic-in-realtime-658a80381151) - [DB](https://www.dropbox.com/s/kxpmxszip016rpf/2021.08.24%20-%20Python%3A%20I%20have%20tested%20a%20Trading%20Mathematical%20Technic%20in%20RealTime..pdf?dl=0)

### A Step-by-Step Implementation of a Trading Strategy in Python Using ARIMA Garch Models

[Link](https://medium.com/analytics-vidhya/a-step-by-step-implementation-of-a-trading-strategy-in-python-using-arima-garch-models-b622e5b3aa39) - [DB](https://www.dropbox.com/s/2xl204frh1sgi4x/2021.08.24-a-step-by-step-implementation-of-a-trading-strategy-in-python-using-arima-garch-models-b622e5b3aa39.pdf?dl=0)

### Forecasting Stock Returns Using the ARIMA Model

[Link](https://blog.quantinsti.com/forecasting-stock-returns-using-arima-model/) - [DB](https://www.dropbox.com/s/0hhosudaxgvf9ba/2021.08.24-forecasting-stock-returns-using-arima-model.pdf?dl=0)

## Working With Audio Files and DeepSpeech in R and Python
<hr/>

### Efficiently split a large audio file in R - Stack Overflow

[Link](https://stackoverflow.com/questions/20713513/efficiently-split-a-large-audio-file-in-r) - [DB](https://www.dropbox.com/s/eep4ypy3sqmyz9q/2021.08.24%20-%20Efficiently%20split%20a%20large%20audio%20file%20in%20R%20-%20Stack%20Overflow.pdf?dl=0)

### DeepSpeech-examples/autosub at r0.9 · mozilla/DeepSpeech-examples

[Link](https://github.com/mozilla/DeepSpeech-examples/tree/r0.9/autosub) - [DB](https://www.dropbox.com/s/s48xan6ak6i0546/2021.08.24%20-%20DeepSpeech-examples%3Aautosub%20at%20r0.9%20%C2%B7%20mozilla%3ADeepSpeech-examples.pdf?dl=0)

Excellent code that uses the [DeepSpeech](https://github.com/mozilla/DeepSpeech) to create subtitles for videos. This code helped immensely when trying to split audio files into chunks to be transcribed by DeepSpeech. One tip was using the [pyAudioAnalysis library](https://github.com/tyiannak/pyAudioAnalysis) to split up audio files at "silent" breaks. That way there are no words that are cut-off.

### How to build Python transcriber using Mozilla DeepSpeech

[Link](https://www.slanglabs.in/blog/how-to-build-python-transcriber-using-mozilla-deepspeech) - [DB](https://www.dropbox.com/s/myt9bs6oi82hd12/2021.08.24-how-to-build-python-transcriber-using-mozilla-deepspeech.pdf?dl=0)

Python code to build a DeepSpeech API and Batch API to build  a streaming transcriber model.  Based on DeepSpeech 0.6 but very relevant.

### Deploying Mozilla DeepSpeech models to AWS Lambda using Serverless

[Link](https://medium.com/@lukasgrasse/deploying-mozilla-deepspeech-models-to-aws-lambda-using-serverless-b5405ccd546b) - [DB](https://www.dropbox.com/s/pto8i4gv5omjl6c/2021.08.24-deploying-mozilla-deepspeech-models-to-aws-lambda-using-serverless-b5405ccd546b.pdf?dl=0)

This one is outside my comfort zone but still a good read.

## Developer Documentation as a Learning Tool
<hr/>

As a new coder, one of the (many) challenges I encounter is understanding the workflow and process that it takes to develop **good code**. What decisions does someone developing an R package make along the way? How do they decide what to include and what not to include? How do they utilize the architecture of the project to support their end goal?

A new writer could review the works of other great authors, dissecting vocabulary, sentence structure, plotting, pacing, opening lines.  [Some do it manually](https://marcykennedy.com/2016/04/dissecting-books-reading-writer-part-1/).  [Data scientists have even statistically analyzed the great authors](https://www.smithsonianmag.com/arts-culture/one-writer-used-statistics-reveal-secrets-what-makes-great-writing-180962515/) - [DB](https://www.dropbox.com/s/6noghk9hexbgd7k/2021.08.24-one-writer-used-statistics-reveal-secrets-what-makes-great-writing-180962515.pdf?dl=0). You can do the same for code; pick a popular repository and reverse engineer how the author(s) solved the issue they faced. Instead of [becoming a better Googler](https://www.hanselman.com/blog/am-i-really-a-developer-or-just-a-good-googler), take the time to internalize what other **good code** does and how you can apply the bigger patterns to your own problem.

An even better next step as an author would be to analyze the process that a Stephen King or [Dostoevsky](https://en.wikipedia.org/wiki/Fyodor_Dostoevsky) or whoever writes those billionaire sex novels takes to develop his or her books. These are things that are not apparent from what is on the page. Thinking about the plot, outlining, writing down ideas and characters studies. Countless books like King's [On Writing](https://www.goodreads.com/book/show/10569.On_Writing) talk about the process of developing a novel.  But unless an author takes the time to write about their process, someone developing their skills is barred from learning the process.

Documentation is the closest thing a coder has to "looking behind the scenes" of how the actual code was developed.  I would argue it provides key insight into why certain decisions were made and how the developer went about creating the code architecture.  More to come on this topic in the future.

That is why the idea proposed and documented in a four-part series by [R-developer Simon Couch](https://blog--simonpcouch.netlify.app/about/) is so intriguing: there is value in seeing the development process of code which in this case is the R package [**stacks**](https://stacks.tidymodels.org/).  Couch documents how he wanted the package to work (and how to keep track of all of the objects), but he also discusses what he wants the user experience to be as an end result.

### Part 1: Introduction

[Link](https://blog--simonpcouch.netlify.app/blog/dev-docs-p1/) - [DB](https://www.dropbox.com/s/511ajuvgbze730u/2021.08.24-dev-docs-p1.pdf?dl=0)

### Part 2: Splitting Things Up

[Link](https://blog.simonpcouch.com/blog/dev-docs-p2/) - [DB](https://www.dropbox.com/s/m1d2sv7wihna7dh/2021.08.24-dev-docs-p2.pdf?dl=0)

### Part 3: Naming Things

[Link](https://blog.simonpcouch.com/blog/dev-docs-p3/) - [DB](https://www.dropbox.com/s/b2g5e0tl1m1s07z/2021.08.24-dev-docs-p3.pdf?dl=0)

### Part 4: Big Things

[Link](https://blog.simonpcouch.com/blog/dev-docs-p4/) - [DB](https://www.dropbox.com/s/dch6fua8tto4ig3/2021.08.24-dev-docs-p4.pdf?dl=0)

More links to help with practicing coding:

* Project Euler. [Link](https://projecteuler.net) - [DB](https://www.dropbox.com/s/68yk4s6v4khuvt8/2021.08.24-projecteuler.net.pdf?dl=0)
* Programming Not For You? Try Thinking.  [Link](https://www.hanselman.com/blog/programmings-not-for-you-how-about-thinking-be-empowered) - [DB](https://www.dropbox.com/s/v24lenvx87i4kfs/2021.08.24-programmings-not-for-you-how-about-thinking-be-empowered.pdf?dl=0)
* Six Essential Language Agnostic Programming Books.  [Link](https://www.hanselman.com/blog/six-essential-language-agnostic-programming-books) - [DB](https://www.dropbox.com/s/vrzhz3jxjm0gqqu/2021.08.24-six-essential-language-agnostic-programming-books.pdf?dl=0)

## AWS Tools: Amazon EMR
<hr />

### Getting Started With AWS EMR

[Link](https://aws.amazon.com/emr/getting-started/) - [DB](https://www.dropbox.com/s/s1ku1qxovjn9ssu/2021.08.24-getting-started.pdf?dl=0)

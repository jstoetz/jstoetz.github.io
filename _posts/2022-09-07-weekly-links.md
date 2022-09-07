---
layout: post
title: Links for the Week of 2022-09-05
by: "Jake Stoetzner"
categories: [links]
---

# Links for the Week of 2022-09-05

Here are some links I covered in the past week.

![](https://i0.wp.com/fronkonstin.com/wp-content/uploads/2022/03/dkzwov.png?resize=768%2C595&ssl=1)

Enjoy [neoplastic art](https://en.wikipedia.org/wiki/Neoplasticism) from the [Fronkonstin blog](https://fronkonstin.com/2022/03/25/the-mondrianomies/) while you read.

## Table of Contents
<hr/>

* [Serialized Listening](#searialized-listening)
* [Art and Coding](#art-and-coding)
* [Static Site Generator "Lite": R Markdown and Wordpress](#static-site-generator-lite-r-markdown-and-wordpress)
* [Data Shows History Repeats Itself (Again)](#data-shows-history-repeats-itself-again)
* [Machine Learning in R](#machine-learning-in-R)

## Serialized Listening
<hr/>

### 1001 Albums You Must Hear Before You Die

[Link](https://1001albumsgenerator.com/)

Are you obsessed with listening to music in a serialized manner? Looking for new music? [1001 Albums You Must Hear Before You Die](https://1001albumsgenerator.com/) randomly generates an album to listen to each week. You can create an account and make notes and rate each selection as you go along. [Follow along](https://1001albumsgenerator.com/jstoetz) with my (lack) of progress.

![](https://upload.wikimedia.org/wikipedia/en/6/6e/DavisBowieAladdinSane.jpg)

My first “assigned” album is David Bowie’s 6th studio album from 1973 titled [Alladin Sane](https://1001albumsgenerator.com/albums/0rBsmtPnOGz2IYmAqCrbuy/aladdin-sane).  This was the followup album to his breakout album from 1972 [The Rise and Fall of Ziggy Stardust and the Spiders from Mars (1972)](https://g.co/kgs/B2qYSj). According to Wikipedia “the record features a tougher and raunchier glam rock sound than its predecessor. The lyrics reflect the pros of Bowie's newfound stardom and the cons of touring and paint pictures of urban decay, drugs, sex, violence and death.”

**TLDR: Listening to and ranking the best 1001 albums is easier with https://1001albumsgenerator.com/**

## Art and Coding
<hr/>

### Why Love Generative Art? — Artnome

[Link](https://www.artnome.com/news/2018/8/8/why-love-generative-art) - [DB](https://www.dropbox.com/s/xww5qed7cy34sg8/2022-Sep-05-%20Why%20Love%20Generative%20Art%3F%20%E2%80%94%20Artnome.pdf?dl=0)

Stumbled across this brilliant piece that discusses generative art, which “is art programmed using a computer that intentionally introduces randomness as part of its creation process.”

One of the founders of generative art is [Georg Nees](https://collections.vam.ac.uk/item/O221321/schotter-print-nees-georg/) who created the work shown below called *Schotter (Gravel Stones)*.

!["Georg Nees generative art called *Schotter (Gravel Stones)*"](https://www.researchgate.net/publication/4732061/figure/fig8/AS:668901159477255@1536489872531/Schotter-Gravel-stones-Computer-generated-image-by-Georg-Nees-1968-1971-The-work-is.png)

[From a tutorial on recreating the iconic graphic:](http://www.artsnova.com/Nees_Schotter_Tutorial.html ) “In the original Schotter, Nees creates a grid of squares 12 across by 22 down. As the drawing of the squares progresses from top to bottom, the amount of disorder increases. This disorder takes two forms. First is in the positioning of each square, second is in its rotation.“In the original Schotter, Nees creates a grid of squares 12 across by 22 down. As the drawing of the squares progresses from top to bottom, the amount of disorder increases. This disorder takes two forms. First is in the positioning of each square, second is in its rotation.”

Users have recreated [Nees’ work in Python](https://github.com/LindomarRodrigues/Georg-Nees-Schotter-Python) and other programs.  R users are [deep into making generative art,](https://towardsdatascience.com/getting-started-with-generative-art-in-r-3bc50067d34b) and there are several packages designed to help you create your next masterpiece, including:

* [generative art](https://github.com/cutterkom/generativeart)
* [jasmines](https://github.com/djnavarro/jasmines)
* [aRtsy](https://github.com/koenderks/aRtsy). You can also check out the [aRtsy twitter feed](https://twitter.com/aRtsy_package) that tweets out an automatically generated work of art on the daily.

Other articles on generative art:

* The Mondrianomies - Fronkonstin - [Link](https://fronkonstin.com/2022/03/25/the-mondrianomies/) - [DB](https://www.dropbox.com/s/qfvqqinb94k45ez/2022-Sep-07-%20The%20Mondrianomies%20%7C%20Fronkonstin.pdf?dl=0)
* Georg Nees, Processing, and a Schotter Tutorial - [Link](http://www.artsnova.com/Nees_Schotter_Tutorial.html) - [DB](https://www.dropbox.com/s/gn405insk0i5ib6/2022-Sep-06-%20Georg%20Nees%2C%20Processing%2C%20and%20a%20Schotter%20Tutorial.pdf?dl=0)
* Recursion and Algorithmic Art by Jim Plaxco - [Link](http://www.artsnova.com/Processing_Recursion_Tutorial.html) - [DB](https://www.dropbox.com/s/o7zmb4ue80j346b/2022-Sep-06-%20Recursion%20and%20Algorithmic%20Art%20by%20Jim%20Plaxco.pdf?dl=0)

**TLDR: Make computer assisted art in R with packages like [generative art](https://github.com/cutterkom/generativeart), [jasmines](https://github.com/djnavarro/jasmines) and [aRtsy](https://github.com/koenderks/aRtsy)**

## Static Site Generator "Lite": R Markdown and Wordpress
<hr/>

### Publish R Markdown to WordPress site? Yes You Can! – Catbird Analytics

[Link](https://catbirdanalytics.wordpress.com/2021/08/02/publish-r-markdown-to-wordpress-site-yes-you-can/) - [DB](https://www.dropbox.com/s/j35lblirhud2aze/2022-Aug-23-%20Publish%20R%20Markdown%20to%20WordPress%20site%3F%20Yes%20You%20Can%21%20%E2%80%93%20Catbird%20Analytics.pdf?dl=0)

### How to publish a blog post on Wordpress using Rmarkdown - tobias dienlin

[Link](https://tobiasdienlin.com/2019/03/08/how-to-publish-a-blog-post-on-wordpress-using-rmarkdown/) - [DB](https://www.dropbox.com/s/cnd2b0gnwu7b72z/2022-Aug-23-%20How%20to%20publish%20a%20blog%20post%20on%20Wordpress%20using%20Rmarkdown%20-%20tobias%20dienlin.pdf?dl=0)

### Enabling HTTPS on your WordPress instance in Amazon Lightsail - Lightsail Documentation

[Link](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-enabling-https-on-wordpress) - [DB](https://www.dropbox.com/s/qamovnbdk02tbk7/2022-Aug-23-%20Enabling%20HTTPS%20on%20your%20WordPress%20instance%20in%20Amazon%20Lightsail%20%7C%20Lightsail%20Documentation.pdf?dl=0)

I have been toying with using Wordpress as a CMS for publishing content rather than something like Jekyll (which this blog runs on). The real advantage that Wordpress offers is that *non-technical* authors and other people who work on your site can easily use the Wordpress dashboard by logging-in via the `my-wordpress-site.com/admin` url and getting to work. They don’t necessarily have to know the finer workings of Github, HTML, YAML, Markdown or other static site generators.

The [Wordpress REST API](https://developer.wordpress.org/rest-api/) allows you to “push” content to your site remotely, including pages, posts, plugins, designs, meta information, settings - essentially anything you traditionally do in the admin dashboard. If you are working with a collaborator that is less “techie” than you, setting up and updating the blog could be completed via the API, and then the collaborator could use the traditional WYSIWYG editor and dashboard. Plus, it opens up the [wide range of integrations and plugins](https://wordpress.org/plugins/) that Wordpress provides.

**TLDR: Use the [Wordpress REST API](https://developer.wordpress.org/rest-api/) and Markdown as a static site generator.**

**TODO: Finish R code for working with the Wordpress API.**

## Data Shows History Repeats Itself (Again)
<hr/>

### Mathematicians Predict the Future With Data From the Past - WIRED

[Link](https://www.wired.com/2013/04/cliodynamics-peter-turchin/amp) - [DB](https://www.dropbox.com/s/a2ga636mzhkl39l/2022-Aug-23-%20Mathematicians%20Predict%20the%20Future%20With%20Data%20From%20the%20Past%20%7C%20WIRED.pdf?dl=0)

Ran across this 2013 article from Wired. It talked about “cliodynamics,” (a word I was unfamiliar with) “where scientists and mathematicians analyze history in the hopes of finding patterns they can then use to predict the future. It's named after Clio, the Greek muse of history.” [Source](https://www.wired.com/2013/04/cliodynamics-peter-turchin/amp).

What really gave me pause was when I read this quote: “we're due for a wave of widespread violence in about 2020, including riots and terrorism.” [Source](https://www.wired.com/2013/04/cliodynamics-peter-turchin/amp). Again, this article was published 7 years before 2020, and well, you would have to agree that the prediction is pretty spot on.

Cliodynamics doesn’t use sophisticated statistical techniques, advanced mathematics or big data. Most of the data is analyzed with off the shelf statistical software. What the author most attributes to the advancement of the study is the simple digitization of historical documents like newspapers and artifacts. This is likely less a factor nine years after the article was published, but it shows how far we have come.

It sounds like the researchers backtest a model they have created and compare it against past events: “[they] will build a mathematical model using one data set and then test that model against other historical data sets they're unfamiliar with. That way, they can see if the model holds.”

Besides the eerily accurate prediction for 2020, what interests me most is the idea that a trading model should test for repeatability and consistency rather than just profitability. Stated otherwise, raw data should be tested for how often it predicts similar patterns in the future not just how often a single pattern makes money. For example, a series of past higher highs results in an X% probability of a higher high in a future period.

Other articles on this subject:

* An introduction to repeatability estimation with rptR - [Link](https://cran.r-project.org/web/packages/rptR/vignettes/rptR.html) - [DB](https://www.dropbox.com/s/ahghbrcbw8agjka/2022-Sep-07-%20An%20introduction%20to%20repeatability%20estimation%20with%20rptR.pdf?dl=0)
* Peter Turchin Home - [Link](https://peterturchin.com/) - [DB](https://www.dropbox.com/s/9w85hm2sdiote5r/2022-Sep-07-%20Peter%20Turchin%20Home%20-%20Peter%20Turchin.pdf?dl=0). Turchin is a Professor at the University of Connecticut and was quoted in the Wired article. He has written several books on cliodynamics. His “research interests lie at the intersection of social and cultural evolution, historical macrosociology, economic history, mathematical modeling of long-term social processes, and the construction and analysis of historical databases.”

**TLDR: Social scientists calls 2020 chaos with simple models based on past historical patterns.**

**TODO: Check out the [rptR package](https://CRAN.R-project.org/package=rptR).**

## Machine Learning in R
<hr/>

### Scikit-Learn vs. Machine Learning in R (mlr) - DZone AI

[Link](https://dzone.com/articles/scikit-learn-vs-mlr-for-machine-learning) - [DB](https://www.dropbox.com/s/cu3jcbhlhjssh04/2022-Aug-23-%20Scikit-Learn%20vs.%20Machine%20Learning%20in%20R%20%28mlr%29%20-%20DZone%20AI.pdf?dl=0)

As I delve deeper into machine learning in R, I get the feeling that R doesn’t get the respect that it deserves when it is compared to Python and the [Keras and Tensorflow](https://www.tensorflow.org) juggernauts.

If Python and the machine learning glitterati are the single-purpose [$1400 knife only used for cutting fish](https://twitter.com/thesarahyork/status/1544509561805180933) then R at best is that $20 frying pan you have had since college that you use for every meal you cook. Bottom line is that can use R for machine learning just as well as any other software *plus a bunch of other stuff too* (eggs anyone?)

The article linked above compares [SciKit Learn](https://scikit-learn.org/stable/) and the [mlr package](https://mlr.mlr-org.com) across the following tasks:

* Creating Your Training and Test Data
* Choosing an Algorithm
* Hyperparameter Tuning
* Training
* Prediction
* Model Evaluation
* Decision Thresholding (i.e. Changing the Classification Threshold)

The reviews are slightly mixed, but the article concludes that R seems to compete well against other software.

**TLDR: R package holds it’s own against SciKit Learn.**

**TODO: Add the [mlr package](https://mlr.mlr-org.com) to the machine learning resource page.**

Have a boss week.

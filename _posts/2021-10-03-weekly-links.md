---
layout: post
title: Links for the Week of 2021-10-03
by: "Jake Stoetzner"
categories: [links]
---

# Links for the Week of 2021-10-03

Here are some links I covered in the past week.

## Table of Contents

- General
- R Interaction with Socket Server
- R Jobs and Simultaneous or Parallel Computations in R and Python
- Algorithmic Trading in R - Statistical Methods
- Risk Management with R

## General

### CBOE - Options Reference Data

[Link](https://www.cboe.com/us/options/market_statistics/reference_data/) - [DB](https://www.dropbox.com/s/boo8bwzeqxjsvco/2021.09.29%20-%20Reference%20Data.pdf?dl=0)

Source of option reference data in **csv** format for individual options and underlying stocks including:
* Underlying strike price intervals for short term expiration options
* Underlying list
* All constituent data

### Tax consequences of nonfungible tokens (NFTs)

[Link](https://www.journalofaccountancy.com/news/2021/jun/tax-consequences-of-nfts-nonfungible-tokens.html) - [DB](https://www.dropbox.com/s/08xu1gbfgbh4ne3/2021.09.27%20-%20Tax%20consequences%20of%20nonfungible%20tokens%20%28NFTs%29.pdf?dl=0)

### Tiger becomes latest star athlete to release NFTs

[Link](https://www.espn.com/golf/story/_/id/32249432/golfer-tiger-woods-releases-10000-digital-images-sold-company-co-founded-tom-brady) - [DB](https://www.dropbox.com/s/wki2o7wkchq7lhw/2021.09.29%20-%20Tiger%20becomes%20latest%20star%20athlete%20to%20release%20NFTs.pdf?dl=0)

Celebrities are known for their investment prowess. Can I get in on this?

### Plans for $400-billion new city in the American desert unveiled

[Link](https://www.cnn.com/style/article/telosa-marc-lore-blake-ingels-new-city/index.html) - [DB](https://www.dropbox.com/s/vymtu48dlrhcuiw/2021.09.29%20-%20Plans%20for%20%24400-billion%20new%20city%20in%20the%20American%20desert%20unveiled.pdf?dl=0)

### The best note taking apps for Mac – markdown, open format, cross platform

[Link](https://davidmytton.blog/the-best-note-taking-apps-for-mac-markdown-open-format-cross-platform/) - [DB](https://www.dropbox.com/s/mhrzhatpowhhkaf/2021.09.28-the-best-note-taking-apps-for-mac-markdown-open-format-cross-platform.pdf?dl=0)

### MkDocs

[Link](https://www.mkdocs.org/) - [DB](https://www.dropbox.com/s/87jt3o4r3sdku9p/2021.09.29%20-%20MkDocs.pdf?dl=0)

Markdown-based project documentation generator.

### An Expert's Take on the Best Philosophy Books

[Link](https://explorethearchive.com/best-philosophy-books) - [DB](https://www.dropbox.com/s/cvqgxh314y7xyzx/2021.09.29%20-%20An%20Expert%27s%20Take%20on%20the%20Best%20Philosophy%20Books.pdf?dl=0)

<small>tags: books</small>

### Water Fasting Results: Why I LOVED Not Eating for 5 Days - Nat Eliason

[Link](https://www.nateliason.com/blog/5-day-water-fast-health-benefits) - [DB](https://www.dropbox.com/s/egkl7lnx1z5rsah/2021.09.29%20-%20Water%20Fasting%20Results%3A%20Why%20I%20LOVED%20Not%20Eating%20for%205%20Days%20-%20Nat%20Eliason.pdf?dl=0)

<small>tags: fasting</small>

One man’s description of his journey to fast for 5 days, sprinkled with the science behind the health benefits of fasting: “Central to the benefits of fasting is a process called “autophagy.” Autophagy is the body’s natural process of killing off, eating up, or cleaning out bad cell matter that’s built up in your body. It’s an important system for staving off many diseases, including preventing cancer development.”

### X1 PRO Gen 2 5000W Ebike Conversion Kit - CYCMOTOR LTD

[Link](https://www.cycmotor.com/x1-pro-gen-2) - [DB](https://www.dropbox.com/s/jedeym3af0n2lm2/2021.09.29%20-%20X1%20PRO%20Gen%202%205000W%20Ebike%20Conversion%20Kit%20%7C%20CYCMOTOR%20LTD.pdf?dl=0)

Because everyone needs a 5000 watt e-bike conversion to dream about. Oh that’s just me?

### GitHub - aahnik/tgcf: The ultimate tool to automate custom telegram message forwarding. Live-syncer, Auto-poster, backup-bot, cloner, chat-forwarder, duplicator, ... Call it whatever you like! tgcf can fulfill your custom needs.

[Link](https://github.com/aahnik/tgcf) - [DB](https://www.dropbox.com/s/ibmtgbnqsc62or3/2021.09.29%20-%20GitHub%20-%20aahnik%3Atgcf%3A%20The%20ultimate%20tool%20to%20automate%20custom%20telegram%20message%20forwarding.%20Live-syncer%2C%20Auto-poster%2C%20backup-bot%2C%20cloner%2C%20chat-forwarder%2C%20duplicator%2C%20...%20Call%20it%20whatever%20you%20like%21%20tgcf%20can%20fulfill%20your%20custom%20needs..pdf?dl=0)

### Writer/Comedian Joe Mande’s Awesome DIY Cap Embroidery

[Link](https://uni-watch.com/2021/01/15/tv-writer-comedian-joe-mandes-awesome-diy-cap-embroidery/) - [DB](https://www.dropbox.com/s/thn7o0ihu30mhoo/2021.10.01%20-%20Writer%3AComedian%20Joe%20Mande%E2%80%99s%20Awesome%20DIY%20Cap%20Embroidery.pdf?dl=0)

The KC “Kurt Cobain” one is legit.

## R Interaction with Socket Server

### Communicating with a TCP socket (server) - Stack Overflow

[Link](https://stackoverflow.com/questions/25938011/communicating-with-a-tcp-socket-server) - [DB](https://www.dropbox.com/s/k3duh2iiyodog4u/2021.09.29%20-%20Communicating%20with%20a%20TCP%20socket%20%28server%29%20-%20Stack%20Overflow.pdf?dl=0)

<small>tags: package</small>

Useful when downloading data from DTN IQfeed. Their model is to download from the TCP/IP feed that runs locally on your machine.  

See also:

* the [svSocket package](https://cran.r-project.org/web/packages/svSocket/svSocket.pdf).
* How do multiple clients connect simultaneously to one port, say 80, on a server? - Stack Overflow. [Link](https://stackoverflow.com/questions/3329641/how-do-multiple-clients-connect-simultaneously-to-one-port-say-80-on-a-server) - [DB](https://www.dropbox.com/s/vznr4zcsvrow9vu/2021.09.29%20-%20How%20do%20multiple%20clients%20connect%20simultaneously%20to%20one%20port%2C%20say%2080%2C%20on%20a%20server%3F%20-%20Stack%20Overflow.pdf?dl=0)

## R Jobs and Simultaneous or Parallel Computations in R and Python

### Call R from R

[Link](https://callr.r-lib.org/) - [DB](https://www.dropbox.com/s/9t9jh1gzm7muyxy/2021.09.29%20-%20Call%20R%20from%20R.pdf?dl=0)

<small>tags: package</small>

The callR Package - useful for running a separate R process when you don’t want to leave your current session.

### RStudio 1.2 Preview: Jobs - RStudio Blog

[Link](https://blog.rstudio.com/2019/03/14/rstudio-1-2-jobs/) - [DB](https://www.dropbox.com/s/a56ihk2xlb6vv8q/2021.09.29%20-%20RStudio%201.2%20Preview%3A%20Jobs%20-%20RStudio%20Blog.pdf?dl=0)

Running local R jobs in R studio.

### Run multiple R-scripts simultaneously - Stack Overflow

[Link](https://stackoverflow.com/questions/31137842/run-multiple-r-scripts-simultaneously/41920899) - [DB](https://www.dropbox.com/s/kndezplgukpmyle/2021.09.29%20-%20Run%20multiple%20R-scripts%20simultaneously%20-%20Stack%20Overflow.pdf?dl=0)

### Run multiple python scripts concurrently - Stack Overflow

[Link](https://stackoverflow.com/questions/28549641/run-multiple-python-scripts-concurrently/28549735) - [DB](https://www.dropbox.com/s/wqqohsnwpbwt4bb/2021.09.29%20-%20Run%20multiple%20python%20scripts%20concurrently%20-%20Stack%20Overflow.pdf?dl=0)

### Running Python Processes In Parallel - Data Science Beginners

[Link](https://datasciencebeginners.com/2018/10/24/running-python-processes-in-parallel-getting-started/) - [DB](https://www.dropbox.com/s/3csbtois3hox1p6/2021.09.29%20-%20Running%20Python%20Processes%20In%20Parallel%20%7C%20Data%20Science%20Beginners.pdf?dl=0)

## Algorithmic Trading in R - Statistical Methods

### The power of R for trading (part 1) - Systemic Risk and Systematic Value

[Link](https://www.sr-sv.com/the-power-of-r-for-trading-part-1/) - [DB](https://www.dropbox.com/s/b66uobsigp55d4w/2021.09.27%20-%20The%20power%20of%20R%20for%20trading%20%28part%201%29%20%7C%20Systemic%20Risk%20and%20Systematic%20Value.pdf?dl=0)

### The power of R for trading (part 2) - Systemic Risk and Systematic Value

[Link](https://www.sr-sv.com/the-power-of-r-for-trading-part-2/) - [DB](https://www.dropbox.com/s/kskuztj5sa9mk45/2021.09.27%20-%20The%20power%20of%20R%20for%20trading%20%28part%202%29%20%7C%20Systemic%20Risk%20and%20Systematic%20Value.pdf?dl=0)

Good information on broad topics as they relate to trading analysis in R.

On logistic regression using the **lm()** function: “Most trading strategies, whether quantitative or not, rely on the relation between a predictor variable and a predicted variable. Trades are often based on the belief that if x happens then y will follow…. Under many circumstances linear regression is simply the easiest way [1] to verify if this belief is significantly supported by the data and [2] to estimate what the magnitude of the response will be.”

On Logit (Logistic) Regression using the **glm()** function: “Logistic Regression is a classification algorithm used to predict a binary outcome (0 or 1) based on probability that is related to a set of independent variables. It operates like a special case of linear regression with a log of odds (i.e. relative probabilities) as dependent variable. Logistic regression is part of a larger class of algorithms known as Generalized Linear Model (GLM).

Logit regression is the standard method to assess whether a specific market event is likely to happen, based on available data.”

### What markets can learn from statistical learning - Systemic Risk and Systematic Value

[Link](http://www.sr-sv.com/what-markets-can-learn-from-statistical-learning/) - [DB](https://www.dropbox.com/s/f1j1jv3cf0d5w47/2021.09.27%20-%20What%20markets%20can%20learn%20from%20statistical%20learning%20%7C%20Systemic%20Risk%20and%20Systematic%20Value.pdf?dl=0)

A broad overview of statistical learning methods, taken from [“An Introduction to Statistical Learning with Applications in R”](http://www-bcf.usc.edu/~gareth/ISL/). From the opening paragraph: “Understanding statistical learning is critical in modern markets, even for non-quants. Statistical learning works with complex datasets to forecast returns or to estimate the impact of specific events. The choice of methods is key: they range from simple regression to complex machine learning. Simplicity can deliver superior returns if it avoids “overfitting” (gearing models excessively to specific past experiences). Success must be measured in “out-of-sample” predictive power, after a model has been selected and estimated.”

## Risk Management with R

### Financial Risk Management with R

[Link](https://www.coursera.org/learn/financial-risk-management-with-r) - [DB](https://www.dropbox.com/s/bwkgk0a8jfc71x9/2021.09.27%20-%20Financial%20Risk%20Management%20with%20R.pdf?dl=0)

Free course offered by Duke University

### The basics of Value at Risk and Expected Shortfall

[Link](https://www.portfolioprobe.com/2012/10/23/the-basics-of-value-at-risk-and-expected-shortfall/) - [DB](https://www.dropbox.com/s/dnyhcs40hcrsux9/2021.09.27%20-%20The%20basics%20of%20Value%20at%20Risk%20and%20Expected%20Shortfall%20%7C%20Portfolio%20Probe%20%7C%20Generate%20random%20portfolios.%20Fund%20management%20software%20by%20Burns%20Statistics.pdf?dl=0)

### Value at Risk (VaR) for Algorithmic Trading Risk Management - Part I - QuantStart

[Link](https://www.quantstart.com/articles/Value-at-Risk-VaR-for-Algorithmic-Trading-Risk-Management-Part-I/) - [DB](https://www.dropbox.com/s/wx8ffc882lg694y/2021.09.27%20-%20Value%20at%20Risk%20%28VaR%29%20for%20Algorithmic%20Trading%20Risk%20Management%20-%20Part%20I%20%7C%20QuantStart.pdf?dl=0)

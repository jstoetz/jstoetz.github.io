---
layout: post
title: Links for the Week of 2021-10-10
by: "Jake Stoetzner"
categories: [links]
---

# Links for the Week of 2021-10-10

Here are some links I covered in the past week.

## Table of Contents
* Articles
* Books
* General
* R for Facebook
* Delta Neutral Trading for Short Straddles

## Articles

* **From [R Views](https://rviews.rstudio.com )**: August 2021: "Top 40" New CRAN Packages. [Link](https://rviews.rstudio.com/2021/09/27/august-2021-top-40-new-cran-packages/) - [DB](https://www.dropbox.com/s/3y6pxp6oxmqd14k/2021.10.10%20-%20August%202021%3A%20%22Top%2040%22%20New%20CRAN%20Packages.pdf?dl=0). Notable packages:
    * [nflreadr](https://cran.r-project.org/web/packages/nflreadr/index.html) provides functions for downloading data from the GitHub repository for the [nflverse project](https://github.com/nflverse).
    * [CRAN - Package meltr](https://cran.r-project.org/package=meltr) provides functions to read non-rectangular data, such as ragged forms of csv (comma-separated values), tsv (tab-separated values), and fwf (fixed-width format) files.
    * [CRAN - Package tidycharts](https://cran.r-project.org/package=tidycharts) Provides functions to generate charts compliant with the International Business Communication Standards (<a href="https://www.ibcs.com/">IBCS</a>) including unified bar widths, colors, chart sizes, etc. There is a <a href="https://cran.r-project.org/web/packages/tidycharts/vignettes/Getting_Started.html">Getting Started</a> guide and vignettes on <a href="https://cran.r-project.org/web/packages/tidycharts/vignettes/EDA-for-palmer-penguins-data-set.html">EDA</a>, <a href="https://cran.r-project.org/web/packages/tidycharts/vignettes/customize-package.html">Customization</a>, and <a href="https://cran.r-project.org/web/packages/tidycharts/vignettes/join_charts.html">Joining Charts</a>.

## Books

### Statistical Inference via Data Science

<img src="https://d33wubrfki0l68.cloudfront.net/19dafd10a53785f1407566a1f3a09b29a6bab847/1e5f0/images/logos/book_cover.png" width="350">

[Bookdown](https://moderndive.com/) - [DB](https://www.dropbox.com/s/wi9edaapq16asgr/Statistical%20Inference%20via%20Data%20Science%20A%20ModernDive%20into%20R%20and%20the%20Tidyverse%20by%20Chester%20Ismay%20Albert%20Y.%20Kim%20%28z-lib.org%29.pdf?dl=0)

A ModernDive into R and the Tidyverse by Chester Ismay and Albert Y. Kim.

## General

### FEATURED: Introducing {cssparser} - parse CSS into R lists and apply CSS to html within R - coolbutuseless

[Link](https://coolbutuseless.github.io/2021/09/29/introducing-cssparser-parse-css-into-r-lists-and-apply-css-to-html-within-r/) - [DB](https://www.dropbox.com/s/05oa43qo50m6ds0/2021.10.10%20-%20Introducing%20%7Bcssparser%7D%20-%20parse%20CSS%20into%20R%20lists%20and%20apply%20CSS%20to%20html%20within%20R%20-%20coolbutuseless-2.pdf?dl=0)

An amazing package that parses **css** from existing **html** into R lists and vice versa applies **css** onto **html** using vanilla R. Look for an upcoming post using this package.

### prettydoc: Creating Pretty Documents From R Markdown

[Link](https://prettydoc.statr.me/) - [DB](https://www.dropbox.com/s/3k3oznsagesj5wy/2021.10.11%20-%20prettydoc%3A%20Creating%20Pretty%20Documents%20From%20R%20Markdown.pdf?dl=0)

<small>tags: package</small>

With a few lines of code - put ```prettydoc::html_pretty ``` in your R Markdown header, you can use a **prettydoc** built-in themes and syntax highlighters.

### Ten awesome R Markdown tricks

[Link](https://towardsdatascience.com/ten-awesome-r-markdown-tricks-56ef6d41098) - [DB](https://www.dropbox.com/s/2oh2woyx7q8sf51/2021.10.11%20-%20Ten%20awesome%20R%20Markdown%20tricks.pdf?dl=0)

<small>tags: markdown</small>

### Up & running with blogdown in 2021 - Alison Hill

[Link](https://www.apreshill.com/blog/2020-12-new-year-new-blogdown/) - [DB](https://www.dropbox.com/s/ptkwgj4ein0pjv5/2021.10.10%20-%20Up%20%26%20running%20with%20blogdown%20in%202021%20%7C%20Alison%20Hill.pdf?dl=0)

### Feeding Cattle Seaweed Reduces Their Greenhouse Gas Emissions 82 Percent

[Link](https://www.ucdavis.edu/news/feeding-cattle-seaweed-reduces-their-greenhouse-gas-emissions-82-percent) - [DB](https://www.dropbox.com/s/drrlutzuq8glz53/2021.10.10%20-%20Feeding%20Cattle%20Seaweed%20Reduces%20Their%20Greenhouse%20Gas%20Emissions%2082%20Percent.pdf?dl=0)

Surf and turf anyone? This steak tastes fishy! (I’ll be here all night). “Agriculture is responsible for 10 percent of greenhouse gas emissions in the U.S., and half of those come from cows and other ruminant animals that belch methane and other gases throughout the day as they digest forages like grass and hay.” The article also says that only a small fraction of the earth is fit for crop production, with “far more” available for grazing. The real question is whether it works for humans…

### Pricing of European Options with Monte Carlo – Predictive Hacks

[Link](https://predictivehacks.com/pricing-of-european-options-with-monte-carlo/) - [DB](https://www.dropbox.com/s/x2fi0c534iwpn1r/2021.10.10%20-%20Pricing%20of%20European%20Options%20with%20Monte%20Carlo%20%E2%80%93%20Predictive%20Hacks.pdf?dl=0)

Basic rundown of using R to predict option prices.

### How Tough is that Football Match?

[Link](https://dm13450.github.io/2021/09/26/Fixture-Difficulty.html) - [DB](https://www.dropbox.com/s/yhrygh7h8jb5ckf/2021.10.10%20-%20How%20Tough%20is%20that%20Football%20Match%3F.pdf?dl=0)

<small> tags: sports</small>

The article breaks down how to compare the relative strength among teams. “KL divergence value shows how unbalanced a match was, but to convert it into a difficulty metric we need to understand who the better team is in the match, our more typically put, the favourite. Again, we can simply use the odds and multiply the KL divergence by 1 or -1 depending on whether the team is the favourite or the underdog.” By combining the difficulty metric and odds, the author is able to show how you can: (1) forecast the outcome of a match using a multinomial neural net with some degree of success; and (2) predict the number of goals with a higher degree of accuracy.

### Jekyll E-commerce Solutions – Jekyll Themes

[Link](https://jekyllthemes.io/resources/jekyll-e-commerce-solutions) - [DB](https://www.dropbox.com/s/dl0uugq7qv7as45/2021.10.11%20-%20Jekyll%20E-commerce%20Solutions%20%E2%80%93%20Jekyll%20Themes.pdf?dl=0)

List of available solutions for e-commerce that integrate with a Jekyll site. For Shopify users, see [Creating a Buy Button](https://help.shopify.com/en/manual/online-sales-channels/buy-button/create-buy-button).

### RoutineHub

[Link](https://routinehub.co/) - [DB](https://www.dropbox.com/s/fs2i35g68ia1cvo/2021.10.11%20-%20RoutineHub.pdf?dl=0)

A collection of user submitted shortcuts for Apple iOS users.

### The Ultimate List of Data Science Podcasts – Real Python

[Link](https://realpython.com/data-science-podcasts/) - [DB](https://www.dropbox.com/s/52nkkuz1yppv6rs/2021.10.11%20-%20The%20Ultimate%20List%20of%20Data%20Science%20Podcasts%20%E2%80%93%20Real%20Python.pdf?dl=0)

<small>tags: podcast</small>

## R for Facebook

### Facebook Data Mining using R

[Link](https://www.listendata.com/2017/03/facebook-data-mining-using-r.html?m=1) - [DB](https://www.dropbox.com/s/g809v0y2hl33ytg/2021.10.08%20-%20Facebook%20Data%20Mining%20using%20R.pdf?dl=0)

In-depth post on using R to mine data on Facebook.

### Analyze Facebook with R - ThinkToStart

[Link](http://thinktostart.com/analyzing-facebook-with-r/) - [DB](https://www.dropbox.com/s/6nr6urjv724dlxh/2021.10.08%20-%20Analyze%20Facebook%20with%20R%20-%20ThinkToStart.pdf?dl=0)

### Analyzing Facebook with R - senthilpazhani - R-Programming Wiki

[Link](https://github.com/senthilpazhani/R-Programming/wiki/1.1.-Analyzing-Facebook-with-R) - [DB](https://www.dropbox.com/s/nchk3ksp2wmsj7z/2021.10.08%20-%201.1.%20Analyzing%20Facebook%20with%20R%20%C2%B7%20senthilpazhani%3AR-Programming%20Wiki.pdf?dl=0)

Great list of (slightly outdated) links to use R to analyze Facebook posts.

## Delta Neutral Trading for Short Straddles

### How to Use the Symbol ATM Straddle Performance History

[Link](https://marketchameleon.com/Learn/SymbolATMStraddlePerformance)

[Market Chameleon](https://marketchameleon.com) has an interesting scan involving long at-the-money option straddles. It looks at the change in value of the straddles over different holding periods.  “At a base level, the report tries to answer the question: If I bought a straddle on a certain day of the week and held it until the close of a different day, how often would that straddle gain value?”

Three basic strategies are examined (historically):

1. **Unhedged ATM Straddle** is a naked option straddle, 1 call and 1 put on the same at-the-money strike, with no additional stock traded. The resultant value is based solely on the value of the options themselves. Any net delta made from the trade is left untouched.
2. **Delta-Neutral ATM Straddles** is based on hedging the straddle's initial net delta to neutral, and then leaving the trade untouched for the remainder of the week.
3. **Daily Delta-Rehedging** is based on hedging the net delta both initially at the "opening" of the trade, and then daily at the end of each day to neutralize the delta.  For techniques that include delta-hedging, the straddle return and the win rate would both be affected by the gain or loss in value of the underlying stock that results from hedging.

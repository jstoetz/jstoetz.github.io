---
layout: post
title: Links for the Week of 2021-08-15
by: "Jake Stoetzner"
categories: [links]
---

Here are some links I covered in the past week.

## Table of Contents

- [Table of Contents](#table-of-contents)
- [General](#general)
- [Important Dental Metrics/KPI](#important-dental-metricskpi)
- [HTML](#html)
- [Membership Websites](#membership-websites)
- [Automating R Scripts and Github Actions](#automating-r-scripts-and-github-actions)
- [A Few Javascript Bookmarklets to Add to Apple Safari on iOS](#a-few-javascript-bookmarklets-to-add-to-apple-safari-on-ios)
- [RDrop2 package](#rdrop2-package)
- [Eaglesoft](#eaglesoft)
- [Google Sheets to Google Calendar](#google-sheets-to-google-calendar)
- [DeepSpeech](#deepspeech)

## General

### ADVFN

[Link](https://www.advfn.com/) - [DB](https://www.dropbox.com/s/bl0o2hix5rq99yj/2021.08.10%20-%20.pdf?dl=0)

Free site for stock, crypto and other data.

### Stock Analysis and Forecasting

[Link](http://rstudio-pubs-static.s3.amazonaws.com/493413_f3d12f11474a4484b2791dd0fd0a9bf5.html#next_steps) - [DB](https://www.dropbox.com/s/5ugylttpl7xxjc4/2021.08.10%20-%20Stock%20Analysis%20and%20Forecasting.pdf?dl=0)

<small>tags: package</small>

How to use various news APIs, sentiment analysis and time series forecasting to analyze stock data.Makes use of the **prophet** package that connects the prophet Facebook API for forecasting. [Link](https://facebook.github.io/prophet/docs/quick_start.html#r-api) - [DB](https://www.dropbox.com/s/rb7x09thc1hjbsn/2021.08.10%20-%20Quick%20Start.pdf?dl=0).

### How to get at the database schema of a hidden DB? - Stack Overflow

[Link](https://stackoverflow.com/questions/4420953/how-to-get-at-the-database-schema-of-a-hidden-db) - [DB](https://www.dropbox.com/s/qst2xup23rd362x/2021.08.10%20-%20How%20to%20get%20at%20the%20database%20schema%20of%20a%20hidden%20DB%3F%20-%20Stack%20Overflow.pdf?dl=0)

This is a great question about how to access an Eaglesoft database.

### How I Attracted 1,000+ New Patients in Less Than 3 Years Using Instagram with Dr. Brian Baliwas - Thriving Dentist with Gary Takacs

[Link](https://www.thrivingdentist.com/how-i-attracted-1000-new-patients-in-less-than-3-years-using-instagram-with-dr-brian-baliwas/) - [DB](https://www.dropbox.com/s/bxo5f8pv6nmr8y2/2021.08.10%20-%20How%20I%20Attracted%201%2C000%2B%20New%20Patients%20in%20Less%20Than%203%20Years%20Using%20Instagram%20with%20Dr.%20Brian%20Baliwas%20-%20Thriving%20Dentist%20with%20Gary%20Takacs.pdf?dl=0)

### R function to read in all sheets from Excel Files

[Link](https://stackoverflow.com/questions/12945687/read-all-worksheets-in-an-excel-workbook-into-an-r-list-with-data-frames)

Custom function that uses the **readxl** package to loop through all of the sheets in and **xls** or **xlsx** file and returns them as a ```tibble``` or a ```data.frame```.

```
library(readxl)    
read_excel_allsheets <- function(filename, tibble = FALSE) {
    # I prefer straight data.frames
    # but if you like tidyverse tibbles (the default with read_excel)
    # then just pass tibble = TRUE
    sheets <- readxl::excel_sheets(filename)
    x <- lapply(sheets, function(X) readxl::read_excel(filename, sheet = X))
    if(!tibble) x <- lapply(x, as.data.frame)
    names(x) <- sheets
    x
}
```

### Gusto API

[Link](https://docs.gusto.com/)

Documentation on how to link to the Gusto payroll API.

### Getting Data from the Web with R - Part 6: HTTP Basics and the RCurl Package

[Link](http://www.cse.chalmers.se/~chrdimi/downloads/web/getting_web_data_r6_http_rcurl.pdf)

## Important Dental Metrics/KPI

### Clinical Metrics

* **Active Patients** - patients seen within last 24 months
* **Active Hygiene** - active patients with recall in the next 6 months.

### Schedule Metrics

* **Average hourly production** - production divided by hours worked
* **Production per procedure** - production divided by total number of procedures
* **Unfilled hours** - total hours unscheduled divided by total available hours
* **Scheduled hours** - total hours unscheduled divided by total available hours
* **Number of broken appointments not reappointed**
* **Production lost from broken appointments**

### Financial Metrics

* **Production** -
  + **By Doctor**
  + **By Hygiene**

### Employee Metrics

* **Production Ratios**
  * **Per Employee** - total production divided by # of employees per period
  * **Per Hours Worked** - total production divided by # of hours worked per period
  * **Per Hygienist** - total production divided by hygienist divied by no of hygienists per period
* **Hours**
  * **Total Hours**
    * **Total Hours by Department**
  * **Hours Per Employee**

* **Compensation**
  * **Gross Earnings** - total compensation, including base wages,
    * **Total** - sum of gross earnings for all employees
      * **Per Employee**
      * **Per Department**
    * **Average Gross Earnings Per Employee**
    * **Average Gross Earnings Per Department**

### Master the Metrics that Matter

[Link](https://www.dentrix.com/resources/pdf/Dentrix_Master-the-metrics-that-matter_eBook.pdf)

## HTML

### Tool that displays outline of HTML5 documents - Software Recommendations Stack Exchange

[Link](https://softwarerecs.stackexchange.com/questions/190/tool-that-displays-outline-of-html5-documents) - [DB](https://www.dropbox.com/s/oj1spofed2i32p1/2021.08.10%20-%20Tool%20that%20displays%20outline%20of%20HTML5%20documents%20-%20Software%20Recommendations%20Stack%20Exchange.pdf?dl=0)

Covers how to get a broad outline of a HTML5 page. Recommends the following tools:

* [GitHub - hoyois/html5outliner: A Javascript implementation of the HTML outline algorithm](https://github.com/hoyois/html5outliner)
* [Ready to check - Nu Html Checker](https://validator.w3.org/nu/) with instructions on how to implement the tool locally. From the article [The Nu Html Checker (v.Nu)](http://validator.github.io/validator/#standalone):

>You can have it downloaded and run within minutes (if not seconds) with just two commands:

```
wget https://sideshowbarker.net/releases/jar/vnu.jar
java -cp ./vnu.jar nu.validator.servlet.Main 8888
```
>Then open **http://localhost:8888/** in your browser and you’ll have a form you can use for either checking documents by specifying their URLs or by file upload. To get an outline for a document, just check the outline checkbox in that form.

* A Chrome extension called [HTML5 Outliner](https://chrome.google.com/webstore/detail/html5-outliner/afoibpobokebhgfnknfndkgemglggomo)

## Membership Websites

### MemberSpace - Turn any part of your website into members-only with just a few clicks

[Link](https://www.memberspace.com/) - [DB](https://www.dropbox.com/s/yf0jayhfwm7ph2i/2021.08.10%20-%20MemberSpace%20-%20Turn%20any%20part%20of%20your%20website%20into%20members-only%20with%20just%20a%20few%20clicks.pdf?dl=0)

### 13 Best Membership Website Builders and Platforms in 2021

[Link](https://blog.hubspot.com/website/membership-website-builder) - [DB](https://www.dropbox.com/s/xoyvy8i99owwgg7/2021.08.10%20-%20Running%20R%20Scripts%20on%20a%20Schedule%20with%20GitHub%20Actions%20-%20Simon%20Couch.pdf?dl=0)

Slightly skewed reviews of membership hosing sites.

## Automating R Scripts and Github Actions

### Running R Scripts on a Schedule with GitHub Actions - Simon Couch

[Link](https://blog--simonpcouch.netlify.app/blog/r-github-actions-commit/) - [DB](https://www.dropbox.com/s/oxhuuzon0aqqz3f/2021.08.10%20-%20Running%20R%20Scripts%20on%20a%20Schedule%20with%20GitHub%20Actions%20%7C%20Simon%20Couch.pdf?dl=0)

Killer article on how to automate running an R script using Github Actions.

## A Few Javascript Bookmarklets to Add to Apple Safari on iOS

I do a lot of research and writing on my iPhone. That means that I frequently need tools that aren't readily available on iOS. Sometimes apps can stand in to do the work.  Sometimes you need a little extra help.

Here is a good overview of [how iOS bookmarklets work and how to install them](https://www.cultofmac.com/500532/how-to-add-bookmarklet-mobile-iphone-safari/) or a [list of useful ones to get you started](http://caiorss.github.io/bookmarklets.html).

### PDF Pretty bookmarklet

Sometimes webpages don't save well as PDFs (looking at you, Medium) and this gives a quick way to print to PDF in a "pretty" manner.

```
javascript:(function(){if(window['priFri']){window.print()}else{pfstyle='nbk';pfBkVersion='1';_pnicer_script=document.createElement('SCRIPT');_pnicer_script.type='text/javascript';_pnicer_script.src='//cdn.printfriendly.com/printfriendly.js';document.getElementsByTagName('head')[0].appendChild(_pnicer_script);}})();
```

### View Page Source bookmarklet

I can't recall where I found this one but it works fairly seamlessly - view the **html** code of any site you are browsing.

```
javascript:(function()%7Bvar%20a=window.open('about:blank').document;a.write('%3C!DOCTYPE%20html%3E%3Chtml%3E%3Chead%3E%3Ctitle%3ESource%20of%20'+location.href+'%3C/title%3E%3Cmeta%20name=%22viewport%22%20content=%22width=device-width%22%20/%3E%3C/head%3E%3Cbody%3E%3C/body%3E%3C/html%3E');a.close();var%20b=a.body.appendChild(a.createElement('pre'));b.style.overflow='auto';b.style.whiteSpace='pre-wrap';b.appendChild(a.createTextNode(document.documentElement.innerHTML))%7D)();
```

### List all Links

```
javascript:(function(){var a=;for(var ln=0;ln<document.links.length;ln++){var lk=document.links[ln];a+=ln+': <a href=\+lk+'\' title=\+lk.text+'\'>'+lk+'</a><br>\n';}w=window.open(,'Links','scrollbars,resizable,width=400,height=600');w.document.write(a);}()
```

## RDrop2 package

More to come on this one but this is a great R interface to Dropbox.

### rdrop2 - Dropbox interface from R

[Link](https://github.com/karthik/rdrop2)

## Eaglesoft

### [Support Home Page](https://pattersonsupport.custhelp.com/)

### [Eaglesoft - Provider Productivity Report](https://pattersonsupport.custhelp.com/app/answers/detail/a_id/20428/~/eaglesoft---provider-productivity-report)

### [Reports in Eaglesoft - Dental Intel Education](https://intercom.help/dental-intel/en/articles/909502-reports-in-eaglesoft)

## Google Sheets to Google Calendar

### How to automatically add a schedule from Google sheets to Calendar using Apps Script - by Varsha Das - Medium

[Link](https://medium.com/@varsha.das22/how-to-automatically-add-a-schedule-from-google-sheets-to-calendar-using-apps-script-2d37990a1ddd)

### How to create an HTML Add to Calendar link for emails - Litmus

[Link](https://www.litmus.com/blog/how-to-create-an-add-to-calendar-link-for-your-emails/)

### “Add to Calendar” Links for Google Calendar, Outlook, ICS

[Link](https://www.labnol.org/apps/calendar.html)

### Class Calendar  -  Apps Script  -  Google Developers

[Link](https://developers.google.com/apps-script/reference/calendar/calendar#createEvent(String,Date,Date))

## DeepSpeech

### [Using a Pre-trained Model — DeepSpeech 0.9.3 documentation](https://deepspeech.readthedocs.io/en/v0.9.3/USING.html#using-the-command-line-client)

### [performance - Efficiently split a large audio file in R - Stack Overflow](https://stackoverflow.com/questions/20713513/efficiently-split-a-large-audio-file-in-r)

### [DeepSpeech-examples/autosub at r0.9 · mozilla/DeepSpeech-examples](https://github.com/mozilla/DeepSpeech-examples/tree/r0.9/autosub)

### [tyiannak/pyAudioAnalysis: Python Audio Analysis Library: Feature Extraction, Classification, Segmentation and Applications](https://github.com/tyiannak/pyAudioAnalysis)

### [tyiannak/deep_audio_features: Pytorch implementation of deep audio embedding calculation](https://github.com/tyiannak/deep_audio_features)

### [r - Converting MP3 file to WAV - Stack Overflow](https://stackoverflow.com/questions/40518311/converting-mp3-file-to-wav)

### [tuneR: Analysis of Music and Speech](https://cran.r-project.org/web/packages/tuneR/tuneR.pdf)

### [Working with audio in R using av - R-bloggers](https://www.r-bloggers.com/2020/02/working-with-audio-in-r-using-av/)

### [audio - Fast way for splitting large .wav file using R - Stack Overflow](https://stackoverflow.com/questions/39047687/fast-way-for-splitting-large-wav-file-using-r)

### [mozilla/DeepSpeech-examples: Examples of how to use or integrate DeepSpeech](https://github.com/mozilla/DeepSpeech-examples)

### [Command Line 101 - Learn Version Control with Git](https://www.git-tower.com/learn/git/ebook/en/command-line/appendix/command-line-101/)

### [Fast setup for using DeepSpeech to transcribe audio files for free -- compare to Google Speech-to-Text](https://satra.cogitatum.org/group/2019/07/07/transcription.html)

### [audio - Large Wave File not being read in Python - Stack Overflow](https://stackoverflow.com/questions/29000788/large-wave-file-not-being-read-in-python)

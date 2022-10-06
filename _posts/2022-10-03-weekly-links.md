---
layout: post
title: Links for the Week of 2022-10-03
by: "Jake Stoetzner"
categories: [links]
---

Here are some links I enjoyed from the past week.

## Table of Contents

* [Can Data Analysis Save Lives](#can-data-analysis-save-lives)
* [GarageBand Art](#garageband-art)
* [Doodle Everywhere](#doodle-everywhere)
* [How to Unroll a Twitter Thread](#how-to-unroll-a-twitter-thread)
* [Spooky Viewing](#spooky-viewing)

## Can Data Analysis Save Lives?

![](https://upload.wikimedia.org/wikipedia/commons/8/87/Crude_U.S._suicide_rate_1981_2016.png)

### [ARTICLE] Can Smartphones Help Predict Suicide? - The New York Times

[Link](https://www.nytimes.com/2022/09/30/health/suicide-predict-smartphone.html?unlocked_article_code=glU2-2j4AIrc9189sZqxSKLMO2UHgPAlCj0qtE-eq89RJGncz6k68HZRgVFcsbI7qAo_EOkuRdRZiiOz9mWS5GcICUTST4ZkS2sYE8ybtaVQ4ruD4VUgnb8Zf7dBuEck0Vofp9APD64Rz3awLmFoR0YZAqRkKDbxxg3sViaL_BvN4skCxHaqpQ2GbIutxECE1Nest3Q4FR8MgjAMHceTEJtkDNs7YwKHwj_ZPYfJ79buPIBmWpc2Gj_KrvBLgpeA-psHikW0DQ-a7OU-I1HFkWKlqFiuG1Sv1ZfJEZiHqVSArZcySmQN-aXrEXyVWD01gE8aHGFpMvGiD_DZoY_4amUBsA&smid=share-url) - [DB](https://www.dropbox.com/s/mdfhtr1asxu02x1/2022-Sep-30-%20Can%20Smartphones%20Help%20Predict%20Suicide%3F%20-%20The%20New%20York%20Times.pdf?dl=0)

*If you are having thoughts of suicide, text the National Suicide Prevention Lifeline at 988 or go to SpeakingOfSuicide.com/resources for a list of additional resources.*

The mental state, thoughts and motivation that lead to suicide are, to say the least, complex and unpredictable.

Imagine being a psychiatrist tasked with predicting which patients will commit suicide.

You know that during the first 3 months after you discharge a patient from a psychiatric facility, they are [100 times as likely to commit suicide](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5710249) compared to the global average. If that patient was admitted for suicidal thoughts and behaviors in the first place then they are [200 times](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5710249) more likely to commit suicide.    You also know that traditional methods of predicting which patients will commit suicide [was only slightly better than chance for all outcomes and “predictive ability has not improved across 50 years of research”](https://psycnet.apa.org/record/2016-54856-001).  Not surprisingly, you know the suicide rate has remained relatively unchanged for the past 100 years and has [actually increased 30% from 2000 to 2020](https://en.m.wikipedia.org/wiki/Suicide_in_the_United_States).

Responsible for this challenging task, what tools then do you have to intervene and identify suicidal patients? The answer: a smartphone, daily questionnaire and a Fitbit. And hopefully some knowledgeable and meticulous data scientists.

Predictive data science has made its way as a tool to prevent suicide. Large data sets consisting of anonymized patient data combined with machine learning algorithms are being used to give patients a risk score for suicide. A computer or clinician then uses this data to identify and intervene with patients days before they act on their suicidal thoughts and behaviors. Studies on the effectiveness of the approach so far are scarce. One study on veterans using a similar approach reduced suicide attempts by 5% but [found “no significant change in the rate of suicide.”](https://www.nytimes.com/2022/09/30/health/suicide-predict-smartphone.html?unlocked_article_code=glU2-2j4AIrc9189sZqxSKLMO2UHgPAlCj0qtE-eq89RJGncz6k68HZRgVFcsbI7qAo_EOkuRdRZiiOz9mWS5GcICUTST4ZkS2sYE8ybtaVQ4ruD4VUgnb8Zf7dBuEck0Vofp9APD64Rz3awLmFoR0YZAqRkKDbxxg3sViaL_BvN4skCxHaqpQ2GbIutxECE1Nest3Q4FR8MgjAMHceTEJtkDNs7YwKHwj_ZPYfJ79buPIBmWpc2Gj_KrvBLgpeA-psHikW0DQ-a7OU-I1HFkWKlqFiuG1Sv1ZfJEZiHqVSArZcySmQN-aXrEXyVWD01gE8aHGFpMvGiD_DZoY_4amUBsA&smid=share-url)

AI and machine learning are already being used in healthcare and public health

* **Precision Medicine** or [predicting what treatment protocols are likely to succeed on a patient based on various patient attributes and the treatment context](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6616181/) is a common application. This type of model uses supervised learning, ie, the model is trained on a large dataset where the outcome variable (a specific disease) is known.
* **Pandemic** Computer algorithms were used to detect the coronavirus outbreak. [Even before the latest pandemic](https://www.cbsnews.com/news/coronavirus-outbreak-computer-algorithm-artificial-intelligence/), a company called Blue Dot spent a year teaching a computer to detect 150 deadly pathogens, tracked movement across 4 billion commercial airline passengers, used anonymized data to track the locations of millions of cellphones and read through thousands on newspapers, literally every 15 minutes.
* **COVID Variants** Even now, [researchers at MIT and Harvard have developed a machine learning tool](https://www.hsph.harvard.edu/news/hsph-in-the-news/machine-learning-tool-can-predict-which-covid-variants-will-cause-case-surges/) to predict which of the many COVID variants will cause a surge in cases. The machine learning framework they developed - called PyR0 - [analyzes over 6 million SARS-CoV-2 genomes in about an hour](https://www.genengnews.com/virology/coronavirus/which-sars-cov-2-variant-will-cause-the-next-wave-an-ai-tool-predicts/), and “determines which mutations are becoming more common and estimates how quickly each mutation can cause the virus to spread.”

But predicting which patients will commit suicide next suffers from a common data analysis problem that COVID variant researchers and precision medicine models rarely face: too little data in too little time. Psychologists are relying upon the patient, who is often times non-compliant or unwilling to share data, to give them the information required to monitor him or her. There is also a very short window between the time a patient exhibits external behaviors that indicate they will act on a suicide attempt. The algorithm must then be able to rely upon partial data, in a short period of time and make a determination that is accurate and reliable enough for a professional o intervene. “It’s probably, in some senses, not a solvable problem, for the same reason that we have school shootings and the same reason that we can’t predict a lot of this kind of stuff….You know, the math is just really daunting.” *[Source.](https://www.nytimes.com/2022/09/30/health/suicide-predict-smartphone.html?unlocked_article_code=glU2-2j4AIrc9189sZqxSKLMO2UHgPAlCj0qtE-eq89RJGncz6k68HZRgVFcsbI7qAo_EOkuRdRZiiOz9mWS5GcICUTST4ZkS2sYE8ybtaVQ4ruD4VUgnb8Zf7dBuEck0Vofp9APD64Rz3awLmFoR0YZAqRkKDbxxg3sViaL_BvN4skCxHaqpQ2GbIutxECE1Nest3Q4FR8MgjAMHceTEJtkDNs7YwKHwj_ZPYfJ79buPIBmWpc2Gj_KrvBLgpeA-psHikW0DQ-a7OU-I1HFkWKlqFiuG1Sv1ZfJEZiHqVSArZcySmQN-aXrEXyVWD01gE8aHGFpMvGiD_DZoY_4amUBsA&smid=share-url)*

But the development is still a net positive and a beacon of hope for an oftentimes hopeless endeavor. More time is needed to see how this new frontier of machine learning develops. Here’s hoping that it improves and helps saves lives.

## GarageBand Art

![](https://pbs.twimg.com/media/FKWtsgkWYAEdoyh?format=png&name=large)

The GRID from [Dan Charnas’](https://twitter.com/dancharnas) thread on [Dilla Time](https://twitter.com/dancharnas/status/1487794152179961860) showing the way music producers create events (music) spaced in time.

### [ARTICLE] Inside Garageband, the Little App Ruling the Sound of Modern Music – Rolling Stone

[Link](https://www.rollingstone.com/pro/features/apple-garageband-modern-music-784257/amp/) - [DB](https://www.dropbox.com/s/wjf6xrurl8nphxb/2022-Oct-03-%20Inside%20Garageband%2C%20the%20Little%20App%20Ruling%20the%20Sound%20of%20Modern%20Music%20%E2%80%93%20Rolling%20Stone.pdf?dl=0)

### [ARTICLE] "The click": Drummer Greg Ellis says this music production tool is the reason why all new music sounds the same

[Link](https://qz.com/1044781/this-music-production-tool-is-the-reason-why-all-new-music-sounds-the-same/) - [DB](https://www.dropbox.com/s/whse4ttqn2e7pkl/2022-Oct-03-%20%22The%20click%22%3A%20Drummer%20Greg%20Ellis%20says%20this%20music%20production%20tool%20is%20the%20reason%20why%20all%20new%20music%20soun.pdf?dl=0)

The generational tension between the old guard and the new breed of musicians over artistic barriers has been discussed so often it passed cliche miles ago. A prime example of this musical gatekeeping is the discussion over GarageBand, the user-friendly Apple app preloaded on most devices and used by millions to make a few hits (and a lot of misses) by everyone from beginners to artists like Rihanna, Usher, Nine Inch Nails and Radiohead. But with everyone having the same beginning palette, colors and canvas, do songs all start sounding the same; a [tapestry of primary colors](https://qz.com/1044781/this-music-production-tool-is-the-reason-why-all-new-music-sounds-the-same/)? And should we blame the unoriginal musicians or the industry that makes it so easy to write the next hit?

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Some of you have seen this symbol on my Twitter page, and on the Dilla Time website. ⁰⁰On the penultimate eve of the release of Dilla Time, it’s time to talk about The Triptych, and what it means… ⁰<br><br>THREAD&gt;&gt;&gt; <a href="https://t.co/MkNqqn2Qg2">pic.twitter.com/MkNqqn2Qg2</a></p>&mdash; Dan Charnas (@dancharnas) <a href="https://twitter.com/dancharnas/status/1487794082349043713?ref_src=twsrc%5Etfw">January 30, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Clearly, [technology design choices influence art and culture](https://www.artdex.com/how-technology-is-changing-the-art-world-2/).  [Research has also shown](https://www.mic.com/articles/107896/scientists-finally-prove-why-pop-music-all-sounds-the-same) that as the the complexity of music increases, sales go down and new users get turned off. [Authors of the original study note](http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0115255#pone.0115255.s001): “as music becoming increasingly formulaic in terms of instrumentation under increasing sales numbers due to a tendency to popularize music styles with low variety and musicians with similar skills." Record companies, dependent on sales and definitely not caring about artistic concepts like uniformity of sound, are well aware of this and are using computer analysis to predict which songs will be the next biggest hit. A company called [Next Big Sound claims that out of 100 top acts they identify, 20% will make the Billboard 200](https://www.theatlantic.com/magazine/archive/2014/12/the-shazam-effect/382237/?single_page=true).  Like most things, [the top 1 percent get the vast majority of the listens](https://www.rollingstone.com/pro/news/top-1-percent-streaming-1055005/): “If you were to take the more than 1.6 million artists who released music to streaming services in the past year and a half and ranked them by their total streams, you’d find that the top 16,000 of those artists pulled in 90 percent of the streams.”

But is it an objectively bad thing that everyone can make music so easily or that popular music sounds the same? Maybe all those shitty, same-sounding songs that are the 99% are the [10,000 ways that don’t work](https://due.com/blog/thomas-edison-10000-ways-that-wont-work/) before we get to the good stuff that does. And do you really want to listen to [Harry Styles](https://www.billboard.com/lists/best-songs-2022-so-far/harry-styles-as-it-was-2/) with 15 extra instruments around him playing [polyrhythmic beats](https://en.wikipedia.org/wiki/Polyrhythm) so that we can all feel justified that we aren’t basic? I say leave it *As it Was…*

### More links on this topic

* [Instrumentational Complexity of Music Genres and Why Simplicity Sells](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0115255#pone.0115255.s001)
* [The Well-Tempered Grid: On Sol LeWitt and Music](https://www.academia.edu/15372633/The_Well_Tempered_Grid_On_Sol_LeWitt_and_Music)
* [Keeping the Grid Square](https://www.fortenotation.com/en/manual/FORTE9/KeepingtheGridSquare.html)

## Doodle Everywhere

![](https://dl.dropboxusercontent.com/s/h0bkq5gt1f7gcxb/d7hftxdivxxvm.cloudfront.net%201%2C126%C3%971%2C600%20pixels-1.png?dl=0)

### [TWEET]  Mr. Doodle Doodles His House

[Link](https://twitter.com/itsmrdoodle/status/1576501179529711617?s=46&t=43Wc6Qge6SpSYA-x7d8KIg)

This [guy @itsmrdoodle](https://twitter.com/itsmrdoodle?s=21&t=43Wc6Qge6SpSYA-x7d8KIg) spent two years doodling on his house and created an incredible 2-minute video of him doing it. [According to the BBC](https://www.bbc.com/news/av/entertainment-arts-63118482) “Mr Doodle's popularity soared internationally after his videos on social media racked up millions of views, and in 2020, he was the world's fifth most successful artist aged under 40 at auction.”

Pretty amazing. Maybe I need to let the kids start drawing on the walls….

## How to Unroll a Twitter Thread

![](https://dl.dropboxusercontent.com/s/r529h7skbny3m9u/how-tweet-threads-three.png.twimg.1920.png?dl=0)

### [ARTICLE] 5 Best Ways to Save Twitter Threads in 2022 (Guide) - Beebom

[Link](https://beebom.com/save-twitter-threads/amp/) - [DB](https://www.dropbox.com/s/i9sl64lprp3cmof/2022-Oct-04-%205%20Best%20Ways%20to%20Save%20Twitter%20Threads%20in%202022%20%28Guide%29%20%7C%20Beebom.pdf?dl=0)

Twitter has turned into a microblog these days with the use of threads. “[A thread on Twitter](https://help.twitter.com/en/using-twitter/create-a-thread) is a series of connected Tweets from one person.”

Because these threads can contain so much valuable information, they are ripe for saving.  Below is a non-comprehensive list of “thread reader” apps.

### Single Purpose/Free Apps

These apps are mostly free and used for the sole purpose of unrolling the thread.

* **[Thread Reader](https://threadreaderapp.com)** is the OG in this space. Reply to a thread with ** @threadreaderapp unroll** and it will automatically reply with a link to the full thread. You can also [use the website](https://threadreaderapp.com/search) and paste the Tweet URL, a Keyword or hashtag.
* **[Twitter Bookmarks](https://help.twitter.com/en/resources/twitter-guide/topics/how-to-join-the-conversation-on-twitter/how-to-use-bookmarks-on-twitter)** makes it easy to privately save a thread to review later.
* **[Unroll Thread](https://unrollthread.com)** is a bot-only (rather than web-based) app that will let you save threads as PDFs for free by replying with **@UnrollThread** command.
* **[Threader](https://threader.app)** was acquired and rolled into Twitter Blue which requires a paid subscription.
* **[Tivitko](https://tivitiko.herokuapp.com)** is a slick little heroku app that does a nice job of unrolling tweets and saves a PDF that is nicely formatted. You can also call the bot by responding **@tivitikothread**.

### Full Featured/Paid Apps

The apps below are full-featured note taking systems (and more) that are most often paid.

* [Matter](https://hq.getmatter.com/) “Matter pulls everything you want to read into one beautiful place. With powerful tools, curation, seamless audio and more, we're building a reader for today's internet.”
* [Obsidian](https://obsidian.md/) with the [Tweet to Markdown Plugin](https://github.com/kbravh/obsidian-tweet-to-markdown)
* [Notion](https://www.notion.so/) here is a complete guide titled [“How to Save Twitter Threads to Notion”](https://www.samuelthomasdavies.com/save-twitter-threads-to-notion/)
* [Readwise](https://readwise.io) use the bot with replying **@readwiseio save** or save in the app.

### Chrome Plugins

Chrome plugins that unroll Twitter threads.

* [Twitter Print Styles](https://github.com/tannerhodges/twitter-print-styles) - does a very nice job - the tweet noted above from @dancharnas [looks like this in PDF](https://www.dropbox.com/s/x002no0vhrvi3yb/2022-10-07-17-%20Dan%20Charnas%20on%20Twitter-%20-...%20THE%20GRID-%20which%20represents%20not%20only%20the%20way%20a%20programmer%20like%20Dilla%20creates%20events%20spaced%20in%20time-%20but%20some%20other%20important%20things...%20-20%20https---t.co-KHwexxu27z-%20-.pdf?dl=0).
* [Go Full Page](https://blog.gofullpage.com/2021/02/04/the-easiest-way-to-save-and-share-tweets/)
* [Twitter Thread Reader](https://threadreader.thevarunraja.com/)

## Spooky Viewing

![](https://upload.wikimedia.org/wikipedia/commons/9/94/Hitchcock%2C_Alfred_02.jpg)

### [LIST] Spooky season viewing - Austin Kleon

[Link](https://austinkleon.com/2022/09/29/spooky-season-viewing/) - [DB](https://www.dropbox.com/s/kafnqr6tpkngask/2022-10-07-Spooky%20season%20viewing%20-%20Austin%20Kleon.pdf?dl=0)

Check out Austin Kleon's list of 31 movies to watch this spooky season.  Alfred approves!

Have a great week, y’all.

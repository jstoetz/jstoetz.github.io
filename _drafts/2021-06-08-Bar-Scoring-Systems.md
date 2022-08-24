---
layout: post
title:  "Bar Scoring Systems"
by: "Jake Stoetzner"
excerpt_separator: <!--more-->
---

## Overview/Summary
Bar scoring systems are intriguing because of their simple and elegant application to time series analysis.
## <a name="toc"></a>TOC
- [Heading 1](#h1)
  - [Sub-Heading 1.1](#h1.1)
  - [Sub-Heading 1.2](#h1.2)
  - [Sub-Heading 1.3](#h1.3)
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

## <a name="h1"></a>Bar Scoring System Considerations
<sub>[jump back to top](#toc)</sub>
To simplify the bar scoring system, it is defined as the number of consecutive positive or negative opening values over a defined period of time.  It implicitly assumes that the past price action affects (but maybe not predicts) future price action.

For example, consider the following 5-minute chart of SPY.



There are considerations to make when creating a bar scoring system.

First, and this is not readily obvious, but you need to consider the number of permutations that your will encounter based on the defined lookback period.



One fix to reduce the number of permutations is to limit them to the *actual* consecutive up or down opens rather than the *potential* consecutive up or down opens.

### <a name="h1.1"></a>About Permutations & Combinations
<sub>[jump back to top](#toc)</sub>
A permutation is the order of a sequence or [arrangement of numbers](https://www.mathsisfun.com/definitions/permutation.html).  Conversely, if the order does not matter, that is considered a combination.  For example, [if the combination to a safe is 472](https://www.mathsisfun.com/combinatorics/combinations-permutations.html) then the that is a permutation because putting in 7-2-4 or 2-4-7 will not work.  If you have a fruit salad with apples, grapes and bananas, you probably don't care that the order of each bite is grape-apple-banana (unless you are my four-year-old).  The fruit salad is a combination.

**When order doesn't matter, it is a COMBINATION.  When the order does matter, it is a PERMUTATION.** [See Combinations and Permutations on Math is Fun](https://www.mathsisfun.com/combinatorics/combinations-permutations.html).

Permutations can allow repetition of choices or not allow repetition.  

Permutations with repetition allow reuse of each number on subsequent rounds.  For example, if you have a combination lock with a 3-digit code and each digit can be any single number from 0-9, then the code *could* be 1-2-3 or 3-3-3 etc.  The number of permutations for the lock is equal to **n** different types (in this case one of 10 digits from 0-9) chosen **r** times (in this case 3 times).  The number of permutations for the lock is then 10 x 10 x 10 = 10^3 = 1,000 permutations.  

Permutations without repetition reduce the number of choices each round.  For our lock example, you could not have a code like 3-3-3 or 3-7-3.  Once each of the digits is used, it cannot be repeated.  So for the first digit of the lock code, you can use a number from 0-9 which is 10 choices.  For the second digit, you would only have 9 choices (since one of the 10-digits was chosen for the first position) and for the third round you would have 8 choices.  The number of permutations without repetition is found by multiplying 10 x 9 x 8 = 720 permutations.

Permutation with repetition is **n^r** where "n is the number of things to choose from, and we choose r of them, repetition is allowed, and order matters."

Permutation without repetition is **n!/(n-r)!** "where n is the number of things to choose from, and we choose r of them, no repetitions, [and] order matters."

Note the above section was taken from the excellent article [Combinations and Permutations on Math is Fun](https://www.mathsisfun.com/combinatorics/combinations-permutations.html).

### <a name="h1.2"></a>Permutations in R-Code
<sub>[jump back to top](#toc)</sub>
Consider the following R-code sample that defines permutations with and without repeats:

```
#-find the total number of permutations without repeating an n
fn.no.permutations.without.rep <- function(n,r){
                                    a <- factorial(n)
                                    b <- factorial(n-r)
                                    c <- a/b
                                    return(c)
                                  }
#-find the total number of permutations while repeating an n
fn.no.permutations.with.rep <- function(n,r){
                                  a <- n ^ r
                                  return(a)
                                }
```

Assume a 52-card deck of playing cards that you draw 5 cards from.  What are the total number of permutations assuming you don't put the cards drawn back into the deck?

There are 52 different types of choices (**n** value) and you can choose each 5 times (**r** value).  Since you don't replace each of the 5 cards after they are drawn, you can calculate the permutations without repetition as 52 x 51 x 50 x 49 x 48 = (52!)/(52-5)! = 52!/47! = 311,875,200.  You can double check the calculation in R:

```
factorial(52)/factorial(47)
# [1] 311875200
52*51*50*49*48
# [1] 311875200
fn.no.permutations.without.rep(52,5)
# [1] 311875200
```

If you were allowed to place the cards back in the deck after each draw, then it would be permutations with repetition.  This can be solved by 52 x 52 x 52 x 52 x 52 = 52 ^ 5 = 380,204,032.

```
52*52*52*52*52
# [1] 380204032
52^5
# [1] 380204032
fn.no.permutations.with.rep(52,5)
# [1] 380204032
```

The [gtools package](https://cran.r-project.org/web/packages/gtools/index.html) also offers a great package for returning a matrix of all of the possible permutations using the ```permutations()``` function.  [See page 15 of the gtools reference manual](https://cran.r-project.org/web/packages/gtools/gtools.pdf).  Note that for **n** values above 45, it may take a long time to return all possible permutations because of R's recursion limit.  In other words, if we tried to get all possible permutations for the card problem noted above with ```permutations(n = 52, r = 5, repeats.allowed = FALSE)``` you would likely lock up your computer because returning a matrix that is 311 million plus rows would take more computing power than you likely have!

Using a simple example of the ```permutations()``` function, find all possible permutations of the numbers 1,2,3 with **r** equal to 2 and repeats allowed:

```
# - gtools permutations() function
# Arguments
# n = Size of the source vector
# r = Size of the target vectors
# v = Source vector. Defaults to 1:n
# repeats.allowed = Logical flag indicating whether the constructed vectors may include duplicated values. Defaults to FALSE.


permutations(n = 3, r = 2, v = 1:3, repeats.allowed = FALSE)

#      [,1] [,2]
# [1,]    1    2
# [2,]    1    3
# [3,]    2    1
# [4,]    2    3
# [5,]    3    1
# [6,]    3    2
```

As an interesting side note, if you have ever shuffled a deck of cards and [you are not a mechanic](https://www.youtube.com/watch?v=N7hG6mx0csE) then it is a near certainty that you are the [only person in the history of the world](https://puzzlewocky.com/brain-teasers/probability-puzzles/every-shuffle-of-a-deck-of-cards-is-probably-unique-in-history/) to arrange the cards in that specific order.  That is because the number of possible permutations of a fair shuffle is 52! or 8.065818e+67 which is a number so astronomically large that our human brains [have trouble visualizing how large it is](https://czep.net/weblog/52cards.html).

### <a name="h1.3"></a><!--Sub-Heading 1.3-->
<sub>[jump back to top](#toc)</sub>
<!-- text-->

## <a name="h2"></a>Bar Scoring Systems
<hr />
<sub>[jump back to top](#toc)</sub>

"Bar‐scoring is an objective way to classify an instrument’s movement potential every bar. The two parts to bar‐scoring are the criterion and the resultant profit X days hence. Both the criterion and the number of days to measure profit are user‐defined."  [Building Reliable Trading Systems: Tradable Strategies That Perform as They Backtest and Meet Your Risk‐Reward Goals by Keith Fitschen, page 120](https://www.google.com/books/edition/Building_Reliable_Trading_Systems/Lh7YHl9yiAYC?hl=en&gbpv=1&dq=stock+trading+bar+scoring&pg=PT123&printsec=frontcover).  "Each bar is scored using any number of user‐defined criteria; the score is the expected value of the profit in a user‐defined number of days after entry. When the bar scores high the chances of an upward move are good; when the bar scores low the chances of a downward move are good.
Bar‐scoring can be used as a standalone entry technique, an exit technique, or as an aid to other entry criteria.""  [Building Reliable Trading Systems: Tradable Strategies That Perform as They Backtest and Meet Your Risk‐Reward Goals by Keith Fitschen, page 119](https://www.google.com/books/edition/Building_Reliable_Trading_Systems/Lh7YHl9yiAYC?hl=en&gbpv=1&dq=stock+trading+bar+scoring&pg=PT123&printsec=frontcover).  Scoring systems have also been created to measure the stock trend and volatility, incorporating market sentiment, stock sentiment, candle pattern, volume and other criteria.  [Trading by Numbers Scoring Strategies for Every Market By Rick Swope, W. Shawn Howell](https://www.google.com/books/edition/Trading_by_Numbers/Y75kYDu3028C?hl=en&gbpv=0).

Most trading systems use some type of bar-scoring.  For instance, a mean-reversion Bollinger Band

As noted above, assume for a second that the prior 20 opening prices have the ability to forecast or predict the opening price *n* bars into the future. 

Whether the open was higher or lower than the prior open determines the permutation. An open higher than the prior open will be indicated as a "1", and an open lower than the prior open is "0". 

For example, a run of 20 bars that have 5 higher opens than each prior bar, 3 lower opens, 7 higher opens and 5 lower opens would be:

```
1,1,1,1,1,0,0,0,1,1,1,1,1,1,1,0,0,0,0,0
```

I pressuppose that order matters. Two up opens followed by 10 down opens is more meaningful (in a predictive sense) than 12 alternating up and down opens or 6 down followed by 6 up.

The permutation is defined as a vector `c(0,1)` with `n = 20` and repeats allowed. 

The length of the permutation is 2.432902e+18. A vector or data.frame this long is unwieldy to work with. Also, a length of actual stock data - opening prices for 1-minute bars over a period of 2-3 days - woukd only have a fraction of the total permutations even if each length was distinct.

That leaves 2 options:
1. Solve for the actual permutations and only backtest the performance of those. 
2. Reduce the total number of permutations by combining each run in a distinctive manner.



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

### Permutation and Combinations
* [Permutations Using R -- Visual Studio Magazine](https://visualstudiomagazine.com/articles/2016/07/01/permutations-using-r.aspx)
* [Combinations and Permutations](https://www.mathsisfun.com/combinatorics/combinations-permutations.html)
* [Permutation Definition (Illustrated Mathematics Dictionary)](https://www.mathsisfun.com/definitions/permutation.html)
* [CRAN - Package gtools](https://cran.r-project.org/web/packages/gtools/index.html)
* [gtools: Various R Programming Tools](https://cran.r-project.org/web/packages/gtools/gtools.pdf)
* [52 Factorial](https://czep.net/weblog/52cards.html)
* [Each Shuffle of a Deck of Cards is Probably Unique in History – puzzlewocky](https://puzzlewocky.com/brain-teasers/probability-puzzles/every-shuffle-of-a-deck-of-cards-is-probably-unique-in-history/)

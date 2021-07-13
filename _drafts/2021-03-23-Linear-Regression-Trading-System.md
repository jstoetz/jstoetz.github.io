---
layout: post
title:  "Linear Regression Trading Systems"
by: "Jake Stoetzner"
excerpt_separator: <!--more-->
---

## Overview/Summary

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
[Link to this Post's R-Code on Github](https://github.com/jstoetz/R_Code/blob/master/Blog_Post_7.r)

## <a name="h1"></a>A Simple Overview and Example of Linear Regression
<sub>[jump back to top](#toc)</sub>
"Linear regression attempts to model the relationship between two variables by fitting a linear equation to observed data. One variable is considered to be an explanatory variable, and the other is considered to be a dependent variable."  See [Linear Regression - Yale Statistics Course](http://www.stat.yale.edu/Courses/1997-98/101/linreg.htm).  For example, you might attempt to determine the grades of a student (dependent variable) based on how much time they spent studying (the explanatory variable).

The first step before jumping into a linear regression (even though R makes the process dead simple) is to determine if there is a relationship between the explanatory variable(s) and the dependent variable.  You have no doubt heard that correlation does not mean causation, but there needs to be [some significant association between the two variables](http://www.stat.yale.edu/Courses/1997-98/101/linreg.htm).

> "A scatterplot can be a helpful tool in determining the strength of the relationship between two variables. If there appears to be no association between the proposed explanatory and dependent variables (i.e., the scatterplot does not indicate any increasing or decreasing trends), then fitting a linear regression model to the data probably will not provide a useful model. A valuable numerical measure of association between two variables is the correlation coefficient, which is a value between -1 and 1 indicating the strength of the association of the observed data for the two variables. A linear regression line has an equation of the form Y = a + bX, where X is the explanatory variable and Y is the dependent variable. The slope of the line is b, and a is the intercept (the value of y when x = 0). See [Linear Regression - Yale Statistics Course](http://www.stat.yale.edu/Courses/1997-98/101/linreg.htm).

A simple example in R of multiple linear regression [using the R function lm()](https://www.rdocumentation.org/packages/stats/versions/3.6.2/topics/lm) is helpful to set the stage.  We will use data that I have pulled from [this study](https://digitalcommons.odu.edu/cgi/viewcontent.cgi?article=1292&context=ots_masters_projects) that tried to determine the correlation between extra study time and grades for students training to be in the United States Navy.  You can [download the data here in csv format](https://www.dropbox.com/sh/m3iqxdp4fow00s8/AABP8bfoOMYuZq_GXrN4zlzca?dl=0) or see page 16 of the above study for the raw data.

I will set-up the data.frame and then divide it into in.sample and out.sample data.  I will get into this more later, but the in.sample data is used to create the model and the out.sample data is used to ```predict()``` and test the ```accuracy()``` of the model.

You can then graph the in.sample data easily with the ```plot()``` command, putting the explanatory variable first (x-axis) and the dependent variable second (y-axis).

```
grade.data <- read_csv("example1_data.csv")

grade.data.in.samp <- grade.data[1:40,]
grade.data.out.samp <- grade.data[41:83,]

plot(grade.data.in.samp$`Mins of Study`, grade.data.in.samp$Grade, xlab = "study time", ylab = "grade")
```

![](/assets/img/2021_03_23_lin_reg_chart1_exampe.png)

You can see that there is some relationship between the grade and the amount of study time.  You can see what the correlation is by running ```cor(grade.data.in.samp$Grade, grade.data.in.samp$`Mins of Study`)```.  But let's let R flex it's statistical muscle and create a linear regression for the in.sample data.

The formula is easy:  (1) specify the dependent variable (ie, what you are trying to predict), (2) the explanatory variable (the information you are using to predict the dependent variable) and (3) the data frame.  Recall we think that a student's grade can be predicted by how much study time they have put in.  In R, you use the ```lm()``` function to calculate the linear regression. A call to ```summary()``` will return the formula and the relevant accuracy information.

```
grade.model <- lm(Grade~`Mins of Study`, data = grade.data.in.samp)

coef(grade.model)
#    (Intercept) `Mins of Study`
#   93.401475913    -0.006159417

summary(grade.model)
#Call:
#lm(formula = Grade ~ `Mins of Study`, data = grade.data.in.samp)
#
#Residuals:
#     Min       1Q   Median       3Q      Max
#-12.5515  -1.9754   0.9496   2.9512   6.9811
#
#Coefficients:
#                 Estimate Std. Error t value Pr(>|t|)    
#(Intercept)     93.401476   1.038675   89.92   <2e-16 ***
#`Mins of Study` -0.006159   0.004401   -1.40     0.17    
#---
#Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
#
#Residual standard error: 4.793 on 38 degrees of freedom
#Multiple R-squared:  0.04902,	Adjusted R-squared:  0.02399
#F-statistic: 1.959 on 1 and 38 DF,  p-value: 0.1698
```

### <a name="h1.1"></a>Applying the Simple Linear Regression Model to New Data Using Predict()
<sub>[jump back to top](#toc)</sub>
<!-- text-->
Now that we have the linear regression formula, we can use the ```predict()``` function and the model we calculated in ```grade.model()``` and apply that to new data.  When I divided the original data into two sets, it was to ensure that I could measure the accuracy of the model.  "The accuracy of forecasts can only be determined by considering how well a model performs on new data that were not used when fitting the model."  See [Forecasting: Principles and Practice - 3.4 Evaluating forecast accuracy](https://otexts.com/fpp2/accuracy.html).  The "test set" or out-of-sample set should be as long as the time horizon of the forecast.  A common rule of thumb is to divide the data into 80% in-sample and 20% out-of-sample portions.  An out-of-sample portion is important to use to test the data because it avoids over-fitting the model and gives real feedback on how likely the model is to work.  You can add new or out-of-sample to the ```predict()``` function by setting the ```new data =``` parameter.

The ```predict()``` function has two other options I will draw your attention to.  The ```interval = ``` parameter specifies whether you want the confidence interval or the prediction interval.  "A prediction interval reflects the uncertainty around a single value, while a confidence interval reflects the uncertainty around the mean prediction values. Thus, a prediction interval will be generally much wider than a confidence interval for the same value." See [Predict in R: Model Predictions and Confidence Intervals](http://www.sthda.com/english/articles/40-regression-analysis/166-predict-in-r-model-predictions-and-confidence-intervals/).  Since I want to know the uncertainty surrounding a particular value (how well time spent studying predicts grades), I will set ```interval = "prediction"``` in the ```predict()``` function.  The second notable parameter is ```type= ```.  "The type="response" option tells R to output probabilities of the form P(Y = 1|X), as opposed to other information". See [4.6.2 Logistic Regression based on the book Introduction to Statistical Learning with Applications in R](http://www.science.smith.edu/~jcrouser/SDS293/labs/lab4-r.html#:~:text=The%20type%3D%22response%22%20option,fit%20the%20logistic%20regression%20model.).  In other words, setting ```type= ``` to response will return the numerical result of the prediction with columns of ```fit```, ```lwr``` and ```upr```.  See [Type parameter of the predict() function](https://stackoverflow.com/questions/23085096/type-parameter-of-the-predict-function).

```
grade.predict <- predict(grade.model, newdata = grade.data.out.samp, type = "response", interval = "prediction")

# - create a data.frame of the predicted data and the actual data for the out.of.sample data
my.grade.data <- data.frame("student" = c(41:83), "predict"=grade.predict[,1], "actual"=grade.data.out.samp$Grade, "diff"=grade.predict[,1] - grade.data.out.samp$Grade)

# - plot the predicted values for the grade (RED) vs the actual values (GREEN)
ggplot(my.grade.data, aes(x=student)) +
  geom_line(aes(y=actual), colour="green") +
  geom_line(aes(y=predict), colour="red")
```

The chart below has a plot the predicted grade (in RED) and the actual grade (in GREEN) for students.

![](/assets/img/2021_03_23_lin_reg_chart2_example.png)

### <a name="h1.2"></a>Measuring the Accuracy of the Simple Linear Regression Model
<sub>[jump back to top](#toc)</sub>
<!-- text-->
R provides an easy way to measure the accuracy of a linear regression model using the ```accuracy()``` function.  Recall that I split the data into two sets - in.sample and out.sample.  This is because "[i]n-sample fit is a notoriously poor guide to out-of-sample accuracy.  Use a holdout sample instead: keep back the last N observations, fit your models to the ones before that, forecast into the holdout sample and evaluate these forecasts." See [This Question on Time Series Forecasting in R on StackExchange](https://stats.stackexchange.com/questions/313851/time-series-forecasting-in-r).  

### <a name="h1.3"></a>A Basic Workflow for Simple Linear Regression
<sub>[jump back to top](#toc)</sub>
<!-- text-->
1. Download and clean the data.
2. Divide the data into in.sample and out.sample portions - either 80/20 or 50/50 - based on how far forward you will be projecting forward.
  + You can do this by indexing the data (ie, data[[1]][1:100,]), using the ```subset()``` function or the ```window()``` function for time-series data.
3. Run ```cor()``` on the in-sample data
4. Plot the dependent (y-axis) and explanatory (x-axis) variables for the in-sample data - look for a linear relationship.
5. Run the linear regression function ```lm()``` on the data.frame using the explanatory~dependent variable format, specifying ```data=``` the in.sample data.frame.
6. Run a ```summary()``` of the ```lm()``` model and get the ```names()``` and coefficients using the ```coef()``` function.
7. Run the ```predict``` function on the out-of-sample data, setting ```newdata=``` equal to the out-of-sample data.frame, and setting ```type= ``` to "response" and ```interval= ``` "prediction" to get the upper and lower prediction intervals.
8. Determine the accuracy of the data by creating a new dataframe with the out-of-sample predicted data and plotting the data with ```ggplot()```
9. Compare the accuracy of the model with ```accuracy()``` function.

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

* [Predict in R: Model Predictions and Confidence Intervals - Articles - STHDA](http://www.sthda.com/english/articles/40-regression-analysis/166-predict-in-r-model-predictions-and-confidence-intervals/)
* [Predicting Stock Market Returns](https://ltorgo.github.io/DMwR2/Rstocks.html#the_trading_system)
* [Other Information](https://ltorgo.github.io/DMwR2/datasets.html)
* [DMwR In Class - Predicting Stock Market Returns](https://rstudio-pubs-static.s3.amazonaws.com/299141_49076bded88d434d9a1af6593da61109.html#the-trading-set-up)
* [Stock Market Prediction with R - Stock Price Prediction Project](https://shakil-stat-bsmrstu.gitbook.io/stockproject/stock-market-prediction-with-r)
* [r - Plotting two variables as lines using ggplot2 on the same graph - Stack Overflow](https://stackoverflow.com/questions/3777174/plotting-two-variables-as-lines-using-ggplot2-on-the-same-graph)
* [Mean Reversion: Simple Trading Strategies Part 1 | by Auquan | auquan | Medium](https://medium.com/auquan/mean-reversion-simple-trading-strategies-part-1-a18a87c1196a)
* [Plot graphs in R by loop and save it like jpeg - Stack Overflow](https://stackoverflow.com/questions/32048305/plot-graphs-in-r-by-loop-and-save-it-like-jpeg)
* [Multivariate Linear Regression](http://users.stat.umn.edu/~helwig/notes/mvlr-Notes.pdf)
* [What does RMSE really mean?. Root Mean Square Error (RMSE) is a… | by James Moody | Towards Data Science](https://towardsdatascience.com/what-does-rmse-really-mean-806b65f2e48e)
* [Measuring forecast model accuracy to optimize your business objectives with Amazon Forecast | AWS Machine Learning Blog](https://aws.amazon.com/blogs/machine-learning/measuring-forecast-model-accuracy-to-optimize-your-business-objectives-with-amazon-forecast/)
* [3.4 Evaluating forecast accuracy | Forecasting: Principles and Practice (2nd ed)](https://otexts.com/fpp2/accuracy.html)
* [accuracy function - RDocumentation](https://www.rdocumentation.org/packages/forecast/versions/8.1/topics/accuracy)
* [Time Series Forecasting Performance Measures With Python](https://machinelearningmastery.com/time-series-forecasting-performance-measures-with-python/)
* [R: How to create, delete, move, and more with files - Open Source Automation](http://theautomatic.net/2018/07/11/manipulate-files-r/)
* [Python File Write](https://www.w3schools.com/python/python_file_write.asp)
* [lm function - RDocumentation](https://www.rdocumentation.org/packages/stats/versions/3.6.2/topics/lm)
* [regression - Time Series Forecasting in R - Cross Validated](https://stats.stackexchange.com/questions/313851/time-series-forecasting-in-r)
* [5.8 Nonlinear regression | Forecasting: Principles and Practice (2nd ed)](https://otexts.com/fpp2/nonlinear-regression.html)
* [3.4 Evaluating forecast accuracy | Forecasting: Principles and Practice (2nd ed)](https://otexts.com/fpp2/accuracy.html)
* [Linear Regression](http://www.stat.yale.edu/Courses/1997-98/101/linreg.htm#:~:text=A%20linear%20regression%20line%20has,y%20when%20x%20%3D%200).)
* [Quick-R: Scatterplots](https://www.statmethods.net/graphs/scatterplot.html)
* [Plot two graphs in same plot in R - Stack Overflow](https://stackoverflow.com/questions/2564258/plot-two-graphs-in-same-plot-in-r/19039094#19039094)
* [Which regression model is best for predicting/forecasting stock prices? - Quora](https://www.quora.com/Which-regression-model-is-best-for-predicting-forecasting-stock-prices)
* [Ordinary least squares - Wikipedia](https://en.wikipedia.org/wiki/Ordinary_least_squares)
* [Kalman filter - Wikipedia](https://en.wikipedia.org/wiki/Kalman_filter)
* [A Regression Model to Predict Stock Market Mega Movements and/or Volatility Using Both Macroeconomic Indicators & Fed Bank Variables](https://commons.erau.edu/cgi/viewcontent.cgi?article=1655&context=publication)
* [Stock Market Prediction with Multiple Regression, Fuzzy Type-2 Clustering and Neural Networks](https://pdf.sciencedirectassets.com/280203/1-s2.0-S1877050911X00054/1-s2.0-S1877050911005035/main.pdf)
* [5y1.org_4935f87b2ec3892af167ead7efdfb074.pdf](file:///C:/Users/Administrator/Downloads/5y1.org_4935f87b2ec3892af167ead7efdfb074.pdf)
* [Multiple Regression Analysis. Strategy Generator and Tester in One - MQL5 Articles](https://www.mql5.com/en/articles/349)
* [Using Discriminant Analysis to Develop Trading Systems - MQL5 Articles](https://www.mql5.com/en/articles/335)
* [Multiple Regression](http://rstudio-pubs-static.s3.amazonaws.com/366213_c3573a5979f74c6986e2b9178c38ea7c.html)
* [8.4 Buy and Hold | Techincal Analysis with R](https://bookdown.org/kochiuyu/Technical-Analysis-with-R/buy-and-hold-1.html)
* [NA Omit in R | 3 Examples for na.omit (Data Frame, Vector & by Column)](https://statisticsglobe.com/na-omit-r-example/)
* [TTR.pdf](https://cran.r-project.org/web/packages/TTR/TTR.pdf)
* [Logistic Regression With R](http://r-statistics.co/Logistic-Regression-With-R.html)
* [RPubs - Log-transformation using R Language](https://rpubs.com/marvinlemos/log-transformation)
* [CRAN packages for generalized linear models and with related methods](https://cran.r-project.org/web/packages/cranly/vignettes/glms.html)
* [An Introduction to `glmnet` • glmnet](https://glmnet.stanford.edu/articles/glmnet.html#assessing-models-on-test-data-1)
* [Generalized Linear Models in R, Part 1: Calculating Predicted Probability in Binary Logistic Regression - The Analysis Factor](https://www.theanalysisfactor.com/r-tutorial-glm1/)
* [r - Save multiple ggplots using a for loop - Stack Overflow](https://stackoverflow.com/questions/26034177/save-multiple-ggplots-using-a-for-loop)
* [R Compile png files into PDF | R-Toolbox](https://jonkimanalyze.wordpress.com/2014/07/24/r-compile-png-files-into-pdf/)
* [combine png files in current folder to a single png file in r - Stack Overflow](https://stackoverflow.com/questions/31732359/combine-png-files-in-current-folder-to-a-single-png-file-in-r)
* [What Are T Values and P Values in Statistics?](https://blog.minitab.com/en/statistics-and-quality-data-analysis/what-are-t-values-and-p-values-in-statistics#:~:text=The%20t%2Dvalue%20measures%20the,evidence%20against%20the%20null%20hypothesis.)
* [Predicting Stock Market Returns](https://ltorgo.github.io/DMwR2/Rstocks.html#the_prediction_models)
* [Stock Market Prediction with R - Stock Price Prediction Project](https://shakil-stat-bsmrstu.gitbook.io/stockproject/stock-market-prediction-with-r)
* [forecast.pdf](https://cran.r-project.org/web/packages/forecast/forecast.pdf)
* [forecastML Overview](https://cran.r-project.org/web/packages/forecastML/vignettes/package_overview.html)
* [time series - Forecasting several periods with machine learning - Cross Validated](https://stats.stackexchange.com/questions/346714/forecasting-several-periods-with-machine-learning)
* [arimapred: Automatic ARIMA fitting and prediction in TSPred: Functions for Benchmarking Time Series Prediction](https://rdrr.io/cran/TSPred/man/arimapred.html)
* [Home · RebeccaSalles/TSPred Wiki · GitHub](https://github.com/RebeccaSalles/TSPred/wiki)
* [A tale of two returns | Portfolio Probe | Generate random portfolios. Fund management software by Burns Statistics](https://www.portfolioprobe.com/2010/10/04/a-tale-of-two-returns/)
* [for loop - How do I add columns to a list of dataframes using paste0() in R? - Stack Overflow](https://stackoverflow.com/questions/59977253/how-do-i-add-columns-to-a-list-of-dataframes-using-paste0-in-r)
* [Linear Regression](http://www.stat.yale.edu/Courses/1997-98/101/linreg.htm)
* [2.9 - Simple Linear Regression Examples | STAT 462](https://online.stat.psu.edu/stat462/node/101/)
* [r - Predicting values using Arima model for periods that are not ahead the end of the series - Stack Overflow](https://stackoverflow.com/questions/10304924/predicting-values-using-arima-model-for-periods-that-are-not-ahead-the-end-of-th)
* [dynamic regression - 1-step-ahead predictions with dynlm R package - Cross Validated](https://stats.stackexchange.com/questions/6758/1-step-ahead-predictions-with-dynlm-r-package)
* [Predict in R: Model Predictions and Confidence Intervals - Articles - STHDA](http://www.sthda.com/english/articles/40-regression-analysis/166-predict-in-r-model-predictions-and-confidence-intervals/)
* [SDS 293 - Machine Learning](http://www.science.smith.edu/~jcrouser/SDS293/)
* [IntroToStatisticalLearning.pdf](http://www.science.smith.edu/~jcrouser/SDS293/reading/IntroToStatisticalLearning.pdf)
* [Lab 2 - Linear Regression in R](http://www.science.smith.edu/~jcrouser/SDS293/labs/lab2-r.html)
* [Lab 4 - Logistic Regression in R](http://www.science.smith.edu/~jcrouser/SDS293/labs/lab4-r.html)
* [r - Type parameter of the predict() function - Stack Overflow](https://stackoverflow.com/questions/23085096/type-parameter-of-the-predict-function)
* [r - predict.lm() in a loop. warning: prediction from a rank-deficient fit may be misleading - Stack Overflow](https://stackoverflow.com/questions/26558631/predict-lm-in-a-loop-warning-prediction-from-a-rank-deficient-fit-may-be-mis)
* [Cross-validation for time series | Rob J Hyndman](https://robjhyndman.com/hyndsight/tscv/)
* [Variations on rolling forecasts | Rob J Hyndman](https://robjhyndman.com/hyndsight/rolling-forecasts/)
* [how to use previous observations to forecast the next period using for loops in r? - Stack Overflow](https://stackoverflow.com/questions/24277055/how-to-use-previous-observations-to-forecast-the-next-period-using-for-loops-in)
* [forecasting - Forecast package in R ARIMA, what went wrong in this script? - Stack Overflow](https://stackoverflow.com/questions/50471452/forecast-package-in-r-arima-what-went-wrong-in-this-script)
* [ARIMA+GARCH Trading Strategy on the S&P500 Stock Market Index Using R | QuantStart](https://www.quantstart.com/articles/ARIMA-GARCH-Trading-Strategy-on-the-SP500-Stock-Market-Index-Using-R/)
* [21 Iteration | R for Data Science](https://r4ds.had.co.nz/iteration.html)

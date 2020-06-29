Project 2
================
Ciara Whipp
6/29/2020

## Introduction

The **OnlineNewsPopularity** data set has a total of 61 variables. There
are 2 non-predictive variables, 58 predictive variables, and 1 target
variable. The purpose of this program is to explore how well two
different types of models, a liner regression model and an ensemble
model, predict the `shares` (target) variable. To do so, we will
separate the **OnlineNewsPopularity** data set into training and testing
data sets and use the training data set to train the models. Then, we
will test the models using the testing data set and compare the accuracy
of the two models. We will automate the Rmarkdown file to repeat this
process and create a report for each day of the week.

## Data

I will use the variables
`global_subjectivity`,`global_sentiment_polarity`, `title_subjectivity`,
and `title_sentiment_polarity` to create the two predictive models.
`global_subjectivity`measures text subjectivity and `title_subjectivity`
measures title subjectivity. Both variables take on values between 0 and
1 . The variable `global_sentiment_polarity` measures text sentiment
polarity and takes on values between -0.39375 and 0.073953824.
`title_sentiment_polarity` measures title polarity and takes on values
between -1 and 1. For the polarity variables, where positive values
represent positive sentiments, negative values represent negative
sentiments, and 0 is neutral.

Read in **OnlineNewsPopularity.csv**. The data set was download from
[this
website](https://archive.ics.uci.edu/ml/datasets/Online+News+Popularity).
Keep the `share` variable, the four predictor variables mentioned above,
and the variables that indicated the weekday the article was published.
Change `shares` into a binary classification by splitting it into two
groups - \<1400 and ≥1400.

``` r
newsData <- read_csv("OnlineNewsPopularity.csv") %>% 
  select(shares, global_subjectivity, global_sentiment_polarity,
         title_subjectivity, title_sentiment_polarity, starts_with("weekday"))
newsData$shares <- ifelse(newsData$shares<1400, "<1400", "≥1400")
```

## Summarization

## Modeling

## Automation

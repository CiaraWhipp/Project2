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
polarity and takes on values between -0.39375 and 0.727840909.
`title_sentiment_polarity` measures title polarity and takes on values
between -1 and 1. For the polarity variables, where positive values
represent positive sentiments, negative values represent negative
sentiments, and 0 is neutral.

Read in **OnlineNewsPopularity.csv**. The data set was download from
[this
website](https://archive.ics.uci.edu/ml/datasets/Online+News+Popularity).
Create a new variable called `day` to indicate the day of the week the
article was publsiehd. Keep the `shares` variable, the four predictor
variables mentioned above, and the new `day` variable. Change `shares`
into a binary classification by splitting it into two groups - \<1400
and ≥1400.

``` r
newsData <- read_csv("OnlineNewsPopularity.csv") %>% 
    mutate(day=ifelse(weekday_is_monday==1, "Monday",
                ifelse(weekday_is_tuesday==1,"Tuesday",
                    ifelse(weekday_is_wednesday==1,"Wednesday",
                        ifelse(weekday_is_thursday==1,"Thursday",
                            ifelse(weekday_is_friday==1,"Friday",
                                ifelse(weekday_is_saturday==1,"Saturday",
                                    "Sunday"))))))) %>%
  select(shares, global_subjectivity, global_sentiment_polarity,
         title_subjectivity, title_sentiment_polarity, day)
newsData$shares <- ifelse(newsData$shares<1400, "<1400", "≥1400")
```

Split the **newsData** data set into training and testing data sets.

``` r
set.seed(1)
train <- sample(1:nrow(newsData), size=nrow(newsData)*0.7)
test <- dplyr::setdiff(1:nrow(newsData), train)

newsDataTrain <- newsData[train, ]
nwsDataTest <- newsData[test, ]
```

## Summarization

### Summary Statistics

The summary statistics tables for the training data give the minimum,
1st quantile, median, mean, 3rd quantile, and maximum for
`global_subjectivity`,`global_sentiment_polarity`, `title_subjectivity`,
and `title_sentiment_polarity` for \<1400 shares and ≥1400 shares
respectively.

``` r
newsDataLt <- newsDataTrain %>%
  filter(shares=="<1400") %>%
  select(global_subjectivity, global_sentiment_polarity,
         title_subjectivity, title_sentiment_polarity)
knitr::kable(apply(newsDataLt,2,summary), digits=2,
             caption="Summary Statisitics for <1400 Shares in Training Data")
```

|         | global\_subjectivity | global\_sentiment\_polarity | title\_subjectivity | title\_sentiment\_polarity |
| ------- | -------------------: | --------------------------: | ------------------: | -------------------------: |
| Min.    |                 0.00 |                      \-0.39 |                0.00 |                     \-1.00 |
| 1st Qu. |                 0.38 |                        0.05 |                0.00 |                       0.00 |
| Median  |                 0.44 |                        0.11 |                0.10 |                       0.00 |
| Mean    |                 0.43 |                        0.11 |                0.27 |                       0.05 |
| 3rd Qu. |                 0.50 |                        0.17 |                0.50 |                       0.14 |
| Max.    |                 1.00 |                        0.73 |                1.00 |                       1.00 |

Summary Statisitics for \<1400 Shares in Training Data

``` r
newsDataGt <- newsDataTrain %>%
  filter(shares=="≥1400") %>%
  select(global_subjectivity, global_sentiment_polarity,
         title_subjectivity, title_sentiment_polarity)
knitr::kable(apply(newsDataGt,2,summary), digits=2,
             caption="Summary Statisitics for ≥1400 Shares in Training Data")
```

|         | global\_subjectivity | global\_sentiment\_polarity | title\_subjectivity | title\_sentiment\_polarity |
| ------- | -------------------: | --------------------------: | ------------------: | -------------------------: |
| Min.    |                 0.00 |                      \-0.38 |                0.00 |                     \-1.00 |
| 1st Qu. |                 0.41 |                        0.07 |                0.00 |                       0.00 |
| Median  |                 0.46 |                        0.13 |                0.17 |                       0.00 |
| Mean    |                 0.45 |                        0.13 |                0.29 |                       0.08 |
| 3rd Qu. |                 0.51 |                        0.18 |                0.50 |                       0.20 |
| Max.    |                 1.00 |                        0.65 |                1.00 |                       1.00 |

Summary Statisitics for ≥1400 Shares in Training Data

## Modeling

## Automation

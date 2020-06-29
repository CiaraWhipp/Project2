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

## Summarization

### Summary Statistics

The summary statistics tables below give the minimum, 1st quantile,
median, mean, 3rd quantile, and maximum for
`global_subjectivity`,`global_sentiment_polarity`, `title_subjectivity`,
and `title_sentiment_polarity` for \<1400 shares and ≥1400 shares
respectively.

``` r
newsDataLt <- newsData %>%
  filter(shares=="<1400") %>%
  select(global_subjectivity, global_sentiment_polarity,
         title_subjectivity, title_sentiment_polarity)
knitr::kable(apply(newsDataLt,2,summary), 
             caption="Summary Statisitics for <1400 Shares")
```

|         | global\_subjectivity | global\_sentiment\_polarity | title\_subjectivity | title\_sentiment\_polarity |
| ------- | -------------------: | --------------------------: | ------------------: | -------------------------: |
| Min.    |            0.0000000 |                 \-0.3937500 |           0.0000000 |                \-1.0000000 |
| 1st Qu. |            0.3847431 |                   0.0479154 |           0.0000000 |                  0.0000000 |
| Median  |            0.4443753 |                   0.1091337 |           0.1000000 |                  0.0000000 |
| Mean    |            0.4348761 |                   0.1116841 |           0.2711116 |                  0.0562428 |
| 3rd Qu. |            0.5002723 |                   0.1706262 |           0.5000000 |                  0.1363636 |
| Max.    |            1.0000000 |                   0.7278409 |           1.0000000 |                  1.0000000 |

Summary Statisitics for \<1400 Shares

``` r
newsDataGt <- newsData %>%
  filter(shares=="≥1400") %>%
  select(global_subjectivity, global_sentiment_polarity,
         title_subjectivity, title_sentiment_polarity)
knitr::kable(apply(newsDataGt,2,summary), 
             caption="Summary Statisitics for ≥1400 Shares")
```

|         | global\_subjectivity | global\_sentiment\_polarity | title\_subjectivity | title\_sentiment\_polarity |
| ------- | -------------------: | --------------------------: | ------------------: | -------------------------: |
| Min.    |            0.0000000 |                 \-0.3802083 |           0.0000000 |                 \-1.000000 |
| 1st Qu. |            0.4063632 |                   0.0678053 |           0.0000000 |                   0.000000 |
| Median  |            0.4611443 |                   0.1267740 |           0.1833333 |                   0.000000 |
| Mean    |            0.4507946 |                   0.1259742 |           0.2921791 |                   0.084696 |
| 3rd Qu. |            0.5148097 |                   0.1835754 |           0.5000000 |                   0.200000 |
| Max.    |            1.0000000 |                   0.6550000 |           1.0000000 |                   1.000000 |

Summary Statisitics for ≥1400 Shares

## Modeling

## Automation

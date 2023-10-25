# SP500 Prediction

## Table of Contents
* [Introduction](#introduction)
* [Data Overview](#data-overview)
* [Machine Learning](#machine-learning)
* [Results](#results)
* [Conclusion](#conclusion)

## Introduction

The stock market is unpredictable and making money requires a lot of thought on the movement of the market. As we want to make money, we only want to buy stock when the price goes up. We'll create a machine learning algorithm to predict if the stock price will increase. If the algorithm says that the price will increase, we'll buy stock. Otherwise, we will not purchase any stock.

## Data Overview

The data we used to perform this analysis was obtained by using the `yfinance` python package, the dataset can be viewed **[here](https://github.com/jidafan/SP500-Prediction/blob/main/sp500.csv)**. The yfinance package takes data from the Yahoo Finance website and imports it into python.

As we can see below, we have one row of data for each day the SP500 stock was traded.  Here are the columns:
| Variable      | Description           | 
| ------------- |:---------------------| 
| `open`     | The price the stock opened at    |
| `high`    | The highest price of the stock during the day          |   
| `low` | The lowest price of the stock during the day                                         |
| `close`    | The price the stock closed at          |   
| `volume` | How many shares were traded throughout the day                                       |

The row index of the DataFrame is the date the stock was traded.  The stock market doesn't operate every day, so some dates are missing due to holidays and weekends.
![image](https://github.com/jidafan/SP500-Prediction/assets/141703009/efd5dc8f-b188-4b46-a9d9-e84886939bfe)

## Machine Learning

We will implement machine learning to predict whether the stock price of the SP500 will increase or decrease. First, we'll identify a target that we're trying to predict.  Our target will be if the price will go up or down tomorrow. If the price went up, the target will be `1` and if it went down, the target will be `0`.

As this is time series data, we will not be able to use cross-validation to create predictions for the whole dataset. Instead, we'll split the data sequentially. We'll start off by predicting just the last 100 rows using the other rows.
We will implement a random forest classifier to generate our predictions, because it can pick up nonlinear relationships in the data, and helps to prevent overfitting.

To test the accuracy of our predictions, we will import precision_score from the scikit-learn package.

![image](https://github.com/jidafan/SP500-Prediction/assets/141703009/95e30edd-5aa4-43cc-9779-ddfa8bdc8689)

## Results 

After reviewing the results from the random forest model, to improve the accuracy of our predictions, we will create a backtesting engine. Backtesting ensures that we only use data from before the day that we're predicting. If we use data from after the day we're predicting, the algorithm will not be applicable to the real world.

![image](https://github.com/jidafan/SP500-Prediction/assets/141703009/f09623d9-f418-4b06-95d1-869bd70f3be4)

Seeing how just implementing a backtest engine does not improve our accuracy to something that we are satisfied with. We will create a new set of predictors by creating some rolling averages, so the model can evaluate the current price against recent prices.  We'll also look at the ratios between different indicators. We also tweak the predictions to probabilities, meaning that if we believe that there is a 65% chance of the stock increasing we will consider that a `1`, if it is below that percentage we will consider it a `0`.

![image](https://github.com/jidafan/SP500-Prediction/assets/141703009/0143444b-a318-43bc-965b-e834ea5b5c7a)


## Conclusion

### Next steps

There are a lot of next steps we could take to improve our predictions to achieve a higher accuracy score:

### Improve the technique

* Calculate the money that could've have been made if we followed the algorithm 

### Improve the algorithm

* Try discarding older data (only keeping data in a certain window)
* Try a different machine learning algorithm
* Tweak random forest parameters, or the prediction threshold

### Add in more predictors

* Account for activity post-close and pre-open
    * Early trading
    * Trading on other exchanges that open before the NYSE (to see what the global sentiment is)
* Economic indicators
    * Interest rates
    * Other important economic news
* Company milestones
    * Earnings calls
    * Analyst ratings
    * Major announcements
* Prices of related stocks
    * Other companies in the same sector
    * Key partners, customers, etc.

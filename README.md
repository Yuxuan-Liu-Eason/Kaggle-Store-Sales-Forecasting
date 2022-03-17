# Kaggle-Store-Sales-Forecasting

https://www.kaggle.com/c/store-sales-time-series-forecasting

This is a share of my solution for the above competition on Kaggle (40/860 on leaderboard). The goal is to use holiday, oil price and promotion information to predict future sales.

## EDA

The EDA mainly uses plotly. Below is the trend of average sales.

![image](https://user-images.githubusercontent.com/76148884/150659974-957b40b6-5943-4615-bb42-a4b8c4d554aa.png)

Sales and holiday:

![image](https://user-images.githubusercontent.com/76148884/150660017-2ec6c565-e5ed-4257-8563-b6595b71b9b3.png)

## Feature Engineering

- Only keep national holidays for simplicity. Drop duplicated holidays.
- Rolling 7 days oil price, oil price lag1, 2, 3.
- Deterministic process (sin and cos)
- Date features: month, day, day of week
- If is school day (month in 3, 4 and 8, 9). This feature is important to one special category "school and office supply".
- Label if is workday. (Not work if is weekends or holidays)

## Modeling

The idea of seperate training using ridge and random forest is from https://www.kaggle.com/dkomyagin/simple-ts-ridge-rf by KDJ2020(@dkomyagin).

- Only select most recent 4 months as training set.
- During the error analysis, I found one product category "SCHOOL AND OFFICE SUPPLIES" has large errors. So I use random forest to train this category. And use ridge regression for the rest categories.
- I also tried lightgbm with optuna parameter tunning for all categories. However, the final result is not as good as the above approach.

Validation set:

![image](https://user-images.githubusercontent.com/76148884/150660390-5c8552b8-d3a7-4840-9365-bd9e2fc7d244.png)

The changllenging aspects about this competition is that there are many stores and products to predict. Each store might have different patterns or rules. That is why I used multioutput models. Another problem is that the special category "SCHOOL AND OFFICE SUPPLIES" is hard to predict even with special training.
